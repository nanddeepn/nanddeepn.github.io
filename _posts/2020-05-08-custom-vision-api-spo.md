---
title: "Azure Cognitive Services: Custom Vision API for better image prediction in SharePoint Online"
date: "2020-05-08"
share: true
categories:
  - AI
  - Custom Vision API
  - SharePoint
  - Azure Cognitive Services
  - Power Automate
header:
  image: media/2020-05-08-custom-vision-api-spo/28.png
  teaser: media/2020-05-08-custom-vision-api-spo/28.png
tags:
  - "2020"
  - May 2020
last_modified_at: 2020-05-08T00:00:00-00:00
---

## Overview

In the previous article, we explored the [Computer Vision API](/posts/2020-06-18-facial-recognition-in-sharepoint/). The computer vision API offers services to detect commonly known objects. For example, when you upload an image of a car, it will correctly identify it as a car. However, in certain circumstances, you want to identify a few objects or scenarios which computer vision API does not recognize. Custom vision API becomes handy in these scenarios.

In this article, we will have an overview of the Custom Vision API, build, and use it in our applications.

## Custom Vision API

Custom vision API is also trained by Microsoft to identify common objects and scenarios. On top of it, we can also train the Custom vision service for specific things we want to recognize ourselves.

Custom vision service follows below simple principal.

![](/media/2020-05-08-custom-vision-api-spo/01.png)

## Custom Vision API in MS Azure

Custom Vision API has a separate key for prediction and training.

**Pricing:**

- Pricing for prediction goes per 1000 transactions.
- Pricing for training goes per computer hour.
- Image storage is priced per 1000 images.

**Set up Custom Vision API**

Follow the below steps to set up the custom vision API in MS Azure.

