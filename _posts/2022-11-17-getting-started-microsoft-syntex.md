---
title: "Getting started with Microsoft Syntex"
date: "2022-11-17"
share: true
header:
  image: media/2022-11-17-getting-started-microsoft-syntex/06.png
  teaser: media/2022-11-17-getting-started-microsoft-syntex/06.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2022"
  - November 2022
last_modified_at: 2022-11-17T00:00:00-00:00
---
## Overview

Microsoft Syntex, officially announced during Microsoft Ignite 2022 is a content understanding, processing, and compliance service.

In this article, we will have an overview of Microsoft Syntex, set up, and create Pre-built model.

## Microsoft Syntex Overview

Over the time organizations have huge content in terms of files or documents, which is impossible for a human being to analyze and classify. Clicking and sorting through these huge files is difficult.

Microsoft Syntex uses AI capabilities to analyze the content and organize it better to understand and classify. It extracts, analyzes, and categorizes the data.


## Models

Microsoft Syntex works on the concept of models to identify and classify the documents. It has prebuilt models, as well you can create your custom model based on type or structure of files as well the information you want to extract.


**Custom models**

You can build custom model to identify and classify documents uploaded to SharePoint document libraries. Model is associated with content type and stores the extracted information in columns.

The examples of custom models include:

- **Unstructured document processing:** These documents must have text that can be identified based on phrases or patterns. (e.g. Contract renewal document with text - Service start date of `<date>`).
- **Freeform document processing**: The information can appear anywhere in the document.
- **Structured document processing**: They follow a pre-defined format.


**Prebuilt models**

These models are pre-trained to recognize documents. The examples include:

- Invoice processing
- Receipt processing


## Features of Syntex

Microsoft Syntex offers useful features as follows:

**Templates**

With Syntex, you can create modern templates based on most used business documents. For e.g., contracts, statements of work, service agreements, letters of consent, and correspondence.

**Advanced metadata search**

Syntex helps you make metadata-based queries on SharePoint document libraries.

**Compliance**

Microsoft Syntex helps you apply retention and sensitivity labels on the document.


## Licensing

Your organization should have license for Microsoft Syntex. The users should have the licenses assigned including below apps:

- Syntex
- Syntex - SPO type
- Common Data Service for Syntex


## Set up Microsoft Syntex

Follow below steps to set up Microsoft Syntex:

1. Open Microsoft 365 admin center.
2. From left menu, click **Setup**.
3. Under Files and content, click **Automate content understanding**.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/01.png)

4. Click **Get started**.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/02.png)

5. Under **Configure AI Builder model creation**, choose any of below option to create structured and freeform document processing models on SharePoint sites.

    a. **All SharePoint sites**: make it available to all SharePoint libraries in your organization.

    b. **Libraries in selected SharePoint sites**: select the sites in which you want to make it available or upload a list of up to 50 sites.

    c. **No SharePoint libraries**: don't want to make it available to any sites.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/03.png)

6. Specify the **Content center name**. SharePoint site will be created with the specified name as a default content center.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/04.png)

7. Review your configuration and click **Activate**.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/05.png)

The default content center will be created at the configured URL.

![](/media/2022-11-17-getting-started-microsoft-syntex/06.png)

If needed, you can create additional content centers from SharePoint admin center.

![](/media/2022-11-17-getting-started-microsoft-syntex/07.png)


## Create Pre-built model

Follow below steps to create pre-built model:

1. On the Content center site, click **New** > **Model**.
2. Under Set up a prebuilt model, select any model (for e.g., Invoice processing).

    ![](/media/2022-11-17-getting-started-microsoft-syntex/08.png)

3. Name your model.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/09.png)

4. Click **Create**.


## Test the model

Once the model is created, follow below steps to test it.

1. From Content center site's top navigation, click **Models**.
2. Click the created model.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/10.png)

3. The model will open as follows:

    ![](/media/2022-11-17-getting-started-microsoft-syntex/11.png)

4. Click **Add a file** to add your invoice.
5. Select the extractors.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/12.png)

6. Click **Apply model** to select the document library in SharePoint site, where you want to apply this model.

    ![](/media/2022-11-17-getting-started-microsoft-syntex/13.png)

In the document library, select the document(s), click Classify and extract to let Microsoft Syntex to extract out the information from your documents. 

> Note:
> This usually takes 30 minutes to process the information and extract the metadata.

![](/media/2022-11-17-getting-started-microsoft-syntex/14.png)


## Summary

Microsoft Syntex, officially announced during Microsoft Ignite 2022 is a content understanding, processing, and compliance service. Microsoft Syntex uses AI capabilities to analyze the content and organize it better to understand and classify. It extracts, analyzes, and categorizes the data.


## References

- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Overview of model types in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/model-types-overview?WT.mc_id=M365-MVP-5003693)
- [Create an enterprise model in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/create-syntex-model?WT.mc_id=M365-MVP-5003693)
