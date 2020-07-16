---
title: "Host SharePoint App in MS Teams"
date: "2020-06-01"
share: true
categories:
  - MS Teams
  - SharePoint
tags:
  - "2020"
  - June 2020
last_modified_at: 2020-06-01T00:00:00-00:00
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

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/01.png)

4. Click **Add**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/02.png)

## App Studio to create a SharePoint App

We will create a new app using App Studio to host SharePoint App.

1. Open **App Studio**.
2. Click **Manifest editor**.
3. Click **Create a new app**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/03.png)

4. Fill in the **App details**.
5. Specify **App names** and **Descriptions**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/04.png)

6. Specify **Developer information** and **App URLs**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/05.png)

7. Under **Capabilities**, click **Tabs**.
8. Under the **Personal tab**, click **Add**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/06.png)

9. Specify the parameters. Follow below format:

- contentUrl: {{siteUrl}}/\_layouts/15/teamslogon.aspx?SPFX=true&dest={{sitePath}}
- websiteUrl: {{siteUrl}}

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/07.png)

10. Click **Save**.
11. Under **Finish**, click **Domains and permissions**.
12. Add your SharePoint online domain name.
13. Click **Add**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/08.png)

14. Add AAD App ID to set up a single sign-on (SSO). Specify AAD application ID as **00000003-0000-0ff1-ce00-000000000000** and Resource Url as **{{subdomain}}.sharepoint.com**

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/09.png)

15. Under **Finish**, click **Test and distribute**. Make sure no errors are displayed.
16. Click **Install**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/10.png)

## Test Personal App

In the Teams desktop client, navigate to the ellipses (…) on the left-hand side of your app bar. Find your new app and load it.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/11.png)

## Troubleshooting

While installing the app, you may get an error: You don't have permission to add Intranet to this team.

**Solution:**

Enable the sideloading of the app by following the below steps.

1. Open Teams admin center.
2. Under **Teams app**, click **Setup policies**.
3. Click **Global (Org-wide default)**.
4. On the **Upload custom apps**.
5. Click **Save**.

![](/media/2020-06-01-host-sharepoint-app-in-ms-teams/12.png)

## Summary

We can create a standalone and static app inside MS Teams that links to the SharePoint intranet site. Please note, this functionality is only available with SharePoint modern sites and page. Classic SharePoint sites and pages are not supported for this.

# References

- Create a SharePoint intranet in Teams ([aka.ms/sharepoint-app-in-teams](http://aka.ms/sharepoint-app-in-teams))
- Admin settings for apps in Microsoft Teams ([aka.ms/ManageAppsInTeams](http://aka.ms/ManageAppsInTeams))

[![HitCount](http://hits.dwyl.com/nanddeepn/nanddeepngithubio/posts/2020-06-01-host-sharepoint-app-in-ms-teams/.svg)](http://hits.dwyl.com/nanddeepn/nanddeepngithubio/posts/2020-06-01-host-sharepoint-app-in-ms-teams/)
