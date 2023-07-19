---
title: "Using Azure OpenAI Chat Completion API with Power Apps"
date: "2023-07-19"
share: true
header:
  image: media/2023-07-19-oai-chat-completion-api-power-apps/12.png
  teaser: media/2023-07-19-oai-chat-completion-api-power-apps/12.png
categories:
  - Power Apps
  - Power Platform
  - OpenAI
tags:
  - "2023"
  - July 2023
last_modified_at: 2023-07-19T00:00:00-00:00
---
## Overview

Azure OpenAI service offers text generation using models like GPT. They are trained to generate a response or content from a prompt supplied to them. We can use this offering in our Power Apps app to create automated responses, generate texts, and more.

In this article, we will explore how to integrate the Azure OpenAI service text generation with GPT in Power Apps.

## Set up Azure OpenAI Service

Access to Azure OpenAI is on request basis. If you do not have one, you need to raise a request at: [https://aka.ms/oai/access](https://aka.ms/oai/access).

From the Azure portal, find and create a resource, Azure OpenAI.

![](/media/2023-07-19-oai-chat-completion-api-power-apps/01.png)


## Deploy a model

Once the Azure OpenAI service is ready, deploy gpt-35-turbo model.

![](/media/2023-07-19-oai-chat-completion-api-power-apps/02.png)

Once the model is deployed, click open the chat playground to test it out. Clicking View code will show you the options to start integrating your current prompt and settings into your application.

![](/media/2023-07-19-oai-chat-completion-api-power-apps/03.png)

The sample code makes it clear that we can use the REST APIs to consume Azure OpenAI in our applications.


## Create Custom Connector

As we know the custom connector is a wrapper around a REST API, we will build one for Azure OpenAI chat completion endpoint to consume it in Power Apps.

Follow the below steps to create a custom connector.

1. Navigate to [Power Automate](https://make.powerautomate.com).
2. Under **Data**, click **Custom connectors**.
3. Click **New custom connector** > **Create from blank**.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/04.png)

4. Name it as "Azure OpenAI Connector".
5. Under **General** section, specify the Host as `openai.azure.com`.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/05.png)

6. Under **Security** tab, specify the `Authentication type` as `API Key`.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/06.png)

7. Under **Definition** tab, click **New action** to add an action for chat completion API.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/07.png)

8. Under **Request**, click **Import from sample**.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/08.png)

9. You can use the tool like Postman to analyze the output by passing headers and body.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/09.png)

10. On the custom connector screen, under **Response**, click **Add default response**. Specify the Body from the Postman query output.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/10.png)

11. Click **Create connector** to finish creating the custom connector.
12. Test the connector before using it.


If you do not want to create a custom connector, alternatively you can use the custom connector available at: [https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureOpenAIService/](https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureOpenAIService/).


## Integrate model with Power Apps app

We will now use the AzureOpenAIService connector available on Github.

1. Create a **Blank canvas app**.
2. Under **Data**, click **Add data** and search for `AzureOpenAI` connectors. This is an Independent Publisher Connector.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/11.png)

3. Add text input, button, and label on the canvas to form a layout.

    ![](/media/2023-07-19-oai-chat-completion-api-power-apps/12.png)

4. On the Screen1 > `OnVisible` event create a global variable name `labelText`.

    ```
    Set(labelText, Blank())
    ```

5. On the Button > `OnSelect` event write the code below.

    ```
    Set(labelText, AzureOpenAIService.ChatCompletion("gpt-35-turbo", "2023-03-15-preview", {
        messages: [
            { role: "system", content: "You are an AI assistant that sarcastically answer the questions." }
        ],
        user_message: TextInput.Text
    }).answer)
    ```

6. Set the Text property of `LabelAnswer` to the global variable `labelText`.


## Test the App

As we have created a prompt to let the bot sarcastically answer the questions, you will have a fun testing it out.

Scenario 1:

![](/media/2023-07-19-oai-chat-completion-api-power-apps/13.png)

Scenario 2:

![](/media/2023-07-19-oai-chat-completion-api-power-apps/14.png)

Scenario 3:

![](/media/2023-07-19-oai-chat-completion-api-power-apps/15.png)


## Summary

Azure OpenAI service offers text generation using models like GPT. They are trained to generate a response or content from a prompt supplied to them. We can build or use custom connector to surface the REST APIs to consume Azure OpenAI in our applications.


## References

- [Use the text generation model in Power Apps](https://learn.microsoft.com/en-us/ai-builder/azure-openai-model-papp?WT.mc_id=M365-MVP-5003693)
- [Build next-gen apps with OpenAI and Microsoft Power Platform](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/build-next-gen-apps-with-openai-and-microsoft-power-platform/ba-p/3742289)
- [Azure OpenAI Service custom connector](https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureOpenAIService/)
