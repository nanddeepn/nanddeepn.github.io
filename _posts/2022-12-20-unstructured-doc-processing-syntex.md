---
title: "Structured Document Processing with Microsoft Syntex"
date: "2022-12-20"
share: true
header:
  image: media/2022-12-20-unstructured-doc-processing-syntex/03.png
  teaser: media/2022-12-20-unstructured-doc-processing-syntex/03.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2022"
  - December 2022
last_modified_at: 2022-12-20T00:00:00-00:00
---
## Overview

Unstructured document processing model works best for unstructured documents, such as letters or contracts which does not have any predefined format.

In this article, we will explore Unstructured document processing model and how to use it to process the Contract Renewal documents.


## Unstructured document processing

Unstructured document processing model uses artificial intelligence (AI) to process documents. As the word Unstructured says, these documents does not have any predefined layout. These documents must have text that can be identified based on phrases or patterns. (e.g. Contract renewal document with text - Service start date of \<date\>). The identified text defines both classification and extractor for the file.

- **Classifiers** helps to identify and classify documents (for e.g., contract renewal) which helps to define the content type.
- **Extractors** extracts the information from the document (for e.g., Customer name, contract date from the document) which helps to define columns of the content type.


## Business Scenario

[Syntex Samples repository](https://github.com/pnp/syntex-samples) demonstrates patterns of unstructured document processing models. In this article, we will use the [Contract Notice model](https://github.com/pnp/syntex-samples/tree/main/models/Contract-Notice).

![](/media/2022-12-20-unstructured-doc-processing-syntex/01.png)


## Create a Structured document processing model

Follow the below steps to create a structured document processing model:

1. Open the content center in SharePoint.
2. From the top navigation, click **Models**.
3. Click **Create a model**.
4. Select the **Teaching method**.

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/02.png)

5. Read out the details to understand the model better. Note, you can use this model on Word, PowerPoint, and PDF files and we must use the Latin alphabet (English characters).

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/03.png)

6. Name your model. Click **Create**.

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/04.png)


## Add example files

We need to upload positive and negative file examples.

- **Positive example**: Contains strings and information that will always be in this type of document. Upload at least five positive examples.
- **Negative example**: Any other document that does not represent the document you want to classify. Upload at least one negative example.


## Train Classifier

After adding the files, we need to label them as either positive or negative by clicking **Train Classifier**.

![](/media/2022-12-20-unstructured-doc-processing-syntex/05.png)

From the viewer, click Yes if example is positive or No otherwise.

![](/media/2022-12-20-unstructured-doc-processing-syntex/06.png)


## Create an Explanation

Explanation helps to recognize the document. For e.g., Contract renewal document will always contain a text `Request for additional disclosure`.

On the **Train** tab, under **Explanations**, click **New**.

![](/media/2022-12-20-unstructured-doc-processing-syntex/07.png)


**Blank**

Under Blank, we can choose Phrase list or Regular expression.

![](/media/2022-12-20-unstructured-doc-processing-syntex/08.png)


**From a template**

This helps to identify the type of document based on predefined Regular expression or Phrase list.

![](/media/2022-12-20-unstructured-doc-processing-syntex/09.png)

In our case, Contract Notice documents will always contain a text `Request for additional disclosure`. We will define the extractor as Blank explanation as follows:

![](/media/2022-12-20-unstructured-doc-processing-syntex/10.png)

Click **Save and train** to see if the explanation is enough to identify positive and negative documents.

![](/media/2022-12-20-unstructured-doc-processing-syntex/11.png)

If the evaluation does not match, consider creating additional explanation(s).


## Create an extractor

Extractor helps to extract information from positive documents. We need to define extractor for each entity, we want to extract from the document. For e.g., Service Start Date.

![](/media/2022-12-20-unstructured-doc-processing-syntex/12.png)


## Add a label

As a next step, we need to label the entity to extract. From the viewer, select the data to extract.

![](/media/2022-12-20-unstructured-doc-processing-syntex/13.png)

For the negative example files, select **No label** from the top.


## Add an explanation

Using explanation, we can provide hint about the entity format. On the **Train** tab, click **New** to create the explanation.

![](/media/2022-12-20-unstructured-doc-processing-syntex/14.png)

Follow the below steps to identify the **Service Start Date** , which can appear in any date format after a phrase `service start date of`.

1. Click **New** > **From a template**.
2. Select **Date** template.

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/15.png)

3. Click **Add**.
4. Click **Save**.
5. Add another explanation as **New** > **From a template**, select **After label**.
6. Select **Explanation type** as **Phrase list**.
7. Enter the text as "service start date of".
8. Add the text "service start date of" under Phrase list.

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/16.png)

9. Click **Save and train**.

Verify the evaluation matches.

![](/media/2022-12-20-unstructured-doc-processing-syntex/17.png)


**Plan Type**

We can extract the plan type by applying before and after labels as shown below:

![](/media/2022-12-20-unstructured-doc-processing-syntex/18.png)


## Apply the model

As a next step, we need to apply the model to a document library to start processing files automatically.

1. Click **+ Add library**
2. Select SharePoint site and document library.

    ![](/media/2022-12-20-unstructured-doc-processing-syntex/19.png)

3. Click **Add**.


## End User Experience

Once the documents are uploaded to the designated document library, the custom model will be applied on it to extract the information.

The result will look as follows:

![](/media/2022-12-20-unstructured-doc-processing-syntex/20.png)


## Summary

Unstructured document processing model works best for unstructured documents, such as letters or contracts which does not have any predefined format. Unstructured document processing model uses artificial intelligence (AI) to process documents.


## References

- [Overview of unstructured document processing in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/document-understanding-overview)
- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Create a document processing custom model](https://learn.microsoft.com/en-us/ai-builder/create-form-processing-model?WT.mc_id=M365-MVP-5003693)
- [Use sample data to do document processing](https://learn.microsoft.com/en-us/ai-builder/form-processing-sample-data?WT.mc_id=M365-MVP-5003693)
- [Overview of model types in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/model-types-overview?WT.mc_id=M365-MVP-5003693)
- [Create an enterprise model in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/create-syntex-model?WT.mc_id=M365-MVP-5003693)
- [PnP - Syntex Samples repository](https://github.com/pnp/syntex-samples)