1. Open MS Azure portal ([https://portal.azure.com/](https://portal.azure.com/))
2. Click **Create a resource**.
3. Search for **Custom vision** in the marketplace.

    ![](/media/2020-05-08-custom-vision-api-spo/02.png)

3. Click **Create**.

    ![](/media/2020-05-08-custom-vision-api-spo/03.png)

    - Choose the **Create** options as training, prediction, or both.
    - Select your subscription.
    - Underneath select existing or create a new resource group.
    - Name the Custom Vision API.
    - For **training resource** select the location and pricing tier (free or standard)
    - For **prediction resource** select the location and pricing tier (free or standard)

4. Click **Review + create**.
5. Review the information. Click **Create**.

Now, let us have a look at the resource group.

![](/media/2020-05-08-custom-vision-api-spo/04.png)

The resource group contains 2 services: training and prediction API. Each one has separate API key and endpoints.

## Train the Model

We need to train the service to recognize our characters.

1. Open [https://www.customvision.ai](https://www.customvision.ai) This offers UI to upload, tag, and train the model.
2. Create a new project.

    ![](/media/2020-05-08-custom-vision-api-spo/05.png)

3. Select your parameters to create a new project.

    ![](/media/2020-05-08-custom-vision-api-spo/06.png)

4. Click **Create project**.
5. Once the project is created, add the related images.

    ![](/media/2020-05-08-custom-vision-api-spo/07.png)

6. Microsoft recommends uploading at least 50 images for each tag you make. Optionally create negative tags to exclude things like company logos and box art.

    ![](/media/2020-05-08-custom-vision-api-spo/08.jpeg)

7. Once we have uploaded and tagged our images, click **Train**. Every time you train the classifier, it will create a new iteration of your model.
8. There are two types of training: Quick and Advanced. **Quick Training** is quicker and works fine when we have lots of good samples. **Advanced Training** takes anywhere from 1-24 hours however can be more accurate. Note that you only get 1 free hour of training, so use the Advanced Training when you absolutely require it.
9. Once training is complete, you will get metrics for the model as overall and per tag.

    ![](/media/2020-05-08-custom-vision-api-spo/09.png)

10. Click the **Quick Test**.
11. Browse to local file for from URL.

    ![](/media/2020-05-08-custom-vision-api-spo/10.png)

    Cool! It predicted Spiderman with a 99.9% probability.

12. Click **Publish** to publish the model.

## Custom Vision Prediction API

The real power of the service is in the API which allows us to integrate within our applications. We will need below information from Azure portal

- API key
- Project ID
- Prediction URL

**API Key**

Click **Keys and Endpoint** to get the key to access Cognitive service API.

![](/media/2020-05-08-custom-vision-api-spo/11.png)

**Prediction Key**

Follow the below steps to get the prediction key:

1. Open [https://www.customvision.ai/](https://www.customvision.ai/)
2. Click the gear icon.
3. Note down the **Key** under **Resources**.

![](/media/2020-05-08-custom-vision-api-spo/12.png)

## Project ID

Follow the below steps to get the Project ID:

1. Open [https://www.customvision.ai/](https://www.customvision.ai/)
2. Select the project.
3. Click the gear icon.
4. Note down the **Key** under **Resources**.

![](/media/2020-05-08-custom-vision-api-spo/13.png)

## Prediction API

Follow the below steps to get the Prediction API:

- Select the project
- Click **Performance**.
- Click the **Prediction URL**.

![](/media/2020-05-08-custom-vision-api-spo/14.png)

The first one is used when we have an image URL. Use the second one when we have an image on a local folder.

## Set up Power Automate Flow.

Now we will set up Power Automate flow to integrate Computer Vision API for analyzing images uploaded to SharePoint.

Follow the below steps to set up Power Automate flow:

1. Open **Power Automate portal**.
2. Create an **Automated flow**.

    ![](/media/2020-05-08-custom-vision-api-spo/15.png)

3. Name the flow.
4. Choose the flow's trigger as **When a file is created in a folder**.

    ![](/media/2020-05-08-custom-vision-api-spo/16.png)

5. Click **Create**.
6. Specify the site address and folder Id.

    ![](/media/2020-05-08-custom-vision-api-spo/17.png)

7. Click **\+ New step**.
8. Find the connector **Custom Vision API**.
9. Select action as **Classify an image (V2) (preview)**.

    ![](/media/2020-05-08-custom-vision-api-spo/18.png)

10. Configure the **Computer Vision API** connection.
11. Specify the name for the connection, cognitive services account key, and endpoint.

    ![](/media/2020-05-08-custom-vision-api-spo/19.png)

12. Click **Create**.
13. In the next screen, specify Project ID of custom vision project, Published Name of the model to evaluate against, and Image content we want to analyze.

    ![](/media/2020-05-08-custom-vision-api-spo/20.png)

14. Upload an image in the SharePoint library. The flow will run, and we will have an output like below:

![](/media/2020-05-08-custom-vision-api-spo/21.png)

## Update Metadata to SharePoint

We will have to parse the response and store the metadata to SharePoint.

1. Add an action "Send an HTTP request to SharePoint" to get to the file to update the properties.

    ![](/media/2020-05-08-custom-vision-api-spo/22.png)

    - **Site address**: Specify SharePoint site url
    - **Method**: Get
    - **Uri**: /\_api/web/getFileByServerRelativeUrl('File Path')?$select=ListItemAllFields/ID&$expand=ListItemAllFields
    - **Headers**: key: accept, value: application/json | key: content-type, value: application/json

2. Run the flow and get the Body from the output of "**Send an HTTP request to SharePoint**".

    ![](/media/2020-05-08-custom-vision-api-spo/23.png)

3. Get the first Prediction as follows:

    ![](/media/2020-05-08-custom-vision-api-spo/24.png)

4. Initialize the variables Prediction tag name, Prediction probability to store the returned prediction value.
5. Initialize the variable FilePath to store the path to the uploaded SharePoint file in the library.

    ![](/media/2020-05-08-custom-vision-api-spo/25.png)

6. Add an action **Parse JSON**. Specify the content as Body.
7. Generate the schema from a sample of the previous run.

    ![](/media/2020-05-08-custom-vision-api-spo/26.png)

8. Add an action “Update file properties”.

![](/media/2020-05-08-custom-vision-api-spo/27.png)

## Test the Solution

In the SharePoint library, upload an image. This will trigger a flow. A flow will call Custom Vision service for analyzing images uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library.

![](/media/2020-05-08-custom-vision-api-spo/28.png)

## Summary

Azure Cognitive Services Custom Vision API helps to analyze images uploaded to SharePoint. Custom vision API is also trained by Microsoft to identify common objects and scenarios. On top of it, we can also train the Custom vision service for specific things we want to recognize ourselves. This information then can be tagged in metadata columns inside the picture/document library for ease of searching and filtering.

## References

- [Azure custom vision](https://www.customvision.ai/)
- [Custom Vision Service](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/)
