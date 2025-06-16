---
title: "Integrating Azure Document Intelligence with Azure OpenAI"
date: "2025-06-16"
share: true
header:
  image: media/2025-06-16-integrate-azdocintelligence-azopenai/01.png
  teaser: media/2025-06-16-integrate-azdocintelligence-azopenai/01.png
categories:
  - AI
  - Copilot
tags:
  - "2025"
  - June 2025
last_modified_at: 2025-06-16T00:00:00-00:00
---
# Integrating Azure Document Intelligence with Azure OpenAI

## Overview

Businesses deal with large volumes of unstructured documents — invoices, contracts, forms, reports, and emails. Extracting information from these documents manually is time-consuming and error-prone.

**Azure Document Intelligence** (formerly *Form Recognizer*) allows you to automate this extraction using AI models that understand document structure and content. Meanwhile, **Azure OpenAI Service** provides powerful language models like GPT-4 that can interpret, summarize, and reason over text.

By integrating these two services, organizations can build intelligent document processing pipelines that:

- Extract data and structure from documents  
- Enrich, summarize, and interpret this data  
- Enable advanced search, reasoning, and automation workflows

---

## Technical Implementation

### High-Level Process

![](/media/2025-06-16-integrate-azdocintelligence-azopenai/01.png)

Component Breakdown

----------|-------------|
Component | Description |
----------|-------------|
Azure Blob Storage | Store scanned PDFs, images, documents |
Azure Document Intelligence	| Extract text, layout, and structured data from documents |
Azure OpenAI Service | Interpret, summarize, answer questions on extracted content |
Application Layer | Web apps, chatbots, dashboards using enriched data |

## Detailed Implementation

**Pre-requisites**
1. Azure Subscription
2. Azure Document Intelligence Resource
3. Azure OpenAI Resource (GPT-4 model)
4. Storage Account (Blob Storage)
5. Optionally: Azure Functions, Logic Apps, or Power Automate to automate the pipeline

## Step-by-step Pipeline

**Step 1: Upload Documents**
User uploads scanned PDF, image, or document to Azure Blob Storage.

**Step 2: Trigger Document Intelligence**

- Use Azure Function / Logic App to trigger Document Intelligence on blob upload.
- Call REST API of Azure Document Intelligence (Document Analysis model).

```
POST https://{endpoint}/formrecognizer/documentModels/prebuilt-layout:analyze?api-version=2023-07-31
```

- Response: JSON with extracted text, tables, key-value pairs, layout.

**Step 3: Prepare Prompt for Azure OpenAI**

- Combine extracted text and structured data.
- Construct a system prompt for GPT-4:

```
You are a document analyst. Given the following extracted content from a contract, summarize the key clauses and obligations. Content:
[EXTRACTED TEXT]
```

**Step 4: Call Azure OpenAI API**

- Send the prepared prompt + extracted content to Azure OpenAI GPT-4 endpoint:

```
POST https://{endpoint}/openai/deployments/{deployment-id}/chat/completions?api-version=2024-02-01
```

- Model returns summary / insights / answers.

**Step 5: Use the Results**

- Store results in:
    - SQL Database
    - Azure Cognitive Search index
    - SharePoint list
- Expose via:
    - Power BI dashboard
    - Power Apps app
    - Teams chatbot (Copilot Studio)
    - Web portal

## Use Cases

**1. Contract Analysis**

- Upload contracts and extract key terms.
- Use GPT-4 to:
    - Summarize obligations.
    - Highlight risks.
    - Answer questions like "What is the payment term?"

**2. Invoice Processing**

- Extract fields like invoice number, amount, due date.
- Use GPT-4 to:
    - Validate extracted data.
    - Provide descriptive summaries of expenses.
    - Trigger workflows if anomalies are detected.

**3. Intelligent Search**

- Index extracted content into Azure Cognitive Search.
- Use GPT-4 to provide natural language Q&A over documents.

Example:

> User: What is the warranty period mentioned in supplier contracts?
>
> Response (via GPT-4): The warranty period is 2 years from the date of delivery.

**4. Report Summarization**

- Summarize large technical or financial reports.
- Provide executive summaries.
- Extract action items from meeting transcripts.

**5. Email Understanding**

- Ingest emails and attachments.
- Summarize email chains.
- Answer questions like What tasks were assigned to John in this email thread?

## Summary

Integrating Azure Document Intelligence with Azure OpenAI unlocks the next level of document understanding:
Automate extraction of structured and unstructured data, Enhance documents with GPT-4 reasoning and summarization, Build rich applications with intelligent document Q&A, and Improve compliance, efficiency, and user experience.

