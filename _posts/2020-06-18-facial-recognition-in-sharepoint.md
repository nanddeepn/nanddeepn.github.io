---
title: "Embed facial recognition into SharePoint"
date: "2020-06-18"
---

## Overview

Face API is an AI service that analyses faces in images. Face API is one of the useful offerings of MS Azure Cognitive Services. The Face API perceives faces and attributes in an image. It recognizes the range of facial expressions like happiness, contempt, neutrality, and fear; and recognition and grouping of similar faces in images.

In this article, we will have an overview of the Face API, build, and use it in our applications.

## Set up Face API in MS Azure

Let us start by building our first building block in MS Azure Cognitive Services by Face API. Follow the below steps to set up Face API.

**Create a resource**

1. Login to Microsoft Azure Portal ([https://portal.azure.com](https://portal.azure.com)).
2. Click **Create a resource**.
3. Select **AI + Machine Learning**.
4. Select **Face**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-12.png)

**Create a Face API**

1. Fill in the details to create the Face API.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-13.png)

The parameters are as follows:

- **Name:** Unique name of Face API.
- **Subscription:** Select Azure subscription to create Face API under it.
- **Location:** The location to host Face API.
- **Pricing tier:** Select tier as per features you want to use. More details at [https://azure.microsoft.com/en-us/pricing/details/cognitive-services/face-api/](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/face-api/)
- **Resource group:** Select an existing or create a new resource group.

1. Click **Create**.
2. Once the deployment finishes, the Face API resource will be available to use.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-14.png)

1. Note down the API Key and endpoint to authenticate your applications and start sending calls to the service.

## Test the Face API

We will use the API Console to quickly try the API without writing any code.

1. From the left menu, select **Quick start**.
2. Click **API Console**.
3. Select the testing console in the region where you created your resource.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-15.png)

1. The selection of the region will form the request URL.
2. Specify the resource name and query parameters.
3. Specify the API key in Headers.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-16.png)

1. In the request body, provide the URL of the image containing Face.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-17.png)

1. Click **Send**.
2. The response will be shown as follows:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-18.png)

## Set up SharePoint Library

Create a SharePoint library with below schema:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-19.png)

## Set up Power Automate Flow

Now we will set up Power Automate flow to integrate Face API for analyzing an image uploaded to the SharePoint library.

Follow the below steps to set up Power Automate flow:

1. On the SharePoint library, click **Automate** > **Power Automate** > **Create a flow**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-20.png)

1. Select a template “When a new file is added in SharePoint, complete a custom”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-21.png)

1. Verify your connection to SharePoint.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-22.png)

1. Click **Continue**.
2. Add an activity “Face API”, select action “Detect faces (preview)”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-23.png)

1. Specify the Connection Name, API Key, and Site URL from MS Azure Face API service.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-24.png)

1. Click **Create**.
2. Specify the **Image Url** as **Link to item**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-25.png)

## The Challenge

Now, if you run the flow, it fails with below error:

BadRequest: Invalid image URL or error downloading from the target server. Remote server error returned: "Response status code does not indicate success: 403 (FORBIDDEN)."

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-26.png)

The reason is Face API in MS Azure subscription is not able to connect to SharePoint which is in another domain.

## The Solution

We will create a storage account inside MS Azure to temporarily hold the image uploaded to SharePoint.

1. In the MS Azure portal, click **Create a resource**.
2. Under **Storage**, click **Storage account - blob, file, table, queue**.
3. Enter the name, subscription, Location, and the Access tier.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-27.png)

1. Click **Create**.
2. Once the storage account is ready, create the container inside it.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-28.png)

1. Click **Create**.

## Return to Flow

Now let’s use this container as temporary storage for uploaded images to the SharePoint library.

1. Get the file content to upload to the container.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-29.png)

1. Add an activity “Azure Blob Storage”, select action “Create blob”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-30.png)

1. Specify the connection name, Storage account name, and Storage account access key from MS Azure.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-31.png)

1. In the Create blob, specify container name as folder path, file name as Blob name, and file content as the Blob content.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-32.png)

1. Add an action “Detect faces (Preview)”. Compose the Image Url as the URL of the storage account + Path to the uploaded file.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-33.png)

1. Add an action “Update file properties”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-34.png)

## Test the Solution

In the SharePoint library, upload a human image. This will trigger a flow. A flow will call Face API for analyzing the image uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library with gender and age.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-35.png)

But I am just 21! 😊

## Summary

Azure Face API analyses faces in images. It recognizes the range of facial expressions like happiness, contempt, neutrality, and fear; and recognition and grouping of similar faces in images.

## Use Cases

The Face API can be useful in the below scenarios:

- Find missing persons
- Recognize VIPs

## References

- Face recognition ([https://azure.microsoft.com/en-in/services/cognitive-services/face/](https://azure.microsoft.com/en-in/services/cognitive-services/face/))
- Face API Pricing ([https://azure.microsoft.com/en-in/pricing/details/cognitive-services/face-api/](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/face-api/))
