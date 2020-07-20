---
title: "Embed facial recognition into SharePoint"
date: "2020-06-18"
share: true
header:
  image: media/2020-06-18-facial-recognition-in-sharepoint/24.png
  teaser: media/2020-06-18-facial-recognition-in-sharepoint/24.png
#   caption: "Photo credit: [**Google**](http://www.google.com)"
#   actions:
#     - label: "Click to see full image"
#       url: "media/2020-06-18-facial-recognition-in-sharepoint/24.png"
categories:
  - AI
  - Face API
  - SharePoint
  - Azure Cognitive Services
  - Power Automate
tags:
  - "2020"
  - June 2020
last_modified_at: 2020-06-18T00:00:00-00:00
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

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/01.png)

**Create a Face API**

1. Fill in the details to create the Face API.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/02.png)

    The parameters are as follows:
    
    - **Name:** Unique name of Face API.
    - **Subscription:** Select Azure subscription to create Face API under it.
    - **Location:** The location to host Face API.
    - **Pricing tier:** Select tier as per features you want to use. More details at [https://azure.microsoft.com/en-us/pricing/details/cognitive-services/face-api/](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/face-api/)
    - **Resource group:** Select an existing or create a new resource group.

2. Click **Create**.
3. Once the deployment finishes, the Face API resource will be available to use.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/03.png)

4. Note down the API Key and endpoint to authenticate your applications and start sending calls to the service.

## Test the Face API

We will use the API Console to quickly try the API without writing any code.

1. From the left menu, select **Quick start**.
2. Click **API Console**.
3. Select the testing console in the region where you created your resource.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/04.png)

4. The selection of the region will form the request URL.
5. Specify the resource name and query parameters.
6. Specify the API key in Headers.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/05.png)

7. In the request body, provide the URL of the image containing Face.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/06.png)

8. Click **Send**.
9. The response will be shown as follows:

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/07.png)

## Set up SharePoint Library

Create a SharePoint library with below schema:

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/08.png)

## Set up Power Automate Flow

Now we will set up Power Automate flow to integrate Face API for analyzing an image uploaded to the SharePoint library.

Follow the below steps to set up Power Automate flow:

1. On the SharePoint library, click **Automate** > **Power Automate** > **Create a flow**.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/09.png)

2. Select a template “When a new file is added in SharePoint, complete a custom”.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/10.png)

3. Verify your connection to SharePoint.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/11.png)

4. Click **Continue**.
5. Add an activity “Face API”, select action “Detect faces (preview)”.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/12.png)

6. Specify the Connection Name, API Key, and Site URL from MS Azure Face API service.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/13.png)

7. Click **Create**.
8. Specify the **Image Url** as **Link to item**.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/14.png)

## The Challenge

Now, if you run the flow, it fails with below error:

BadRequest: Invalid image URL or error downloading from the target server. Remote server error returned: "Response status code does not indicate success: 403 (FORBIDDEN)."

![](/media/2020-06-18-facial-recognition-in-sharepoint/15.png)

The reason is Face API in MS Azure subscription is not able to connect to SharePoint which is in another domain.

## The Solution

We will create a storage account inside MS Azure to temporarily hold the image uploaded to SharePoint.

1. In the MS Azure portal, click **Create a resource**.
2. Under **Storage**, click **Storage account - blob, file, table, queue**.
3. Enter the name, subscription, Location, and the Access tier.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/16.png)

4. Click **Create**.
5. Once the storage account is ready, create the container inside it.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/17.png)

6. Click **Create**.

## Return to Flow

Now let’s use this container as temporary storage for uploaded images to the SharePoint library.

1. Get the file content to upload to the container.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/18.png)

2. Add an activity “Azure Blob Storage”, select action “Create blob”.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/19.png)

3. Specify the connection name, Storage account name, and Storage account access key from MS Azure.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/20.png)

4. In the Create blob, specify container name as folder path, file name as Blob name, and file content as the Blob content.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/21.png)

5. Add an action “Detect faces (Preview)”. Compose the Image Url as the URL of the storage account + Path to the uploaded file.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/22.png)

6. Add an action “Update file properties”.

    ![](/media/2020-06-18-facial-recognition-in-sharepoint/23.png)

## Test the Solution

In the SharePoint library, upload a human image. This will trigger a flow. A flow will call Face API for analyzing the image uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library with gender and age.

![](/media/2020-06-18-facial-recognition-in-sharepoint/24.png)

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

[![HitCount](http://hits.dwyl.com/nanddeepn/nanddeepngithubio/posts/2020-06-18-facial-recognition-in-sharepoint/.svg)](http://hits.dwyl.com/nanddeepn/nanddeepngithubio/posts/2020-06-18-facial-recognition-in-sharepoint/)
