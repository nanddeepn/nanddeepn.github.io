---
title: "Using Azure OpenAI Chat Completion API with Power Virtual Agents"
date: "2023-08-06"
share: true
header:
  image: media/2023-08-06-oai-chat-completion-api-pva/11.png
  teaser: media/2023-08-06-oai-chat-completion-api-pva/11.png
categories:
  - Power Virtual Agents
  - Power Platform
  - OpenAI
tags:
  - "2023"
  - August 2023
last_modified_at: 2023-08-06T00:00:00-00:00
---
## Overview

Azure OpenAI service offers text generation using models like GPT. They are trained to generate a response or content from a prompt supplied to them. We can use this offering in our Power Virtual Agents (PVAs) to create automated responses, generate texts, and more.

In this article, we will explore how to integrate the Azure OpenAI service text generation with GPT in Power Virtual Agents.


## Set up Azure OpenAI Service

Access to Azure OpenAI is on a request basis. If you do not have one, you need to raise a request at: [https://aka.ms/oai/access](https://aka.ms/oai/access).

From the Azure portal, find and create a resource, Azure OpenAI.

![](/media/2023-07-19-oai-chat-completion-api-power-apps/01.png)


## Deploy a model

Once the Azure OpenAI service is ready, deploy the gpt-35-turbo model.

![](/media/2023-07-19-oai-chat-completion-api-power-apps/02.png)

Once the model is deployed, click open the chat playground to test it out. Use a system message template as an Xbox customer support agent.

![](/media/2023-08-06-oai-chat-completion-api-pva/01.png)

In Chat Completion API there are distinct sections of the prompt that are sent to the API in the form of an array of dictionaries with associated roles:

1. **System** : The system's role is to process user inputs, interpret their intent, and generate appropriate and relevant outputs. It leverages the trained models and prompt engineering techniques to understand and respond to user queries or requests.

2. **User** : The user represents the person interacting with the conversational AI system. They input their queries, requests, or commands through text or speech. The user's role is to communicate their needs or seek information from the assistant.

3. **Assistant** : The assistant is the virtual entity that interacts with the user and provides responses or assistance.

Clicking the **View code** will show you the options to start integrating your current prompt and settings into your application.

The sample code makes it clear that we can use the REST APIs to consume Azure OpenAI in our applications.


## Use Custom Connector

You can either build a custom connector from scratch to interact with the OpenAI APIs or can use the one available on GitHub.

Follow the steps below to use a custom connector.

1. Navigate to the Power Automate page ([https://make.powerautomate.com](https://make.powerautomate.com/)).
2. Under **Data**, click **Custom Connectors**.
3. Click **New custom connector** > **Import from GitHub**.
4. Select the `AzureOpenAIService` connector as shown below:

    ![](/media/2023-08-06-oai-chat-completion-api-pva/02.png)

5. After you create the connector, click the **Test** tab and select **New Connection.**

    ![](/media/2023-08-06-oai-chat-completion-api-pva/03.png)

6. To make a connection, provide the Azure OpenAI service instance name and API Key.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/04.png)


## Using the Chat Completion API in Power Virtual Agent

Follow the below steps to use the custom connector in PVA.

1. Create a new chatbot.
2. Create a new topic inside it with the name xbox.
3. Add the phrases as xbox and gaming.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/05.png)

4. Add an **Ask a question** node to get the user response and store it in a variable (e.g., VarUserQuestion).
5. We will now add a node **Call an action** > **Create a flow**.
6. Accept the user question to start with the flow.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/06.png)

7. Add an action to use our OpenAI custom connector.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/07.png)

8. Configure the chat completion API as follows:

    ![](/media/2023-08-06-oai-chat-completion-api-pva/08.png)

9. Return the answer from the Chat completion API.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/09.png)

10. Configure the action with input and output.
11. Add a node with Send a message with an output of flow.

    ![](/media/2023-08-06-oai-chat-completion-api-pva/10.png)


## Test the PVA

Start an interaction with xbox intention to see the answer being generated from the chat completions API.

![](/media/2023-08-06-oai-chat-completion-api-pva/11.png)


## Summary

Azure OpenAI service offers text generation using models like GPT. They are trained to generate a response or content from a prompt supplied to them. We can build or use custom connectors to surface the REST APIs to consume Azure OpenAI in our applications.


## References

- [Use the text generation model in Power Apps](https://learn.microsoft.com/en-us/ai-builder/azure-openai-model-papp?WT.mc_id=M365-MVP-5003693)
- [Build next-gen apps with OpenAI and Microsoft Power Platform](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/build-next-gen-apps-with-openai-and-microsoft-power-platform/ba-p/3742289)
- [Azure OpenAI Service custom connector](https://github.com/microsoft/PowerPlatformConnectors/blob/dev/custom-connectors/AzureOpenAIService/)
