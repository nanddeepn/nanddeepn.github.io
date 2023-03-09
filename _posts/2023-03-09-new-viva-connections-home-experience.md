---
title: "Explore New Viva Connections Home Experience"
date: "2023-03-09"
share: true
header:
  image: media/2023-03-09-new-viva-connections-home-experience/01.png
  teaser: media/2023-03-09-new-viva-connections-home-experience/01.png
categories:
  - Microsoft Viva
  - MS Teams
  - SharePoint
tags:
  - "2021"
  - October 2021
last_modified_at: 2023-03-09T00:00:00-00:00
---
## Introduction

The new Viva Connections home experience to Desktop has started to roll out in the targeted release tenants. This new experience makes it easy to roll out Viva Connections without having to set up the SharePoint home site.

In this article, we will explore the new Viva Connections home experience and its configuration.


## New Viva Connections home experience

With the new Viva Connections home experience, you no more need to set up the SharePoint home site. When added, the new experience is ready to use with branding, a dashboard, and resources.

![](/media/2023-03-09-new-viva-connections-home-experience/01.png)

There are a few points to consider:

- If you already have the Viva Connections set up with the home site:
  - The new home design will use the same content without any additional configurations needed.
  - Cards on Dashboard will be used from the previous configuration (home site).
  - Resources will be used from the previous configuration (home site).
  - Audience targeting on dashboard cards and Resources will be used from the previous configuration (home site).

You can choose the home site or new home experience by running the below command.

```powershell
Set-SPOHomeSite -HomeSiteUrl <Home site URL> -VivaConnectionsDefaultStart <boolean: $true / $false>
```

When the `VivaConnectionsDefaultStart` parameter is set to `true`, it will keep the Viva Connections landing experience to the SharePoint home site. If set to `false`, the new Viva Connections home experience will be used.


## Assign content author

One needs member-level or higher permission to edit the new desktop experience. On the right top corner click **…** and select **Manage access**.

![](/media/2023-03-09-new-viva-connections-home-experience/02.png)

Grant access to other users to manage the new desktop experience.

- Full control: Can edit all content and permission settings.
- Limited control: Can edit content in the Dashboard and Navigation.
- View only: Can view content.

![](/media/2023-03-09-new-viva-connections-home-experience/03.png)


## Provide a unique branding experience

To provide a unique branding experience, you can use a background image for the new experience.

Click **Edit** > **Change image** to select your branding image.

![](/media/2023-03-09-new-viva-connections-home-experience/04.png)

This will give you a file selection dialog experience as in SharePoint to select the image from stock images, web search, OneDrive, SharePoint site, or upload an image.

After you upload your branding image, you can reposition a focal point of the image to adjust it better and save it.

![](/media/2023-03-09-new-viva-connections-home-experience/05.png)


## Set up the Dashboard

The dashboard helps you place cards for your employees to manage their work better.

![](/media/2023-03-09-new-viva-connections-home-experience/06.png)

Clicking **Edit** next to the Dashboard section will take you to the dashboard page where you can add multiple cards to a dashboard for a desktop and mobile experience.

![](/media/2023-03-09-new-viva-connections-home-experience/07.png)


## Set up Resources

Resources define your navigation and wayfinding. Clicking **Edit** on the Resources section will help you customize the navigation.

![](/media/2023-03-09-new-viva-connections-home-experience/08.png)


## Summary

The new Viva Connections home experience to Desktop experience makes it easy to roll out Viva Connections without having to set up the SharePoint home site. If you already have set up Viva Connections with the SharePoint home site, you may choose to continue with the same experience or switch to the new home experience.


## References

- [Customize and edit the Viva Connections home experience](https://learn.microsoft.com/en-us/viva/connections/edit-viva-home?WT.mc_id=M365-MVP-5003693)
- [Set-SPOHomeSite](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/set-spohomesite?WT.mc_id=M365-MVP-5003693)
