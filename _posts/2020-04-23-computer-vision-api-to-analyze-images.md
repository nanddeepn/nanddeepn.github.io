---
title: "Azure Cognitive Services Computer Vision API to analyze images for SharePoint Online"
date: "2020-04-23"
share: true
header:
  image: media/2020-04-23-computer-vision-api-to-analyze-images/21.png
  teaser: media/2020-04-23-computer-vision-api-to-analyze-images/21.png
categories:
  - AI
  - Computer Vision API
  - SharePoint
  - Azure Cognitive Services
  - Power Automate
tags:
  - "2020"
  - April 2020
last_modified_at: 2020-04-23T00:00:00-00:00
---

## Overview

As we upload images to SharePoint, we should be able to analyze them and attach corresponding metadata to it. This metadata will be helpful in the future to search the images by keyword as well as use related images when needed while content creation. We should be able to extract the key information about the image such as description, picture taken date, location of image, and objects present in an image, etc. This information then can be tagged in metadata columns inside the picture / document library for ease of searching and filtering.

## The Solution

As of today, SharePoint Online does not support this functionality out of the box. However, we can leverage the below components to develop a low code / no-code solution.

1. **Azure Cognitive Service (Computer Vision API):** This helps to identify the image content, classify the image, and categorize it.
2. **Power Automate (MS Flow):** This helps to integrate Computer Vision API for analyzing images uploaded to SharePoint.

The high-level implementation will be as follows:

![](/media/2020-04-23-computer-vision-api-to-analyze-images/01.png)

## Set up Computer Vision API

Let us start by building our first building block in MS Azure Cognitive Services by creating Computer Vision API. Follow the below steps to set up Computer Vision API.

**Create a resource**

1. Login to Microsoft Azure Portal ([https://portal.azure.com](https://portal.azure.com)).
2. Click **Create a resource**.
3. Select **AI + Machine Learning**
4. Select **Computer Vision**.

![](/media/2020-04-23-computer-vision-api-to-analyze-images/02.png)

**Create a Computer Vision**

1. Fill in the details to create the computer vision.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/03.png)

    The parameters are as follows:
    - **Name:** Unique name of computer vision.
    - **Subscription:** Select Azure subscription to create the computer vision service under it.
    - **Location:** The location to host computer vision service.
    - **Pricing tier:** Select tier as per features you want to use. More details at [https://azure.microsoft.com/en-us/pricing/details/cognitive-services/computer-vision/](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/computer-vision/)
    - **Resource group:** Select an existing or create a new resource group.

2. Click **Create**.
3. Once the deployment finishes, the computer vision resource will be available to use.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/04.png)

4. Note down the API Key and endpoint to authenticate your applications and start sending calls to the service.

## Test the Computer Vision

We will use the API Console to quickly try the API without writing any code.

1. Click **API Console** under step 2 from the above image.
2. Select the testing console in the region where you created your resource.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/05.png)

3. The selection of the region will form the request URL.
4. Specify the host and query parameters.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/06.png)

5. Specify the API key in Headers.
6. In the request body, provide the URL to the image.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/07.png)

7. The HTTP request will be composed as follows.
8. Click **Send**.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/08.png)

9. The response will be shown as follows:

![](/media/2020-04-23-computer-vision-api-to-analyze-images/09.png)


## Set up Power Automate Flow.

Now we will set up Power Automate flow to integrate Computer Vision API for analyzing images uploaded to SharePoint.

Follow the below steps to set up Power Automate flow:

1. Open Power Automate portal.
2. Create an **Automated flow**.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/10.png)

3. Name the flow.
4. Choose the flow’s trigger as "When a file is created in a folder".

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/11.png)

5. Click **Create**.
6. Specify the site address and folder Id.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/12.png)

7. Click **+ New step**.
8. Find the connector **Computer Vision API**.
9. Select action as **Analyze Image (preview)**.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/13.png)

10. Configure the **Computer Vision API** connection.
11. Specify the name for the connection, cognitive services account key and endpoint.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/14.png)

12. Click **Create**.
13. Specify the image source and content to analyze the image.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/15.png)

14. Upload an image in the SharePoint library. The flow will run and we will have an output like below:

![](/media/2020-04-23-computer-vision-api-to-analyze-images/16.png)


## Update Metadata to SharePoint

We will have to parse the response and store the metadata to SharePoint.

1. Add an action "Send an HTTP request to SharePoint" to get to the file to update the properties.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/17.png)

    - Site address: Specify SharePoint site url
    - Method: Get
    - Uri: /_api/web/getFileByServerRelativeUrl('File Path')?$select=ListItemAllFields/ID&$expand=ListItemAllFields
    - Headers: key: accept, value: application/json | key: content-type, value: application/json

2. Run the flow and get the Body from the output of "Send an HTTP request to SharePoint".

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/18.png)

3. Add an action **Parse JSON**. Specify the content as Body.
4. Generate the schema from a sample of previous runs.

    ![](/media/2020-04-23-computer-vision-api-to-analyze-images/19.png)

5. Add an action **Update file properties**.

![](/media/2020-04-23-computer-vision-api-to-analyze-images/20.png)

Update our custom "Cognitive Tags" field with tags using the expression: join(body('Analyze_Image')?['description']?['tags'],',')


## Test the Solution

In the SharePoint library, upload an image. This will trigger a flow. A flow will call Computer Vision API for analyzing images uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library.

![](/media/2020-04-23-computer-vision-api-to-analyze-images/21.png)


## Summary

Azure Cognitive Services Computer Vision API helps to analyze images uploaded to SharePoint by extracting the key information about the image such as description, picture taken date, location of image, and objects present in an image, etc. This information then can be tagged in metadata columns inside the picture / document library for ease of searching and filtering.

## References

- [https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/#features](https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/#features)
- [https://docs.microsoft.com/en-in/azure/cognitive-services/](https://docs.microsoft.com/en-in/azure/cognitive-services/)
