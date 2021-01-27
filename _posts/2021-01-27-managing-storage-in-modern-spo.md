---
title: "Managing storage for your sites in Modern SharePoint Online"
date: "2021-01-11"
share: true
header:
  image: media/2021-01-27-managing-storage-in-modern-spo/03.png
  teaser: media/2021-01-27-managing-storage-in-modern-spo/03.png
categories:
  - SharePoint
tags:
  - "2021"
  - January 2020
last_modified_at: 2021-01-27T00:00:00-00:00
---

## Overview

With Office 365 subscription, we get storage space for SharePoint Online based on a number of licenses. As we create SharePoint online sites and start using them by adding our content, we start to consume the assigned storage space. It is important for us to keep a track of consumed and available site storage.

In this article, we will explore the site storage experience in modern SharePoint and it&#39;s configuration.

## Site Storage in SharePoint Online

The modern SharePoint online admin center gives an easy option to create new site collections.

![](/media/2021-01-27-managing-storage-in-modern-spo/01.png)

Just a few simple fields to fill in and the site will be ready in few seconds! But did you notice? We do not have a field to specify the storage quota for our site!

The Active sites page in the SharePoint Online admin center though shows the storage used for each site. Also, we can see the total storage used and available for use.

![](/media/2021-01-27-managing-storage-in-modern-spo/02.png)

Let us see from where site storage can be configured.

## Site storage configuration

In the SharePoint admin center, from the left menu click **Settings** > **Site storage limits**.

![](/media/2021-01-27-managing-storage-in-modern-spo/03.png)

The default storage limit is **Automatic**.

The storage limit can be configured as:

- **Automatic:** The default. All sites get the storage from a central pool. The global or SharePoint administrator does not have to manage the storage per site. It is handled automatically by SharePoint for them.
- **Manual:** The global or SharePoint administrator can have a fined tuned site storage allocated for each of the SharePoint site collection.

## Manage site storage for individual sites

If you want to manage the site storage per site, you will have to go with the manual configuration option.

With the manual option, the administrator can configure the maximum storage per site and can also set the notification for site owners when a certain percentage of the site storage limit is consumed.

![](/media/2021-01-27-managing-storage-in-modern-spo/04.png)

The site owners or collection administrators can check the storage quota by following the below steps:

1. Navigate to the **site settings** page.
2. Under **Site Collection Administration**, click **Storage Metrics**.

![](/media/2021-01-27-managing-storage-in-modern-spo/05.png)

> Note: 
> The recycle bin count towards the site quota.

## PowerShell support

The site quota can be set with PowerShell:

```
Set-SPOSite -Identity https://contoso.sharepoint.com -StorageQuota 1500 -StorageQuotaWarningLevel 1400
```

## Summary

With Office 365 subscription, we get storage space for SharePoint Online based on a number of licenses. It is important for us to keep a track of consumed and available site storage. We explored the site storage experience in modern SharePoint and it&#39;s configuration.

## Reference
- [Manage site storage limits](https://docs.microsoft.com/en-us/sharepoint/manage-site-collection-storage-limits?WT.mc_id=M365-MVP-5003693)
- [SharePoint Online limits](https://docs.microsoft.com/en-us/office365/servicedescriptions/sharepoint-online-service-description/sharepoint-online-limits?WT.mc_id=M365-MVP-5003693)
- [Add storage space for your subscription](https://docs.microsoft.com/en-us/microsoft-365/commerce/add-storage-space?WT.mc_id=M365-MVP-5003693)