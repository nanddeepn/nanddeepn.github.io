---
title: "Azure Cognitive Services: Custom Vision API for better image prediction in SharePoint Online"
date: "2020-05-08"
---

## Overview

In the previous article, we explored the [Computer Vision API](https://nanddeepnachanblogs.com/2020/04/computer-vision-api-to-analyze-images/). The computer vision API offers services to detect commonly known objects. For example, when you upload an image of a car, it will correctly identify it as a car. However, in certain circumstances, you want to identify a few objects or scenarios which computer vision API does not recognize. Custom vision API becomes handy in these scenarios.

In this article, we will have an overview of the Custom Vision API, build, and use it in our applications.

## Custom Vision API

Custom vision API is also trained by Microsoft to identify common objects and scenarios. On top of it, we can also train the Custom vision service for specific things we want to recognize ourselves.

Custom vision service follows below simple principal.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-17.png)

## Custom Vision API in MS Azure

Custom Vision API has a separate key for prediction and training.

**Pricing:**

- Pricing for prediction goes per 1000 transactions.
- Pricing for training goes per computer hour.
- Image storage is priced per 1000 images.

**Set up Custom Vision API**

Follow the below steps to set up the custom vision API in MS Azure.

1. Open MS Azure portal ([https://portal.azure.com/](https://portal.azure.com/))
2. Click “**Create a resource**”.
3. Search for “**Custom vision**” in the marketplace.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-18.png)

1. Click **Create**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-19.png)

- Choose the **Create** options as training, prediction, or both.
- Select your subscription.
- Underneath select existing or create a new resource group.
- Name the Custom Vision API.
- For **training resource** select the location and pricing tier (free or standard)
- For **prediction resource** select the location and pricing tier (free or standard)

1. Click “**Review + create**”.
2. Review the information. Click **Create**.

Now, let us have a look at the resource group.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-20.png)

The resource group contains 2 services: training and prediction API. Each one has separate API key and endpoints.

## Train the Model

We need to train the service to recognize our characters.

1. Open [https://www.customvision.ai](https://www.customvision.ai) This offers UI to upload, tag, and train the model.
2. Create a new project.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-21.png)

1. Select your parameters to create a new project.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-22.png)

1. Click **Create project**.
2. Once the project is created, add the related images.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-23.png)

1. Microsoft recommends uploading at least 50 images for each tag you make. Optionally create negative tags to exclude things like company logos and box art.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image.jpeg)

1. Once we have uploaded and tagged our images, click **Train**. Every time you train the classifier, it will create a new iteration of your model.
2. There are two types of training: Quick and Advanced. **Quick Training** is quicker and works fine when we have lots of good samples. **Advanced Training** takes anywhere from 1-24 hours however can be more accurate. Note that you only get 1 free hour of training, so use the Advanced Training when you absolutely require it.
3. Once training is complete, you will get metrics for the model as overall and per tag.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-24.png)

1. Click the **Quick Test**.
2. Browse to local file for from URL.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-25.png)

Cool! It predicted Spiderman with a 99.9% probability.

1. Click **Publish** to publish the model.

## Custom Vision Prediction API

The real power of the service is in the API which allows us to integrate within our applications. We will need below information from Azure portal

- API key
- Project ID
- Prediction URL

**API Key**

Click **Keys and Endpoint** to get the key to access Cognitive service API.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-26.png)

**Prediction Key**

Follow the below steps to get the prediction key:

1. Open [https://www.customvision.ai/](https://www.customvision.ai/)
2. Click the gear icon.
3. Note down the **Key** under **Resources**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-27.png)

## Project ID

Follow the below steps to get the Project ID:

1. Open [https://www.customvision.ai/](https://www.customvision.ai/)
2. Select the project.
3. Click the gear icon.
4. Note down the **Key** under **Resources**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-28.png)

## Prediction API

Follow the below steps to get the Prediction API:

- Select the project
- Click **Performance**.
- Click the **Prediction URL**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-29.png)

The first one is used when we have an image URL. Use the second one when we have an image on a local folder.

## Set up Power Automate Flow.

Now we will set up Power Automate flow to integrate Computer Vision API for analyzing images uploaded to SharePoint.

Follow the below steps to set up Power Automate flow:

1. Open **Power Automate portal**.
2. Create an **Automated flow**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-30.png)

1. Name the flow.
2. Choose the flow’s trigger as “When a file is created in a folder”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-31.png)

1. Click **Create**.
2. Specify the site address and folder Id.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-32.png)

1. Click “**\+ New step**”.
2. Find the connector “**Custom Vision API**”.
3. Select action as “**Classify an image (V2) (preview)**”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-33.png)

1. Configure the “**Computer Vision API**” connection.
2. Specify the name for the connection, cognitive services account key, and endpoint.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-34.png)

1. Click **Create**.
2. In the next screen, specify Project ID of custom vision project, Published Name of the model to evaluate against, and Image content we want to analyze.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-35.png)

1. Upload an image in the SharePoint library. The flow will run, and we will have an output like below:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-36.png)

## Update Metadata to SharePoint

We will have to parse the response and store the metadata to SharePoint.

1. Add an action "Send an HTTP request to SharePoint" to get to the file to update the properties.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-37.png)

- **Site address**: Specify SharePoint site url
- **Method**: Get
- **Uri**: /\_api/web/getFileByServerRelativeUrl('File Path')?$select=ListItemAllFields/ID&$expand=ListItemAllFields
- **Headers**: key: accept, value: application/json | key: content-type, value: application/json

1. Run the flow and get the Body from the output of "**Send an HTTP request to SharePoint**".

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-38.png)

1. Get the first Prediction as follows:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-39.png)

1. Initialize the variables Prediction tag name, Prediction probability to store the returned prediction value.
2. Initialize the variable FilePath to store the path to the uploaded SharePoint file in the library.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-40.png)

1. Add an action “**Parse JSON**”. Specify the content as Body.
2. Generate the schema from a sample of the previous run.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-41.png)

1. Add an action “Update file properties”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-42.png)

## Test the Solution

In the SharePoint library, upload an image. This will trigger a flow. A flow will call Custom Vision service for analyzing images uploaded to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the library.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-43.png)

## Summary

Azure Cognitive Services Custom Vision API helps to analyze images uploaded to SharePoint. Custom vision API is also trained by Microsoft to identify common objects and scenarios. On top of it, we can also train the Custom vision service for specific things we want to recognize ourselves. This information then can be tagged in metadata columns inside the picture/document library for ease of searching and filtering.

## References

- [Azure custom vision](https://www.customvision.ai/)
- [Custom Vision Service](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/)
