---
title: "Information Barriers Overview"
date: "2021-08-02"
share: true
header:
  image: media/2021-08-02-information-barriers-overview/01.png
  teaser: media/2021-08-02-information-barriers-overview/01.png
categories:
  - Office 365
tags:
  - "2021"
  - August 2021
last_modified_at: 2021-08-02T00:00:00-00:00
---
## Overview

Microsoft 365 enables communication and collaboration between various users and groups across the organization. In some scenarios, there might be a need to restrict the communication or collaboration between certain groups in the organization.

In this article, we will get introduced to Information Barriers (IB) and how it helps to restrict communication between groups in an organization.

## Information Barriers (IB)

Information Barriers are useful in scenarios including below, when we need to restrict the communication between groups due to business policies and information handling:

1. In Banking sector, restrict communications between investment bankers and financial advisors.
2. In Legal firm, maintaining confidentiality of data being worked on by lawyers for various their clients.
3. In general, communication between Sales and Research departments in an organization.

Below is an example of IB in action:

![](/media/2021-08-02-information-barriers-overview/01.png)

Image reference: [https://docs.microsoft.com/en-us/microsoftteams/information-barriers-in-teams](https://docs.microsoft.com/en-us/microsoftteams/information-barriers-in-teams?WT.mc_id=M365-MVP-5003693)

Information barriers applies to Microsoft Teams, SharePoint Online, and OneDrive.

## Licenses Requirements

Information barriers are available in below subscriptions:

- Microsoft 365 E5/A5
- Office 365 E5/A5
- Office 365 Advanced Compliance
- Microsoft 365 Compliance E5/A5
- Microsoft 365 Insider Risk Management

Make a note that, it is not available on E5 developer subscription.

To define the Information barriers, one should have any of below role assigned:

- Microsoft 365 global administrator
- Office 365 global administrator
- Compliance administrator
- IB Compliance Management

## Information Barriers Configuration

The configuration of Information barriers involves below steps:

1. Pre-requisites
    - Make sure you have below things in place:
    - Organization structure is reflected in directory data.
    - Enable scoped directory search in Microsoft Teams
    - EXO license
    - Audit logging
    - No address book policies
    - PowerShell modules (Azure, Security and Compliance)
2. Identify segments in your organization and segment the users.
3. Create and configure information barrier policies
4. Apply information barrier policies

## Summary

Information Barriers are useful to restrict the communication between groups due to business policies and information handling. Information barriers applies to Microsoft Teams, SharePoint Online, and OneDrive.

## References

- [Information barriers in Microsoft 365](https://docs.microsoft.com/en-us/microsoft-365/compliance/information-barriers-solution-overview?WT.mc_id=M365-MVP-5003693)
