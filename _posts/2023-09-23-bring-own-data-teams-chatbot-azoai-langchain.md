---
title: "Bring your own data to Microsoft Teams chat bot with Azure OpenAI and LangChain"
date: "2023-09-23"
share: true
header:
  image: media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/02.png
  teaser: media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/02.png
categories:
  - MS Teams
  - OpenAI
tags:
  - "2023"
  - September 2023
last_modified_at: 2023-09-23T00:00:00-00:00
---
## Introduction

In the world of OpenAI, the ability to harness and integrate your own data into AI models has become a game-changer. Azure OpenAI and LangChain, two powerful tools open up a world of possibilities for bringing your own data to the AI applications. You can personalize and enrich your Teams chatbot with your own data, making it a true extension of your organization's knowledge and processes.

This article explores the exciting possibilities of bringing your own data to Microsoft Teams chatbots using the dynamic duo of Azure OpenAI and LangChain.

> This article is an inspiration from the awesome work by brilliant [Garry Trinder](https://twitter.com/garrytrinder) to [Build an AI powered chat bot for Microsoft Teams using Teams Toolkit for Visual Studio Code, LangChain and OpenAI](https://www.linkedin.com/feed/update/urn:li:activity:7107854582721966080?updateEntityUrn=urn%3Ali%3Afs_feedUpdate%3A%28V2%2Curn%3Ali%3Aactivity%3A7107854582721966080%29).


## Why LangChain?

When you use ChatGPT like applications, it uses OpenAI API to respond to your prompts, which internally uses LLM such as GPT-3.5 or GPT-4.

There are below limitations to using ChatGPT:

1. ChatGPT does not have access to your internal organization data.
2. ChatGPT is not aware of current world happenings. Its knowledge is limited to September 2021 (as of today).

    ![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/01.png)

LangChains fills in this gap by helping you connect to your organization's data.


## What is LangChain?

LangChain is an open-source framework that can help you build applications using LLMs (Large Language Models). The LangChain simply helps you to connect the LLMs such as OpenAI's GPT-3.5 and GPT-4 with external data sources (including your organization data, YouTube videos, articles, etc.)

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/02.png)

In our scenario, we will use one of my published articles as an example of our own data.


## Azure OpenAI

Follow the below steps to set up Azure OpenAI:

- Create an **Azure OpenAI service** in the Azure portal.
- In the Azure AI Studio, from the left navigation, under **Management**, click **Models**.
- Search and select the **text-embedding-ada-002** and **gpt-35-turbo** models.
- Click **Deploy**.

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/03.png)


## Teams Toolkit

Teams Toolkit is available as an extension for Visual Studio Code and Visual Studio 2022 to help developers create, debug, and deploy Microsoft Teams apps.

In Visual Studio Code, it can be installed as an extension. Follow the below steps to install:

1. Open Visual Studio Code.
2. Click **View** > **Extensions**.
3. Find and install **Teams Toolkit**.

    ![](/media/2023-05-07-openap-apis-teams-toolkit-bot/01.png)


## Create a Basic Bot with Teams Toolkit

Follow the steps below to create a bot solution with Teams Toolkit.

1. Open Visual Studio Code.
2. From the left menu, click **Teams Toolkit**.
3. Choose to **Create a New App**.
4. Select **Bot**.

    ![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/04.png)


## Install npm packages

Use the below command to install the npm packages:

**Install LangChain**

```powershell
npm install -S langchain
```

**Install Cheerio**

Cheerio is being used as a document loader.

```powershell
npm install -S cheerio
```


## Make our Basic Bot intelligent with Azure OpenAI and

**Update environment file**

To start with, we will add the Azure OpenAI key to the environment file as shown below:

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/05.png)

**Update runtime environment variables**

Update the runtime environment variables section in the `teamsapp.local.yml` file as shown below:

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/06.png)

**Update config.ts**

Update the `config.ts` file as shown below:

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/07.png)

**Update bot code**

Update the bot code as follows:

```typescript
import { TeamsActivityHandler } from "botbuilder";
import { ChatOpenAI } from "langchain/chat_models/openai";
import config from "./config";

// Document loader
import { CheerioWebBaseLoader } from "langchain/document_loaders/web/cheerio";

// Text splitter
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { OpenAIEmbeddings } from "langchain/embeddings/openai";
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { RetrievalQAChain } from 'langchain/chains';

// Split the Document into chunks for embedding and vector storage.
const textSplitter = new RecursiveCharacterTextSplitter({
  chunkSize: 8000,
  chunkOverlap: 0,
});

const loader = new CheerioWebBaseLoader(
  "https://nanddeepnachanblogs.com/about-me/"
);

const model = new ChatOpenAI({
  azureOpenAIApiKey: config.openAIApiKey,
  azureOpenAIApiVersion: "2023-07-01-preview",
  azureOpenAIApiInstanceName: "az-contoso-oai",
  azureOpenAIApiDeploymentName: "gpt-35-turbo"
});

export class TeamsBot extends TeamsActivityHandler {
  constructor() {
    super();

    this.onMessage(async (context, next) => {
      // get user message
      const { text } = context.activity;

      // send typing indicator
      await context.sendActivities([{ type: "typing" }]);

      const data = await loader.load();
      const splitDocs = await textSplitter.splitDocuments(data);

      // Embed and store the splits in a vector database (in-memory)
      const embeddings = new OpenAIEmbeddings({
        azureOpenAIApiKey: config.openAIApiKey,
        azureOpenAIApiVersion: "2023-07-01-preview",
        azureOpenAIApiInstanceName: "az-contoso-oai",
        azureOpenAIApiDeploymentName: "text-embedding-ada-002",
        maxConcurrency: 5,
        maxRetries: 10,
      });

      const vectorStore = await MemoryVectorStore.fromDocuments(splitDocs, embeddings);

      const chain = RetrievalQAChain.fromLLM(model, vectorStore.asRetriever());
      const response = await chain.call({
        query: text
      });

      // send result to user
      await context.sendActivity(response.text);

      // By calling next() you ensure that the next BotHandler is run.
      await next();
    });
  }
}
```


## Test the bot

From the Visual Studio Code, select **Run and Debug** from the left menu. Choose the run profile and test the bot solution.

I have provided the [About Me](/about-me/) page of my blog as my own data. Langchain helps the bot to connect to this data and help answer the questions.

![](/media/2023-09-23-bring-own-data-teams-chatbot-azoai-langchain/08.png)


## Code Download

The code demonstrated in this article is available [here](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/teams-toolkit/azopenai-bot-langchain/langchainbot).


## Summary

Azure OpenAI and LangChain, two powerful tools open up a world of possibilities for bringing your own data to the AI applications. LangChain is an open-source framework that can help you build applications using LLMs.


## References

- [Langchain](https://www.langchain.com)
- [Langchain: QA and Chat over Documents](https://js.langchain.com/docs/use_cases/question_answering/)
- [Teams Toolkit Overview](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/teams-toolkit-fundamentals?WT.mc_id=M365-MVP-5003693)
- [Install Teams Toolkit](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/install-teams-toolkit?WT.mc_id=M365-MVP-5003693)
- [Command bot in Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/bots/how-to/conversations/command-bot-in-teams?WT.mc_id=M365-MVP-5003693)
