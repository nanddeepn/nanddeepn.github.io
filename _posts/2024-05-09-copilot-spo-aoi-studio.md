---
title: "Building Copilot with your SharePoint documents, Azure OpenAI, and Copilot Studio"
date: "2024-05-09"
share: true
header:
  image: media/2024-05-09-copilot-spo-aoi-studio/16.png
  teaser: media/2024-05-09-copilot-spo-aoi-studio/16.png
categories:
  - Microsoft 365
  - Copilot
  - SharePoint
tags:
  - "2024"
  - May 2024
last_modified_at: 2024-05-09T00:00:00-00:00
---
## Overview

Bringing your own data from SharePoint documents to Copilot helps to build the Retrieval Augmented Generation (RAG) based system, enhancing the platform's capabilities for information retrieval.

In this article, we will explore building the RAG pattern to surface the SharePoint documents to Copilot.

## Solution Components

Below are the components involved:

- **SharePoint Document Library**: A place where users can upload documents manually.
- **Power Automate**: On document add / update / delete, a flow will update the files in Blob storage.
- **Blob storage:** The documents will be stored in blob storage, which later can be accessed via AI Search
- **AI Search:** It will index documents and embed the vectors.
- **Azure OpenAI**: GPT based model will be used for generating the text and an embedding model will be used to generate the embeddings.

## Power Automate

We will start by creating a Power Automate flow to upload the documents to Azure blob storage.

Follow the below steps:
1. Open the [Power Automate portal](https://make.powerautomate.com/).
2. Build an **automated cloud flow**.
3. Start by adding **When a file is created or modified (properties only)**

    ![](/media/2024-05-09-copilot-spo-aoi-studio/01.png)

4. To read file content, we will use **Get file content using path**.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/02.png)

5. Finally, we will upload the content to the blob using the **Create blob (V2)** activity.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/03.png)


## Getting Data into Azure AI Search

Azure AI Search supports various data sources, including Azure Blob Storage, Azure SQL Database, Cosmos DB, Azure Table Storage, and more. Depending on your data source and its update frequency, you may need to set up a scheduled refresh to keep your index up to date with the latest data changes.

Follow the below steps:

1. Open Azure AI Search service.
2. Click **Import data**.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/04.png)

3. Select data source as Azure Blob storage.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/05.png)

4. Click **Next**.
5. Optionally consider adding any cognitive skills.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/06.png)

6. Customize the target index as needed for your scenario.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/07.png)

7. Create an indexer and define the refresh schedule.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/08.png)

8. Click **Submit**.
9. Verify your index reflects the correct document count.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/09.png)


## Add your data from Azure OpenAI Studio

We will implement the RAG by bringing our hotel data indexed in Azure AI Search service using Azure OpenAI Studio.

1. Open Azure OpenAI Studio.
2. Open **Chat** playground.
3. Click **Add your data**.
4. Select the configurations as shown below:

    ![](/media/2024-05-09-copilot-spo-aoi-studio/10.png)

5. Click **Next**.
6. Select the Search type as Semantic.

    ![](/media/2024-05-09-copilot-spo-aoi-studio/11.png)

7. Click **Next**.


## Deploy to Copilot Studio

Once our indexed data is brought in, click **Deploy to** and select **A new copilot in Copilot Studio (preview)**.

![](/media/2024-05-09-copilot-spo-aoi-studio/12.png)

Create a Copilot named **Hotel Copilot**.

![](/media/2024-05-09-copilot-spo-aoi-studio/13.png)

Now, if you navigate to **Topics**\> **Conversational boosting** and **Edit** under **Create generative answers**, you should see the Azure OpenAI configured as a data source.

![](/media/2024-05-09-copilot-spo-aoi-studio/14.png)

The **Connection properties** should have all the Azure OpenAI configurations from Azure AI Studio.

![](/media/2024-05-09-copilot-spo-aoi-studio/15.png)


## Test the Copilot

In the test pane, provide the prompt (e.g., Tell me about the Hotel Creek) to test the working.

![](/media/2024-05-09-copilot-spo-aoi-studio/16.png)


## Limitations:

1. Vector search is not yet supported.
2. Azure OpenAI service and Microsoft Copilot Studio need to be in the same tenant. That means multi-tenant application is not supported.

## Summary

Microsoft Copilot Studio supports data source as Azure OpenAI. The documents uploaded in SharePoint can be indexed using Azure AI Search to build Copilots in order to help users answer the queries.

## References

- [Connect your data to Azure OpenAI for generative answers](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-generative-answers-azure-openai?WT.mc_id=M365-MVP-5003693)
- [Use a custom data source for generative answers](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-generative-answers-custom-data?WT.mc_id=M365-MVP-5003693)
