---
title: "Freeform Document Processing with Microsoft Syntex"
date: "2022-12-05"
share: true
header:
  image: media/2022-12-05-freeform-doc-processing-syntex/12.png
  teaser: media/2022-12-05-freeform-doc-processing-syntex/12.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2022"
  - December 2022
last_modified_at: 2022-12-05T00:00:00-00:00
---
## Overview

When your organization receives a huge number of documents over email, fax, or a physical copy, it is difficult to process them physically and classify the information. Freeform document processing as an AI offering of Microsoft Syntex makes your work easy in these situations.

This article is an extension to my previous article Getting started with Microsoft Syntex but focuses specifically on Freeform document processing.

## Freeform document processing

The freeform selection method trains a model by selecting content anywhere in a file. This is helpful to extract information from unstructured and freeform documents. E.g., letters, contracts, etc. as information can appear anywhere in a document.

Freeform document processing model uses Microsoft Power Apps AI Builder to create and train models within Microsoft Syntex.

## Business Scenario

Let us take a scenario, where we are receiving invoices from multiple vendors (e.g., Adatum and Contoso). Adatum shares the invoices over an email. Whereas invoices received from Contoso are in scanned format. Each of them follows its format to share the invoices.

You may use the [sample data](https://learn.microsoft.com/en-us/ai-builder/form-processing-sample-data?WT.mc_id=M365-MVP-5003693) for setting up the model.

## Create a Freeform document processing model

Follow the below steps to create a Freeform document processing model:

1. Open the content center in SharePoint.
2. From the top navigation, click **Models**.
3. Click **Create a model**.
4. Select the **Freeform selection method**.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/01.png)

5. Read out the details to understand the model better. Note, you can use this model on PDF and image files in the English language.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/02.png)

6. Name your model. Click **Create**.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/03.png)


**Choose information to extract**

1. We can now start by defining the information to extract as Field, Checkbox, or Table.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/04.png)

2. Based on the fields that we want to capture from the invoice, let us define the fields.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/05.png)


**Add collections of documents**

In the next step, we need to create a collection for each layout (e.g., Adatum and Contoso invoices).

![](/media/2022-12-05-freeform-doc-processing-syntex/06.png)

We can add documents from any of below source:

- Local device
- Feedback loop
- SharePoint
- Azure Blob Storage

> Note:
> We need to add at least 5 sample documents in total.


**Tag documents**

1. In the next step, we need to tag the documents to extract the information from the sample documents into the fields defined.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/07.png)

2. Processing the table involves defining the rows and columns. Or switch to advanced tagging mode to tag the content of the table.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/08.png)


**Model summary**

Review your model.

![](/media/2022-12-05-freeform-doc-processing-syntex/09.png)


## Train a model

Click **Train** to train your model. 

> Note:
> This may take from 20 minutes up to a few hours.


## Publish trained model

After the training is complete, your model is ready to publish. It is a good idea to perform a **Quick test** before you go ahead and publish it.

![](/media/2022-12-05-freeform-doc-processing-syntex/10.png)


## Apply the model

As a next step, we need to apply the model to a document library to start processing files automatically.

1. Click **+ Add library**.
2. Select SharePoint and document library.
3. If you choose to **Extract info from tables**, specify the SharePoint list.

    ![](/media/2022-12-05-freeform-doc-processing-syntex/11.png)


## End User Experience

Once the documents are uploaded to the designated document library, select the documents, and click **Classify and Extract**.

![](/media/2022-12-05-freeform-doc-processing-syntex/12.png)

> Note:
> It takes around 30 minutes for Microsoft Syntex to extract the information from the documents by applying the custom models.

The result will look as follows:

![](/media/2022-12-05-freeform-doc-processing-syntex/13.png)

The information from the table gets stored in a specified SharePoint list.


## Summary

Freeform document processing is an AI offering of Microsoft Syntex that makes it easy to extract information when an organization receives a huge number of documents over email, fax, or a physical copy.


## References

- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Create a document processing custom model](https://learn.microsoft.com/en-us/ai-builder/create-form-processing-model?WT.mc_id=M365-MVP-5003693)
- [Use sample data to do document processing](https://learn.microsoft.com/en-us/ai-builder/form-processing-sample-data?WT.mc_id=M365-MVP-5003693)
- [Overview of model types in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/model-types-overview?WT.mc_id=M365-MVP-5003693)
- [Create an enterprise model in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/create-syntex-model?WT.mc_id=M365-MVP-5003693)
