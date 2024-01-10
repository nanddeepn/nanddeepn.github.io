---
title: "Introducing SharePoint Embedded for Document management in your apps"
date: "2024-01-10"
share: true
header:
  image: media/2024-01-10-intro-sharepoint-embedded/architecture-overview.png
  teaser: media/2024-01-10-intro-sharepoint-embedded/architecture-overview.png
categories:
  - SharePoint
tags:
  - "2024"
  - January 2024
last_modified_at: 2024-01-10T00:00:00-00:00
---
## Introduction

Microsoft SharePoint Embedded is like a smart storage system in the cloud for files and documents. It's designed for use in different apps, and developers can use it to make their apps work with Microsoft 365's storage. It's great for big businesses making specialized apps and for software developers creating apps that can be used by multiple users.

In this article, we will have an overview of Microsoft SharePoint Embedded as a smart storage for your apps.


## Headless CMS

Headless simply means no User Interface. SharePoint Embedded brings in a new idea of a storage box that custom app creators can use to keep their files and documents safe. These files can only be reached through this special box. Developers use familiar Microsoft Graph APIs to get to the files and documents inside these storage boxes.

People who make SharePoint Embedded apps, whether they're developers or ISVs, are often called providers. The ones using these apps are usually called consumers.

When a consumer installs or registers a SharePoint Embedded app in their Microsoft 365 account, SharePoint Embedded makes a special storage area just for that app. This storage area doesn't have a user interface, but the files inside can only be reached through APIs. This way, all the files are available to the app creator, but they stay in the user's Microsoft 365 account. In this new storage space in a Microsoft 365 account, a SharePoint Embedded app can make lots of "File Storage Containers" to keep things organized.

Every document in the storage area made by the SharePoint Embedded app is in the user's Microsoft 365 account. This means that all the rules and settings set by the user for their Microsoft 365 account apply to these documents.


## SharePoint Embedded App Architecture

In SharePoint Embedded, all files and documents are kept in special boxes called Containers. These boxes are made and kept in a Microsoft 365 account. The ISV or LOB app uses the Microsoft Graph API to work with its own special box(es). Even if the app is set up in different ways, these boxes still belong to the Microsoft 365 user's account.

![](/media/2024-01-10-intro-sharepoint-embedded/architecture-overview.png)

Reference: [SharePoint Embedded App Architecture](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/concepts/app-concepts/app-architecture?WT.mc_id=M365-MVP-5003693)

Each Container is identified by a Container Type. Every storage box in SharePoint Embedded has a label called "Container Type." Each label is connected to one app, and each app can have only one label.

The main job of a Container Type is to control how the app can use the storage boxes. The label decides what the app can do with all the boxes of that type, like creating, reading, writing, or deleting boxes, and managing who can use them.


## Use Cases

Here are a few scenarios where SharePoint embedded can be used effectively:

- SharePoint as storage: I have seen many developers using SharePoint as a backend system to store files and documents. SharePoint Embedded can be used in such cases as Storage as a service.
- Document upload solution: SharePoint Embedded being headless, it can be used to hide the underlying document storage and build an app with a UI layer that uses SharePoint Embedded as storage.
- Custom Applications: Developers can use SharePoint Embedded to integrate customized file and document management features into their applications tailored to specific business needs.
- Line of Business (LOB) Apps: Enterprises building LOB applications can leverage SharePoint Embedded to enhance document storage and management capabilities within their business software.
- Multi-Tenant Applications: Independent Software Vendors (ISVs) creating applications for multiple users or tenants can benefit from SharePoint Embedded to provide scalable and secure file storage solutions.
- Collaborative Work: Teams working on projects can utilize SharePoint Embedded to streamline document collaboration, ensuring everyone has access to the latest files and versions.
- Cloud-Based Storage: As a cloud-based solution, SharePoint Embedded offers convenient and accessible storage for applications, reducing the need for on-premises infrastructure.
- Integration with Microsoft 365: SharePoint Embedded seamlessly integrates with Microsoft 365, allowing applications to leverage the robust features and capabilities of the Microsoft productivity suite.

In essence, SharePoint Embedded is versatile and can be applied in various contexts where efficient file and document management is essential for application functionality.


## Pricing

With SharePoint Embedded, you pay for what you use through a pay-as-you-go (PAYG) system using an Azure subscription. The bill depends on how much data you store, the transactions you make to change things, and the data you send out while using an application built on SharePoint Embedded.

For more information, please visit [Microsoft SharePoint Embedded ("SharePoint Embedded") Paid Public Preview Terms and Conditions](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/terms-of-service?WT.mc_id=M365-MVP-5003693).


## Summary

Microsoft SharePoint Embedded is like a smart storage system in the cloud for files and documents. It's designed for use in different apps, and developers can use it to make their apps work with Microsoft 365's storage.


## References

- [Overview of SharePoint Embedded](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/overview?WT.mc_id=M365-MVP-5003693)
- [Announcing SharePoint Embedded Public Preview at ESPC23](https://techcommunity.microsoft.com/t5/sharepoint-premium-blog/announcing-sharepoint-embedded-public-preview-at-espc23/ba-p/3993428?WT.mc_id=M365-MVP-5003693) by Zach Rosenfield
