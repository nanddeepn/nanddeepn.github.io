---
title: "Create Test Documents in SharePoint with OpenAI APIs and Power Automate"
date: "2023-02-12"
share: true
header:
  image: media/2023-02-12-test-docs-spo-openai-power-Automate/10.png
  teaser: media/2023-02-12-test-docs-spo-openai-power-Automate/10.png
categories:
  - SharePoint
  - Power Automate
  - OpenAI
tags:
  - "2023"
  - February 2023
last_modified_at: 2023-02-12T00:00:00-00:00
---
## Introduction

While working with SharePoint, we most times need to create test content related to a specific topic. OpenAI API is the best fit in this case to generate content related to any topic.

In this article, we will explore how OpenAI API can be combined with Power Automate to generate content in SharePoint.


## Get the API Key

Follow the below steps to get the API key to work with Open AI APIs.

1. Navigate to [OpenAI](https://openai.com/api/). Sign up, or register to proceed.

2. From your profile, select **View API Keys**.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/01.png)

3. Click **+ Create new secret key** to create one. Note down the secret key for future use.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/02.png)


## Open AI Playground

Before we start using the Open AI functionality in our application, the best place to play and test it, is the Open AI Playground.

![](/media/2023-02-12-test-docs-spo-openai-power-Automate/03.png)

Once you are happy with evaluating the functionality, click **View code**. The available code options are python, node.js, curl, and json.

The `curl` option will provide you with more information on passing header information. We will use `json` format for our Power Automate flow.

![](/media/2023-02-12-test-docs-spo-openai-power-Automate/04.png)


## Power Automate

We will now develop a Power Automate flow to make a request to OpenAI API. We will make 2 calls to OpenAI APIs to generate a document (.txt) in SharePoint.

1. Generate a given number of random words.
2. Write an essay for each of the generated random words.

Follow the below steps to set up the Power Automate flow:

1. We will start by creating an instant cloud flow and add input as a Question. In the next step, we will define a variable to store the "Number of documents" to generate.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/05.png)

2. In the next step, we will use `HTTP` action to post the request to OpenAI API. This will help us to get the random words from OpenAI.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/06.png)

3. Run the flow manually to see the result. Copy the **Body** from the output of the HTTP request.
4. Now, we will use Parse JSON to extract the output.
5. In the next step, let us initialize a variable with the below expression to store the response from Open AI (i.e., randomly generated comma-separated words).

    ```
    replace(first(body('Parse_JSON')?['choices'])['text'],decodeUriComponent('%0A'),'')
    ```

5. Further, let us split the comma-separated words string into an array stored in a variable.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/07.png)

6. Using the Apply to each action, we will iterate through each word by calling OpenAI API to write an essay on it.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/08.png)

7. Finally, the test document (.txt) can be created in the SharePoint document library using **Create file** activity.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/09.png)


Use the below formulas:

|Parameter|Formula|
|---------|-------|
| File Name | `concat(items('Apply_to_each'), '.txt')` |
| File Content | `replace(first(body('Parse_JSON_Essay_Response')?['choices'])['text'],decodeUriComponent('%0A'),'')` |


## End Result

After running the Power Automate, the test documents will be generated in the SharePoint document library as follows.

![](/media/2023-02-12-test-docs-spo-openai-power-Automate/10.png)


## Summary

OpenAI API is the best fit to generate content related to any topic. This feature can be combined with Power Automate to generate content in SharePoint.


## References

- [OpenAI](https://openai.com/)
