---
title: "Azure Cognitive Services Computer Vision API to analyze images for SharePoint Online"
date: "2020-04-23"
---

## Overview

As we upload images to SharePoint, we should be able to analyze them and attach corresponding metadata to it. This metadata will be helpful in the future to search the images by keyword as well as use related images when needed while content creation. We should be able to extract the key information about the image such as description, picture taken date, location of image, and objects present in an image, etc. This information then can be tagged in metadata columns inside the picture / document library for ease of searching and filtering.

## The Solution

As of today, SharePoint Online does not support this functionality out of the box. However, we can leverage the below components to develop a low code / no-code solution.

1. **Azure Cognitive service:** Computer Vision API: This helps to identify the image content, classify the image, and categorize it.
2. **Power Automate (MS Flow):** This helps to integrate Computer Vision API for analyzing images uploaded to SharePoint.

The high-level implementation will be as follows:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-3.png)

## Set up Computer Vision API

Let us start by building our first building block in MS Azure Cognitive Services by creating Computer Vision API. Follow the below steps to set up Computer Vision API.

**Create a resource**

1. Login to Microsoft Azure Portal ([https://portal.azure.com](https://portal.azure.com)).
2. Click **Create a resource**.
3. Select **AI + Machine Learning**
4. Select **Computer Vision**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-4.png)

**Create a Computer Vision**

1. Fill in the details to create the computer vision.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-5.png)

The parameters are as follows:

- **Name:** Unique name of computer vision.
- **Subscription:** Select Azure subscription to create the computer vision service under it.
- **Location:** The location to host computer vision service.
- **Pricing tier:** Select tier as per features you want to use. More details at [https://azure.microsoft.com/en-us/pricing/details/cognitive-services/computer-vision/](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/computer-vision/)
- **Resource group:** Select an existing or create a new resource group.

1. Click **Create**.
2. Once the deployment finishes, the computer vision resource will be available to use.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-6.png)

1. Note down the API Key and endpoint to authenticate your applications and start sending calls to the service.

## Test the Computer Vision

We will use the API Console to quickly try the API without writing any code.

1. Click **API Console** under step 2 from the above image.
2. Select the testing console in the region where you created your resource.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-7.png)

1. The selection of the region will form the request URL.
2. Specify the host and query parameters.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-8.png)

1. Specify the API key in Headers.
2. In the request body, provide the URL to the image.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-9.png)

1. The HTTP request will be composed as follows.
2. Click Send.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-10.png)

1. The response will be shown as follows:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-11.png)

## Set up Power Automate Flow.

Now we will set up Power Automate flow to integrate Computer Vision API for analyzing images uploaded to SharePoint.

Follow the below steps to set up Power Automate flow:

1. Open Power Automate portal.
2. Create an **Automated flow**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-12.png)

1. Name the flow.
2. Choose the flow’s trigger as “When a file is created in a folder”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-13.png)

1. Click **Create**.
2. Specify the site address and folder Id.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-14.png)

1. Click “+ New step”.
2. Find the connector “Computer Vision API”.
3. Select action as “Analyze Image (preview)”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-15.png)

1. Configure the “Computer Vision API” connection.
2. Specify the name for the connection, cognitive services account key and endpoint.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-16.png)

1. Click **Create**.
2. Specify the image source and content to analyze the image.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-17.png)

1. Upload an image in the SharePoint library. The flow will run and we will have an output like below:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-18.png)

## Update Metadata to SharePoint

We will have to parse the response and store the metadata to SharePoint.

1. Add an action "Send an HTTP request to SharePoint" to get to the file to update the properties.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-19.png)

- Site address: Specify SharePoint site url
- Method: Get
- Uri: /\_api/web/getFileByServerRelativeUrl('File Path')?$select=ListItemAllFields/ID&$expand=ListItemAllFields
- Headers: key: accept, value: application/json | key: content-type, value: application/json

1. Run the flow and get the Body from the output of "Send an HTTP request to SharePoint".

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-20.png)

1. Add an action “Parse JSON”. Specify the content as Body.
2. Generate the schema from a sample of previous runs.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-21.png)

1. Add an action “Update file properties”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-22.png)

Update our custom “Cognitive Tags” field with tags using the expression: join(body('Analyze\_Image')?\['description'\]?\['tags'\],',')

## Test the Solution

In the SharePoint library, upload an image. This will trigger a flow. A flow will call Computer Vision API for analyzing images uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/word-image-23.png)

## Summary

Azure Cognitive Services Computer Vision API helps to analyze images uploaded to SharePoint by extracting the key information about the image such as description, picture taken date, location of image, and objects present in an image, etc. This information then can be tagged in metadata columns inside the picture / document library for ease of searching and filtering.

## References

- [https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/#features](https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/#features)
- [https://docs.microsoft.com/en-in/azure/cognitive-services/](https://docs.microsoft.com/en-in/azure/cognitive-services/)
