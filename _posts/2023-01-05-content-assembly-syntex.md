---
title: "Content Assembly in Microsoft Syntex"
date: "2023-01-05"
share: true
header:
  image: media/2023-01-05-content-assembly-syntex/06.png
  teaser: media/2023-01-05-content-assembly-syntex/06.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2023"
  - January 2023
last_modified_at: 2023-01-05T00:00:00-00:00
---
## Overview

Generating a large set of documents with the same content but targeted for different entities is a time-consuming and error-prone task. E.g., generating a contract renewal document for multiple vendors with the same content but just changing the vendor's name and a piece of specific information at certain places in the document.

In this article, we will explore the feature of Microsoft Syntex called Content Assembly that helps to automatically generate repetitive business documents. E.g., contracts, statements of work, and service agreements.


## Content assembly overview

The content assembly feature of Microsoft Syntex helps to automatically generate repetitive business documents by creating modern templates and using those templates to generate documents.

It is a 4-step process as follows:

1. Create a modern template from a file uploaded to SharePoint, OneDrive, or your local storage.
2. Associate fields in the template
3. Publish modern template
4. Create documents from a modern template


## Business case

Let us consider a business case to address with content assembly. Consider you are working for Contoso, an organization that works with multiple vendors as consultants who works as an expert in a specific area to advise Contoso. Consulting agreement is a contract between the Consultant (i.e., vendor) and Client (i.e., Contoso) which layouts the terms of the agreement.

The consulting agreement needs to be renewed for each vendor every year. Contoso spends a significant amount of time generating Consulting agreements which is time-consuming and error-prone work. Contoso is looking for automating this work.

For this article, I am using one such example of consulting agreement from LegalContract.com. You can download the same from here.

The document has the below placeholders

| **Placeholder** | **Description** |
| --- | --- |
| \<CONTRACT DATE\> | Date of renewal for the contract. |
| \<CLIENT NAME\> | Name of client. E.g., the department at Contoso. |
| \<CONTACTOR NAME\> | Name of the contractor whose services are used by Contoso. |
| \<SERVIES PROVIDED\> | Services of the Contractor being used by the Client. |
| \<COMPENSATION\> | Compensation from Client to Contractor for the contract. |

Let us follow the below steps to help Contoso automate its renewal process.


## Step 1: Create a modern template

Create a modern template by below steps:

1. Open any document library from any SharePoint site.
2. Click **New** \> **Create a modern template**.

    ![](/media/2023-01-05-content-assembly-syntex/01.png)

3. Select a word document to be used as a template from SharePoint, OneDrive, or your local device.
4. After uploading, the document will be displayed in the template studio. We need to add fields to this document to create a template out of it.

    ![](/media/2023-01-05-content-assembly-syntex/02.png)

5. You can rename the template by clicking the pencil icon on the top left corner. By default, it takes the file name as a template name.


### Create fields

After having a modern template, we need to define multiple fields that can be mapped at multiple occurrences in the template. E.g., \<CLIENT NAME\> appears at multiple places in the document (aka modern template).

Follow the below steps to create a field.

1. Click **+ New field**.
2. Name the field.
3. Select the content in a modern template.

    ![](/media/2023-01-05-content-assembly-syntex/03.png)

4. Click **Next**.
5. In the **How authors fill this field** section, define how the authors will be able to fill in this field.

    ![](/media/2023-01-05-content-assembly-syntex/04.png)

6. If you select the authors to enter text or select a date (i.e., option 1), you will get the below options to configure.

    ![](/media/2023-01-05-content-assembly-syntex/05.png)

Follow the steps through this section to define all fields on the modern template, as shown below:

![](/media/2023-01-05-content-assembly-syntex/06.png)


### Publish a modern template

Click Publish to make the modern template available for everyone to create documents based on it. The modern template (Consulting Agreement) should be now visible under the New menu.

![](/media/2023-01-05-content-assembly-syntex/07.png)


## Create a document from a modern template

A modern template can be used to create similar documents. Follow the below steps to create a document from the modern template.

1. From the SharePoint document library, select **New**.
2. Select **Consulting Agreement**.
3. Fill in the needed fields.

    ![](/media/2023-01-05-content-assembly-syntex/08.png)

4. Click **Create document**. Name it and your document is ready to use.


## Edit modern template

To edit the modern template, follow the below steps:

1. From the SharePoint document library, select **New** \> **Edit New menu**.
2. Under the **modern templates** section, select the template to edit, delete, or unpublish.

    ![](/media/2023-01-05-content-assembly-syntex/09.png)


## Limitations

Below are the limitations to note:

1. Word documents should not have any comments or track changes enabled.
2. Images should not be text-wrapped.
3. A template is associated with a library where it is created.
4. Once a document is created from a template, it's not associated with the template.


## Summary

Generating a large set of documents with the same content but targeted for different entities is a time-consuming and error-prone task. The content assembly feature of Microsoft Syntex helps to automatically generate repetitive business documents by creating modern templates and using those templates to generate documents.


## References

- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Overview of content assembly in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/content-assembly?WT.mc_id=M365-MVP-5003693)
