---
title: "Get App Details from MS Teams Catalog"
date: "2022-03-30"
share: true
header:
  image: media/2022-03-30-app-details-ms-teams-catalog/03.png
  teaser: media/2022-03-30-app-details-ms-teams-catalog/03.png
categories:
  - MS Teams
tags:
  - "2022"
  - March 2022
last_modified_at: 2022-03-30T00:00:00-00:00
---
## Overview

As a Microsoft Teams administrator, you can view and manage apps available in the app catalog for Microsoft Teams from the Microsoft Teams Admin Center (TAC). It is a huge list to scroll through including first-party apps available from Microsoft, third-party apps available from service partners, and apps developed by organization developers. However, it lacks the reporting and download functionality.

In this article, we will explore extracting the information about Microsoft Teams apps efficiently.

## Graph APIs and its limitations

We have a nice Graph API available to list apps from the Microsoft Teams app catalog as follows:

```
GET /appCatalogs/teamsApps
```

With a minimum delegated or application permission of `AppCatalog.Read.All`, we can read the apps from the app catalog.

![](/media/2022-03-30-app-details-ms-teams-catalog/01.png)

The API works fine to get the basic details of apps available in an app catalog.

Let us get bit further in this and get more information about an individual app with call to below API:

```
https://graph.microsoft.com/v1.0/appCatalogs/teamsApps?$filter=id eq '05ab3377-bd38-41dc-b917-b05449c13c78'&$expand=appDefinitions&$expand=appDefinitions($expand=bot)
```

![](/media/2022-03-30-app-details-ms-teams-catalog/02.png)

With two Graph API calls, we get more information about the app along with its definition.

**Limitations**

There are a couple of limitations, I observed working with this Graph API.

1. We have to make multiple Graph API calls to get our work done, as follows:
    1. Get a list of all the apps
    2. For each of the apps with its id, get more information
2. The Graph APIs does not provide information on the Publisher Name

The publisher's name is important to understand how many apps are available from an individual publisher and their status. Organizations have their approved list of vendors or approvers, and it is important to understand the availability of apps from the approved vendors.

## Workaround

There is an internal, undocumented API available to get this information. Follow the below steps to get the app details:

1. Browse to the **Manage Apps** section in [Microsoft Teams Admin Center](https://admin.teams.microsoft.com/policies/manage-apps).
2. In the browser, open Developer Dashboard (F12).
3. Navigate to the Network tab to observe the call happening in the background.
4. There is an interesting API being called [https://teams.microsoft.com/api/mt/apac/beta/admin/tenantStagedApps](https://teams.microsoft.com/api/mt/apac/beta/admin/tenantStagedApps), which gives app details in one call.

![](/media/2022-03-30-app-details-ms-teams-catalog/03.png)

As an authentication, we need to pass the Bearer token.

## Limitation

The Bearer token available from the browser session can be used further. If you generate the one with PowerShell by combining the Graph APIs, it might not work to fetch the details.

## Summary

Graph APIs have limitations to fetch the app information. The workaround is to use the internal API. However, avoid using the internal API in the production-ready code.

I am still looking into options to automate this PowerShell script as an end-to-end solution. Appreciate your thoughts on the same.

## Resources

- [teamsApp resource type](https://docs.microsoft.com/en-us/graph/api/resources/teamsapp?WT.mc_id=M365-MVP-5003693)
- [List teamsApp](https://docs.microsoft.com/en-us/graph/api/appcatalogs-list-teamsapps?WT.mc_id=M365-MVP-5003693)