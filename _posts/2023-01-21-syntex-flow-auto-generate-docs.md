---
title: "Microsoft Syntex and Power Automate to auto-generate documents"
date: "2023-01-21"
share: true
header:
  image: media/2023-01-21-syntex-flow-auto-generate-docs/02.png
  teaser: media/2023-01-21-syntex-flow-auto-generate-docs/02.png
categories:
  - Office 365
  - Microsoft Syntex
tags:
  - "2023"
  - January 2023
last_modified_at: 2023-01-21T00:00:00-00:00
---
## Overview

This article is in continuation with my previous article [Content Assembly in Microsoft Syntex](/posts/2023-01-05-content-assembly-syntex/), where we explored the content assembly feature of Microsoft Syntex to generate a large set of documents with repetitive content.

In this article, we will explore how Power Automate flow can help us to automatically generate standard repetitive documents using modern templates.


## Business case

Let us consider a business case, where Contoso maintains the vendors' renewal information in a SharePoint list. When there will be a time to renew the contract with the vendor, the concerned Contoso department will add an item to the SharePoint list. This action will be picked by Power Automate flow to generate the contract renewal document based on the information provided.


## SharePoint list for contract renewal

We will start by creating a SharePoint list (named Vendor Details) with the below schema to raise a request for generating the contract renewal document.

| **#** | **Column Name** | **Type** | **Description** |
| --- | --- | --- | --- |
| 1 | Client Name | Single line of text | Name of client. E.g., the department at Contoso. (Reuse OOB Title column) |
| 2 | Contractor Name | Single line of text | Name of the contractor whose services are used by Contoso. |
| 3 | Contract Date | Date and Time | Date of renewal for the contract. |
| 4 | Services Provided | Multiple lines of text | Services of the Contractor being used by the Client. |
| 5 | Compensation | Single line of text | Compensation from Client to Contractor for the contract. |


## Power Automate

We will now set up a Power Automate flow which will trigger when an item is added to the Vendor Details SharePoint list.

Follow the below steps in the flow:

1. Add an action **Generate document using Syntex (preview)**.
2. Select the SharePoint site and then the document library containing the modern template.
3. After the template selection, you will see the template fields which need to be mapped with the related column from the SharePoint list.

    ![](/media/2023-01-21-syntex-flow-auto-generate-docs/01.png)

Below are the formulas used in the fields:

| **Field** | **Formula** |
| --- | --- |
| File Name | concat(triggerOutputs()?['body/Title'], ' - ', triggerOutputs()?['body/ContractorName']) |
| Contract Date | formatDateTime(triggerOutputs()?['body/ContractDate'], 'dd/MM/yyyy') |


## Test the scenario!

Add an item to the SharePoint list with the contract renewal details. This should trigger a flow and generate the document in the specified document library using a modern template.

![](/media/2023-01-21-syntex-flow-auto-generate-docs/02.png)


## Summary

The Power Automate flow and Content Assembly feature of Microsoft Syntex can help us automatically generate standard repetitive documents using modern templates.


## References

- [Overview of Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/syntex-overview?WT.mc_id=M365-MVP-5003693)
- [Overview of content assembly in Microsoft Syntex](https://learn.microsoft.com/en-us/microsoft-365/contentunderstanding/content-assembly?WT.mc_id=M365-MVP-5003693)
- [Automate document generation with Microsoft Syntex and Power Automate (Preview)](https://learn.microsoft.com/en-us/microsoft-365/syntex/automate-document-generation?WT.mc_id=M365-MVP-5003693)
