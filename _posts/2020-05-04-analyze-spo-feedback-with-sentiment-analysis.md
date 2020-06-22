---
title: "Analyze User Feedback in SharePoint Online with Sentiment Analysis API and Power Automate"
date: "2020-05-04"
---

## Overview

The Sentiment analysis API provided by MS Azure helps to evaluate the user input and returns the sentiment score in the range of 0 (negative) to 1 (positive). This helps in analyzing the user feedback for any service.

In this article, we will use a SharePoint list as a backend to collect user feedback. The feedback will be then sent to Azure Sentiment analysis using Power Automate and store back the response in the SharePoint list.

## Set up Sentiment Analysis API in MS Azure

Let us start by building our first building block in MS Azure Cognitive Services by creating Sentiment Analysis API. Follow the below steps to set up Sentiment Analysis API.

**Create a resource**

1. Login to Microsoft Azure Portal ([https://portal.azure.com](https://portal.azure.com)).
2. Click **Create a resource**.
3. Select **AI + Machine Learning**
4. Select **Text Analytics**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image.png)

**Create a Text Analytics**

1. Fill in the details to create the Text analytics API.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-1.png)

The parameters are as follows:

- **Name:** Unique name of Text analytics API.
- **Subscription:** Select Azure subscription to create Text analytics under it.
- **Location:** The location to host Text analytics API.
- **Pricing tier:** Select tier as per features you want to use. More details at [https://azure.microsoft.com/en-us/pricing/details/cognitive-services/text-analytics/](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/text-analytics/)
- **Resource group:** Select an existing or create a new resource group.

1. Click **Create**.
2. Once the deployment finishes, the Text analytics resource will be available to use.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-2.png)

1. Note down the API Key and endpoint to authenticate your applications and start sending calls to the service.

## Test the Computer Vision

We will use the API Console to quickly try the API without writing any code.

1. From the left menu, select **Quick start**.
2. Click **API Console (V2)**.
3. Select the testing console in the region where you created your resource.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-3.png)

1. The selection of the region will form the request URL.
2. Specify the resource name and query parameters.
3. Specify the API key in Headers.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-4.png)

1. In the request body, provide the custom text to analyze or use the default one.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-5.png)

1. Click **Send**.
2. The response will be shown as follows:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-6.png)

## Set up SharePoint list

Create a SharePoint list with below schema

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-7.png)

## Set up Power Automate Flow

Now we will set up Power Automate flow to integrate Text analysis API for analyzing user feedback submitted to the SharePoint list.

Follow the below steps to set up Power Automate flow:

1. On the SharePoint list, click **Automate** > **Power Automate** > **Create a flow**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-8.png)

1. Select a template “When a new item is added in SharePoint, complete a custom”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-9.png)

1. Verify your connection to SharePoint.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-10.png)

1. Click **Continue**.
2. Add an activity “Text Analytics”, select action “Detect Sentiment”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-11.png)

1. Specify the connection name, Key from Azure and Azure endpoint.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-12.png)

1. Click **Create**.
2. Specify the SharePoint list column Feedback as Text. Optionally specify the language of feedback.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-13.png)

1. Add an entry to the SharePoint list. This will trigger a flow. The OUTPUTS section will show the Score from Text Analytics API.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-14.png)

1. Add an activity Update item available under the SharePoint category.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-15.png)

## Test the Solution

In the SharePoint library, add a new item as user feedback. This will trigger a flow. A flow will call Sentiment analysis API for analyzing the feedback submitted to SharePoint. The flow will then update the returned metadata to the SharePoint column inside the list with a sentiment score.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/05/word-image-16.png)

## Use Cases

The sentiment analysis can be useful in below scenarios:

- Product reviews
- Social media analytics (e.g. Tweets targeted to a hashtag)
- Intellectual Property

## Summary

Azure Sentiment analysis API helps to analyze the text input submitted to SharePoint and evaluate the user input and returns the sentiment score in the range of 0 (negative) to 1 (positive).

## References

- [https://azure.microsoft.com/en-in/services/cognitive-services/text-analytics/](https://azure.microsoft.com/en-in/services/cognitive-services/text-analytics/)
- [https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3)
