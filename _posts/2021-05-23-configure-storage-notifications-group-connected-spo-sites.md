---
title: "Configure Site Storage Notifications for Group Connected SharePoint Sites"
date: "2021-05-23"
share: true
header:
  image: media/2021-05-23-configure-storage-notifications-group-connected-spo-sites/01.png
  teaser: media/2021-05-23-configure-storage-notifications-group-connected-spo-sites/01.png
categories:
  - SharePoint
tags:
  - "2021"
  - May 2020
last_modified_at: 2021-05-23T00:00:00-00:00
---

## Overview

In the previous article, we have explored [Managing storage for your sites in Modern SharePoint Online](https://nanddeepnachanblogs.com/posts/2021-01-27-managing-storage-in-modern-spo/). Ideally site owners should receive the notifications by email when the site reaches the % storage limit specified in the configuration. This works well as expected for the Communication sites. However, it does not work out of the box for Group connected SharePoint sites.

In this article, we will explore why it does not work out of the box for Group connected SharePoint sites and the configurations to make it work.

## Communication Site - Storage notification

When the site storage notification is set on the Communication site, the owners receive an email notification as follows:

![](/media/2021-05-23-configure-storage-notifications-group-connected-spo-sites/01.png)

## Group Connected SharePoint Site – Storage notification

The Group connected SharePoint (Team) site owners does not get this notification by default. These email notifications are sent by the timer job. The schedule is unknown but let us try to find out the cause with below facts at hand:

1. The email notification is being sent from an email [no-reply@sharepointonline.com](mailto:no-reply@sharepointonline.com)
2. We have tentative time window of email received for the Communication site.


**Troubleshooting the email**

We will carry out message trace to find more details and cause.

1. Open Exchange online admin center.
2. Under **Mail flow**, click **Message trace**.
3. Click **Start a trace**.

    ![](/media/2021-05-23-configure-storage-notifications-group-connected-spo-sites/02.png)

4. In the message trace,
    1. Specify the **Sender** as [no-reply@sharepointonline.com](mailto:no-reply@sharepointonline.com)
    2. Specify the O365 group email address as **Recipients**.
    3. Specify the time range based on the email received for Communication site.
    4. Specify the report type.
    5. Click **Search**.

Analyze the message trace for a potential cause. The error reported is as follows:

> UnifiedGroupAgent; Delivery failed because the sender isn't a group member or external senders aren't permitted to send to this group.

From the error, it is evident that Office 365 Group is not allowing external senders (in our case [no-reply@sharepointonline.com](mailto:no-reply@sharepointonline.com)

## Office 365 Group settings

We need to ensure that emails from [no-reply@sharepointonline.com](mailto:no-reply@sharepointonline.com) to Office 365 Group are accepted. Follow below steps to configure Office 365 Group settings:

1. Open Microsoft 365 admin center.
2. From left menu, click **Group** > **Active groups**.
3. Search and select the group.
4. Click **Settings** tab.
5. Under **General** settings, select **Allow external senders to email this group**.
6. Click **Save**.

![](/media/2021-05-23-configure-storage-notifications-group-connected-spo-sites/03.png)

## Summary

Storage notifications does not work out of the box for Group connected SharePoint sites. It needs some extra configurations at each Office 365 group level in order to make it work.


## Reference
- [Manage site storage limits](https://docs.microsoft.com/en-us/sharepoint/manage-site-collection-storage-limits?WT.mc_id=M365-MVP-5003693)
- [SharePoint Online limits](https://docs.microsoft.com/en-us/office365/servicedescriptions/sharepoint-online-service-description/sharepoint-online-limits?WT.mc_id=M365-MVP-5003693)
- [Add storage space for your subscription](https://docs.microsoft.com/en-us/microsoft-365/commerce/add-storage-space?WT.mc_id=M365-MVP-5003693)