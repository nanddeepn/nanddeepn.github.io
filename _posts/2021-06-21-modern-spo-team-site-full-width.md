---
title: "Make Modern Team Site to Full Width"
date: "2021-06-21"
share: true
header:
  image: media/2021-06-21-modern-spo-team-site-full-width/01.png
  teaser: media/2021-06-21-modern-spo-team-site-full-width/01.png
categories:
  - SharePoint
tags:
  - "2021"
  - June 2021
last_modified_at: 2021-06-21T00:00:00-00:00
---

## Overview

Modern SharePoint Team site (Group connected as well non-Group created) by default shows a left navigation with pre-configured links, as well we can add our own customized links.

In this article, we will explore how we can turn off the left navigation and make the Team site to work in full width.

## Left Navigation

The left navigation helps to consolidate the important links at one place throughout the site navigation, but also takes significant amount of space on the screen.

![](/media/2021-06-21-modern-spo-team-site-full-width/01.png)

## Make the Team site as full width

Follow the below steps to turn off the left navigation and make use of Team site as full width:

1. Navigate to **site settings**.
2. Under **Look and Feel**, click **Navigation Elements**.
3. Deselect **Enable Quick Launch**.

    ![](/media/2021-06-21-modern-spo-team-site-full-width/01.png)

4. Click **OK**.

## PowerShell

If you need to carry out this on multiple SharePoint Team sites, as well you want to make this functionality available as a step of your site provisioning, this can be achieved with PowerShell.

**PnP PowerShell**

```powershell
# Connect to SharePoint
Connect-PnPOnline -Url $siteURL

# Disable the Quick Launch
$web = Get-PnPWeb
$web.QuickLaunchEnabled = $false
$web.Update()
```

## The End Result

Once the quick launch is disabled, the Team site will be turned in to full width site experience.

![](/media/2021-06-21-modern-spo-team-site-full-width/01.png)

## Summary

Modern SharePoint Team site (Group connected as well non-Group created) by default shows a left navigation. It takes significant amount of space on the screen. With simple configuration of quick launch, it can be tuned off to make the Team site available in full width for use.
