---
title: "Host SharePoint App in MS Teams"
date: "2020-06-01"
---

## Overview

This is a recent update/announcement from **Microsoft Build 2020**. We can create a standalone and static app inside MS Teams that links to the SharePoint intranet site.

In this article, we will explore the option of hosting the SharePoint site as a static app inside MS Teams, which will offer convenience to MS Teams users to access the updates.

## Pre-requisites

Before we begin, we will need below parameters handy:

Consider using the SharePoint modern site. (E.g. [https://contoso.sharepoint.com/sites/SPDemo](https://contoso.sharepoint.com/sites/SPDemo))

**Placeholder**

**Example**

{{siteUrl}}

[https://contoso.sharepoint.com/sites/SPDemo](https://contoso.sharepoint.com/sites/SPDemo)

{{sitePath}}

/sites/SPDemo

{{subdomain}}

contoso

## Set up App Studio to create SharePoint App

Follow these steps to download App Studio from the App Store, if you do not have it.

1. Open MS Teams desktop client.
2. Click **Apps** from the left menu.
3. Search and click “**App Studio**”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image.png)

1. Click **Add**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-1.png)

## App Studio to create a SharePoint App

We will create a new app using App Studio to host SharePoint App.

1. Open **App Studio**.
2. Click **Manifest editor**.
3. Click **Create a new app**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-2.png)

1. Fill in the **App details**.
2. Specify **App names** and **Descriptions**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-3.png)

1. Specify **Developer information** and **App URLs**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-4.png)

1. Under **Capabilities**, click **Tabs**.
2. Under the **Personal tab**, click **Add**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-5.png)

1. Specify the parameters. Follow below format:

- contentUrl: {{siteUrl}}/\_layouts/15/teamslogon.aspx?SPFX=true&dest={{sitePath}}
- websiteUrl: {{siteUrl}}

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-6.png)

1. Click **Save**.
2. Under **Finish**, click **Domains and permissions**.
3. Add your SharePoint online domain name.
4. Click **Add**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-7.png)

1. Add AAD App ID to set up a single sign-on (SSO). Specify AAD application ID as **00000003-0000-0ff1-ce00-000000000000** and Resource Url as **{{subdomain}}.sharepoint.com**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-8.png)

1. Under **Finish**, click **Test and distribute**. Make sure no errors are displayed.
2. Click **Install**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-9.png)

## Test Personal App

In the Teams desktop client, navigate to the ellipses (…) on the left-hand side of your app bar. Find your new app and load it.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-10.png)

## Troubleshooting

While installing the app, you may get an error: You don't have permission to add Intranet to this team.

**Solution:**

Enable the sideloading of the app by following the below steps.

1. Open Teams admin center.
2. Under **Teams app**, click **Setup policies**.
3. Click **Global (Org-wide default)**.
4. On the **Upload custom apps**.
5. Click **Save**.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/06/word-image-11.png)

## Summary

We can create a standalone and static app inside MS Teams that links to the SharePoint intranet site. Please note, this functionality is only available with SharePoint modern sites and page. Classic SharePoint sites and pages are not supported for this.

# References

- Create a SharePoint intranet in Teams ([aka.ms/sharepoint-app-in-teams](http://aka.ms/sharepoint-app-in-teams))
- Admin settings for apps in Microsoft Teams ([aka.ms/ManageAppsInTeams](http://aka.ms/ManageAppsInTeams))
