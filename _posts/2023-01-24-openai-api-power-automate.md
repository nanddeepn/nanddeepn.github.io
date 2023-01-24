---
title: "Using Open AI APIs with Power Automate"
date: "2023-01-24"
share: true
header:
  image: media/2023-01-24-openai-api-power-automate/03.png
  teaser: media/2023-01-24-openai-api-power-automate/03.png
categories:
  - Office 365
  - Power Automate
  - OpenAI
tags:
  - "2023"
  - January 2023
last_modified_at: 2023-01-24T00:00:00-00:00
---
## Introduction

Open AI is the most buzzing thing at this moment. It is powered by a family of models with different capabilities.

In this article, we will use the Open AI API to answer the questions from Power Automate.


## Get the API Key

Follow the below steps to get the API key to work with Open AI APIs.

1. Navigate to [OpenAI](https://openai.com/api/). Sign up, or register to proceed.
2. From your profile, select **View API Keys**.
3. Click **+ Create new secret key** to create one. Note down the secret key for future use.

    ![](/media/2023-01-24-openai-api-power-automate/01.png)


## Open AI Playground

Before we start using the Open AI functionality in our application, the best place to play and test it, is the Open AI Playground.

From the playground, you can load a pre-defined preset (e.g., Q&A, Chat, etc.) and select other configurations.

![](/media/2023-01-24-openai-api-power-automate/02.png)

Once you are happy with evaluating the functionality, click **View code**. The available code options are python, node.js, curl, and json.

![](/media/2023-01-24-openai-api-power-automate/03.png)

The curl option will provide you with more information on passing header information. We will use json format for our Power Automate flow.

![](/media/2023-01-24-openai-api-power-automate/04.png)


## Power Automate

We will now develop a Power Automate flow to make a request to Open AI API to answer the user question.

Follow the below steps to set up the Power Automate flow:

1. We will start by creating an instant cloud flow and add input as a Question. In the next step, we will have a variable to store the prompt.

    ![](/media/2023-01-24-openai-api-power-automate/05.png)

2. In the next step, we will use HTTP action to post the request to Open AI API.

    ![](/media/2023-01-24-openai-api-power-automate/06.png)

3. Run the flow manually to see the result. Copy the **Body** from the output of the HTTP request.

    ![](/media/2023-01-24-openai-api-power-automate/07.png)

4. In the next step, we will use Parse JSON to extract the output. Use the expression `first(body('Parse\_JSON')?['choices'])['text']` in the compose action to see the response from Open AI.

    ![](/media/2023-01-24-openai-api-power-automate/08.png)


## Test the result

Manually trigger the flow and ask your question. You should get the answer back from the Open AI API.

![](/media/2023-01-24-openai-api-power-automate/09.png)


## Summary

Open AI offers various models to use in various scenarios. Open AI API can be consumed in Power Automate to answer user queries.


## References

- [OpenAI](https://openai.com/)
