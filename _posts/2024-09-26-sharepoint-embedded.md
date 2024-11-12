---
title: "Getting started with SharePoint Embedded"
date: "2024-09-10"
share: true
header:
  image: media/2024-09-26-sharepoint-embedded/01.png
  teaser: media/2024-09-26-sharepoint-embedded/01.png
categories:
  - SharePoint
tags:
  - "2024"
  - September 2024
last_modified_at: 2024-09-26T00:00:00-00:00
---
## Introduction

SharePoint Embedded is a cutting-edge, API-only solution designed to integrate the robust capabilities of Microsoft 365's file and document storage platform into any application. This innovative approach is particularly beneficial for enterprises developing line-of-business applications and independent software vendors (ISVs) creating multi-tenant applications.

In this article, we will have an overview of SharePoint Embedded, its key features, and use cases.


## Key Features

Below are the key features of SharePoint Embedded:

- **App Documents in Microsoft 365 Tenant:** SharePoint Embedded creates a dedicated storage partition within the user's Microsoft 365 tenant. This partition is accessible only via APIs, ensuring that documents remain secure and isolated within the tenant.

- **File Storage Containers:** Applications using SharePoint Embedded utilize Microsoft Graph APIs to store files in "File Storage Containers." These containers function similarly to Document Libraries in SharePoint but are designed for API access only. Each container can store multiple terabytes of content and is managed separately, providing granular control over permissions.

- **App-Managed Content Experiences:** Content stored by SharePoint Embedded applications is accessible exclusively through the owning application. This setup allows developers to leverage Microsoft 365's rich content management features, such as automatic versioning, search, sharing, and more.

- **Integration with Microsoft 365 Features:** SharePoint Embedded integrates seamlessly with other Microsoft 365 services, including Office collaboration tools, Microsoft Purview's security and compliance features, and Copilot capabilities. This integration enhances the overall functionality and security of the applications.


## Benefits

Below are the benefits of using SharePoint Embedded:

- **Enhanced Security and Compliance**: By keeping documents within the Microsoft 365 tenant, SharePoint Embedded ensures that all data is protected by Microsoft's robust security and compliance measures.

- **Scalability:** The ability to create multiple File Storage Containers within a tenant allows applications to scale efficiently, accommodating growing data storage needs without compromising performance.

- **Developer Flexibility:** Developers can build custom user experiences and manage content directly through their applications, providing a tailored experience for end-users while leveraging the powerful backend capabilities of Microsoft 365.


## Use Cases

Here are some use cases where SharePoint Embedded can be effectively used:

- **Enterprise Applications:** Enterprises can use SharePoint Embedded to manage internal documents and files securely, ensuring compliance with organizational policies and regulations.

- **ISV Solutions:** ISVs can integrate SharePoint Embedded into their multi-tenant applications, offering customers a secure and scalable document management solution that leverages Microsoft 365's advanced features.


## File Storage Containers

SharePoint Embedded applications use Microsoft Graph APIs to save files and documents in a new type of storage called a "File Storage Container" or simply "Container". These containers are like a specialized version of a SharePoint Document Library, but with some differences. Your app can create many containers in each environment, and you can control who has access to each one separately, storing a large amount of files - up to several terabytes. The files and documents your app relies on are kept separate and secure within that specific environment.


## Setting up Containers via VS Code

**Install extension**

1. Open Visual Studio Code.
2. From left menu, click **Extensions**.
3. Search and install the extension - **SharePoint Embedded**.

    ![](/media/2024-09-26-sharepoint-embedded/01.png)


**Sign-in to SharePoint Embedded extension**

1. In the VS Code, from left menu, click **SharePoint Embedded**.
2. Click **Sign into Microsoft 365**.

    ![](/media/2024-09-26-sharepoint-embedded/02.png)

3. Authenticate with SharePoint admin and provide the consent.

    ![](/media/2024-09-26-sharepoint-embedded/03.png)


**Create a container type**

You can create a container type for testing or development without connecting it to any Azure billing account. This lets developers try out SharePoint Embedded app development and explore its features at no cost.

1. In the VS Code, from left menu, click **SharePoint Embedded**.
2. Click **Create Trial Container Type**.

    ![](/media/2024-09-26-sharepoint-embedded/04.png)

3. Follow the steps to give your container type a name. You can always change the name later if needed.


**Create a Microsoft Entra ID App**

Each container type is linked to a Microsoft Entra ID application. When creating a free trial container type, the first thing you need to do is either create a new Microsoft Entra ID application or choose one you already have to own the container. You can either name the new app or select one from your existing applications.

![](/media/2024-09-26-sharepoint-embedded/05.png)


**Register the container type**

Now, we need to register the container type on the local tenant for a SharePoint Embedded application to interact with containers in a consuming tenant.

1. Right click on the container type and select **Register on local tenant**.

    ![](/media/2024-09-26-sharepoint-embedded/06.png)

2. Provide the admin consent as requested.


**Create first container**

You can create up to five containers of container type to upload and manage content.

1. Right-click on the Containers drop-down and select **Create container**.

    ![](/media/2024-09-26-sharepoint-embedded/07.png)

2. Name the container.

    ![](/media/2024-09-26-sharepoint-embedded/08.png)


## Graph API Support

The Graph APIs supports [fileStorageContainer resource type](https://learn.microsoft.com/en-us/graph/api/resources/filestoragecontainer?WT.mc_id=M365-MVP-5003693) to work with the file storage container.


## Summary

SharePoint Embedded represents a significant advancement in document management solutions, providing a secure, scalable, and flexible platform for integrating Microsoft 365's powerful features into any application. Whether for enterprise use or ISV solutions, SharePoint Embedded offers a robust and efficient way to manage and store documents within the Microsoft 365 ecosystem.


## References

- [SharePoint Embedded Overview](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/overview?WT.mc_id=M365-MVP-5003693)
- [Introducing SharePoint Embedded: Microsoft 365 features for Your Marketplace App](https://techcommunity.microsoft.com/t5/marketplace-blog/introducing-sharepoint-embedded-microsoft-365-features-for-your/ba-p/4082069?WT.mc_id=M365-MVP-5003693)
- [SharePoint Embedded for Visual Studio Code](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/getting-started/spembedded-for-vscode?WT.mc_id=M365-MVP-5003693)
- [Register file storage container type application permissions](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/concepts/app-concepts/register-api-documentation?WT.mc_id=M365-MVP-5003693)
