---
title: "Structured Document Processing with Microsoft Syntex"
date: "2022-12-12"
share: true
header:
  image: media/2022-12-12-structured-doc-processing-syntex/12.png
  teaser: media/2022-12-12-structured-doc-processing-syntex/12.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2022"
  - December 2022
last_modified_at: 2022-12-12T00:00:00-00:00
---
## Overview

Structured document processing model works best for structured and semi-structured documents including forms and invoices.

In this article, we will explore Structured document processing model and how to use it in invoices processing scenario.


## Structured document processing

Structured document processing model uses Microsoft Power Apps AI Builder to create and train models within Microsoft Syntex. It can be used to process the documents received over mail, fax, and email.

Structured document processing model helps to extract out the key-value pairs and table data from structured and semi-structured documents.


## Business Scenario

Let us take a scenario, where we are receiving invoices from multiple vendors (e.g., Adatum and Contoso). Adatum shares the invoices over an email. Whereas invoices received from Contoso are in scanned format. Each of them follows its format to share the invoices.

![](/media/2022-12-12-structured-doc-processing-syntex/01.png)

You may use the [Invoices sample data](https://learn.microsoft.com/en-us/ai-builder/form-processing-sample-data?WT.mc_id=M365-MVP-5003693) for setting up the model.


## Create a Structured document processing model

Follow the below steps to create a structured document processing model:

1. Open the content center in SharePoint.
2. From the top navigation, click **Models**.
3. Click **Create a model**.
4. Select the **Layout method**.

    ![](/media/2022-12-12-structured-doc-processing-syntex/02.png)

5. Read out the details to understand the model better. Note, you can use this model on PDF and image files and it does support more than 73 languages.

    ![](/media/2022-12-12-structured-doc-processing-syntex/03.png)

6. Name your model. Click **Create**.

    ![](/media/2022-12-12-structured-doc-processing-syntex/04.png)


### Choose information to extract

We can now start by defining the information to extract as Field, Checkbox, or Table. Based on the fields that we want to capture from the invoice, let us define the fields.

![](/media/2022-12-12-structured-doc-processing-syntex/05.png)


### Add collections of documents

In the next step, we need to create a collection for each layout (e.g., Adatum and Contoso invoices).

![](/media/2022-12-12-structured-doc-processing-syntex/06.png)

We can add documents from any of below source:

- Local device
- Feedback loop
- SharePoint
- Azure Blob Storage

We need to add at least 5 sample documents in total.

### Tag documents

In the next step, we need to tag the documents to extract the information from the sample documents into the fields defined from each collection.

![](/media/2022-12-12-structured-doc-processing-syntex/07.png)

Processing the table involves defining the rows and columns. Or switch to advanced tagging mode to tag the content of the table.

![](/media/2022-12-12-structured-doc-processing-syntex/08.png)


### Model summary

Review your model.

![](/media/2022-12-12-structured-doc-processing-syntex/09.png)


## Train a model

Click **Train** to train your model. This may take from 20 minutes up to a few hours.


## Publish trained model

After the training is complete, your model is ready to publish. It is a good idea to perform a **Quick test** before you go ahead and publish it.

Accuracy score of the model will show the correctly predicted percentage.

![](/media/2022-12-12-structured-doc-processing-syntex/10.png)

Click **Publish** to make the model available.


## Apply the model

As a next step, we need to apply the model to a document library to start processing files automatically.

1. Click **+ Add library**
2. Select SharePoint site and document library.
3. If you choose to **Extract info from tables** , specify the SharePoint list.

![](/media/2022-12-12-structured-doc-processing-syntex/11.png)


## End User Experience

Once the documents are uploaded to the designated document library, the custom model will be applied on it to extract the information.

> Note:
> It takes around 30 minutes for Microsoft Syntex to extract the information from the documents by applying the custom models.

The result will look as follows:

![](/media/2022-12-12-structured-doc-processing-syntex/12.png)

The information from the table gets stored in a specified SharePoint list.


## Summary

Structured document processing model works best for structured and semi-structured documents including forms and invoices. Structured document processing model helps to extract out the key-value pairs and table data from structured and semi-structured documents.


## References

- [Overview of structured document processing in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/form-processing-overview?WT.mc_id=M365-MVP-5003693)
- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Create a document processing custom model](https://learn.microsoft.com/en-us/ai-builder/create-form-processing-model?WT.mc_id=M365-MVP-5003693)
- [Use sample data to do document processing](https://learn.microsoft.com/en-us/ai-builder/form-processing-sample-data?WT.mc_id=M365-MVP-5003693)
- [Overview of model types in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/model-types-overview?WT.mc_id=M365-MVP-5003693)
- [Create an enterprise model in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/create-syntex-model?WT.mc_id=M365-MVP-5003693)
