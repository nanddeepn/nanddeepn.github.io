---
title: "Configure Viva Connections for MS Teams Desktop"
date: "2021-10-27"
share: true
header:
  image: media/2021-10-28-configure-viva-connections/14.png
  teaser: media/2021-10-28-configure-viva-connections/14.png
categories:
  - Microsoft Viva
  - MS Teams
  - SharePoint
tags:
  - "2021"
  - October 2021
last_modified_at: 2021-10-28T00:00:00-00:00
---
## Overview

Viva Connections is a modern gateway designed to engage everyone and stay informed. It gives everyone a personalized way to discover the content.

In this article, we will explore setting up Viva connections.

## Why Viva connections?

Viva connections brings in the following advantages:

- Brings in optimized experience in Microsoft Teams for information workers and frontline workers.
- Provides curated content to employees
- Built on existing capabilities in Microsoft 365 including SharePoint, Yammer, and Stream

Viva Connections provides mobile and desktop experiences.

## Pre-requisites

Below are the minimum roles required to set up Viva connections:

1. SharePoint Admin
2. MS Teams Admin


## Step 1: Set up SharePoint Home Site

Start by setting up a Communication or a Team site. Take benefits of templates available from Microsoft or your organization.

![](/media/2021-10-28-configure-viva-connections/01.png)

Set up the site as a home site by following below steps:

1. Open SharePoint admin center.
2. From the left menu, click **Settings**.
3. Select **SharePoint** > **Home site**.
4. Specify the **URL of the site you want to use** as a SharePoint Home site.
5. Click **Save**.

    ![](/media/2021-10-28-configure-viva-connections/02.png)

> Note:
> It may take up to 10 minutes for new home site URLs to take effect.

Once set, you will be able to see the home site URL reflected in the settings.

![](/media/2021-10-28-configure-viva-connections/03.png)

The home site settings menu reflects the home site capabilities, like Global navigation creation and Viva Connections setup options.

![](/media/2021-10-28-configure-viva-connections/04.png)

> Note:
> A home site is required before we enable and customize the SharePoint app bar.


## Step 2: Enable Global Navigation and SharePoint app bar

Viva Connections uses elements from the SharePoint app bar and global navigation for desktop and mobile experiences. Global navigation can be enabled and customized in the SharePoint app bar.

Follow the below steps to enable Global navigation:

1. On the SharePoint Home site, click **Settings** > **Global navigation**.
2. On the Global navigation settings pane, configure the below settings:
    1. Click **Enable global navigation**.
    2. Upload the **Logo** for global navigation.
    3. Specify the **Title** to appear at the top of the global navigation pane.
    4. In the **Navigation source** , specify the source as either **Home site navigation** or **Hub or global navigation**.

![](/media/2021-10-28-configure-viva-connections/05.png)


## Step 3: Set up Viva Connections Dashboard web part

Dashboard includes cards that allow users to access useful information. Once set up, the Dashboard web part can be used on the SharePoint Home site.

Follow the below steps to enable Global navigation:

1. On the SharePoint Home site, click **Settings** > **Set up Viva Connections (Preview)**.
2. On the **Set up Viva Connections (Preview)** pane, click **+ Create Dashboard**.
3. Start by adding cards on the dashboard for mobile and desktop views.

    ![](/media/2021-10-28-configure-viva-connections/06.png)

4. Once the cards are added, click **Publish**.
5. After the Dashboard is set up, on the Home page, add **Dashboard (Preview) for Viva Connections** web part.

    ![](/media/2021-10-28-configure-viva-connections/07.png)


## Step 4: Enable the Viva Connections app in the Microsoft Teams admin center

Once we are prepared our intranet for Viva Connections in SharePoint, we are ready to add the Viva Connections app from the Microsoft Teams admin center.

Follow the below steps:

1. Open [Microsoft Teams admin center](https://admin.teams.microsoft.com/).
2. From the left navigation, click **Teams apps** > **Manage apps**.
3. Search and select **Viva Connections**.

    ![](/media/2021-10-28-configure-viva-connections/08.png)

4. Click **Allow**. The Viva Connections app is blocked by default.

    ![](/media/2021-10-28-configure-viva-connections/09.png)


**Customize the app**

1. Under **Actions**, click **Customize**.

    ![](/media/2021-10-28-configure-viva-connections/10.png)

2. From the **Customize** panel, update the needed details.

    ![](/media/2021-10-28-configure-viva-connections/11.png)


## Step 5: Pin the Personal App

Now, we will pin the Viva connections app as a personal app to all the users by following the below steps.

1. Open Microsoft Teams admin center ([https://admin.teams.microsoft.com](https://admin.teams.microsoft.com/)).
2. From the left navigation, click **Teams apps** > **Setup policies**.
3. Click an existing policy **Global (Org-wide default)**.
4. Under **Pinned apps** , click **Add apps**.
5. **Search by name** as Viva Connections.
6. Click **Add**.

    ![](/media/2021-10-28-configure-viva-connections/12.png)

7. Move the Viva Connections app to the top of the list.

    ![](/media/2021-10-28-configure-viva-connections/13.png)


## Enjoy the Viva Connections in Microsoft Teams

Start using Viva Connections in Microsoft Teams as it will be pinned as a personal app to all the users in an organization.

![](/media/2021-10-28-configure-viva-connections/14.png)


## Summary

Viva Connections is a modern gateway designed to engage everyone and stay informed. It gives everyone a personalized way to discover the content.


## References

- [Overview: Viva Connections (Preview)](https://docs.microsoft.com/en-us/sharepoint/viva-connections-overview?WT.mc_id=M365-MVP-5003693)
- [Set up and launch Viva Connections (Preview)](https://docs.microsoft.com/en-us/sharepoint/guide-to-setting-up-viva-connections?WT.mc_id=M365-MVP-5003693)
- [Introduction to the SharePoint app bar](https://docs.microsoft.com/en-us/sharepoint/sharepoint-app-bar?WT.mc_id=M365-MVP-5003693)
- [Add the Viva Connections app in the Teams Admin Center (Preview)](https://docs.microsoft.com/en-us/SharePoint/add-viva-connections-app?WT.mc_id=M365-MVP-5003693)
