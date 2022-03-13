---
title: "Host Power BI Report as MS Teams App"
date: "2022-03-13"
share: true
header:
  image: media/2022-03-13-host-power-bi-report-teams-app/03.png
  teaser: media/2022-03-13-host-power-bi-report-teams-app/03.png
categories:
  - MS Teams
  - Power BI
tags:
  - "2022"
  - March 2022
last_modified_at: 2022-03-13T00:00:00-00:00
---
## Overview

The power BI app in Microsoft Teams is useful to show the Power BI reports in Microsoft Teams. The Power BI app can be added to the Microsoft Teams team as a tab to show an individual report or as a personal app to show the full Power BI experience.

In this article, we will explore to show an individual Power BI report as an app in Microsoft Teams as a personal app.

## Power BI App in Microsoft Teams

Power BI app is available in Microsoft Teams as an app. Follow the below steps to add the Power BI app.

1. In Microsoft Teams, click **Apps**.
2. Search and open the **Power BI** app.
3. The Power BI can be added as a personal app, as a tab to the team channel, or a chat.

![](/media/2022-03-13-host-power-bi-report-teams-app/01.png)

## Power BI Report as Teams app

A variety of apps for every need are available in Microsoft Teams. Power BI is an app available from Microsoft to show Power BI reports in Microsoft Teams.

When we have a Power BI report which shows specific information of a logged-in user, it is difficult to make it available as an app to the entire organization.

**Limitations with Power BI app as a tab**

The Power BI app when added to the Microsoft Teams channel as a tab, can configure it to show a specific report. However, we cannot have entire organization users added to a single Microsoft Teams team.

**Limitations with Power BI app as a personal app**

The other option to make an app available to all organization users is to push the app as a personal app for everyone. However, we cannot configure the Power BI app as a personal app showing a specific report.

The Power BI app when added as a personal app, provides a generic interface of Power BI showing apps and workspaces.

![](/media/2022-03-13-host-power-bi-report-teams-app/02.png)

## Microsoft Teams Manifest

We can create custom apps for Microsoft Teams by creating an app manifest as defined [here](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema). In the App manifest, we can define the functionality for tabs, bots, connectors, messaging extensions, etc.

**Manifest for hosting Power BI report**

The staticTabs element in manifest helps to define tabs that can be added as a personal app. The contentUrl property points to the URL of a website to be hosted in the tab.

We will take benefit of this configuration to show the Power BI report in the personal app of Teams.

The manifest looks as follows:

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/teams/v1.12/MicrosoftTeams.schema.json",
    "manifestVersion": "1.12",
    "version": "1.0.0",
    "id": "a131b5de-5f5f-433a-912b-2d92e7d502cc",
    "packageName": "com.example.myapp",
    "developer": {
        "name": "Publisher Name",
        "websiteUrl": "https://website.com/",
        "privacyUrl": "https://website.com/privacy",
        "termsOfUseUrl": "https://website.com/app-tos",
        "mpnId": "1234567890"
    },
    "name": {
        "short": "Power BI App",
        "full": "Power BI App in MS Teams"
    },
    "description": {
        "short": "Power BI as Teams app",
        "full": "Display Power BI as MS Teams app"
    },
    "icons": {
        "outline": "outline.png",
        "color": "color.png"
    },
    "accentColor": "#009B00",    
    "staticTabs": [
        {
            "entityId": "cb3184c1-5fde-4864-9d89-6869f9df9d84",
            "scopes": [
                "personal"
            ],
            "context": [
                "personalTab",
                "channelTab"
            ],
            "name": "Power BI",
            "contentUrl": "https://app.powerbi.com/links/SuxhkZGSJg?ctid=de348bc7-1aeb-4406-8cb3-97db021cadb4&pbi_source=linkShare",
            "websiteUrl": "https://app.powerbi.com/groups/c86231ea-6933-444b-82b9-ca31eac969f2/reports/cb3184c1-5fde-4864-9d89-6869f9df9d84/ReportSectionc50b2593fb84b97af13a"
        }
    ],
    "validDomains": [
        "*.powerbi.com"
    ],
	"webApplicationInfo":{
		"id": "00000009-0000-0000-c000-000000000000",
		"resource": "https://app.powerbi.com"
	}
}
```

## Deployment

The custom app hosting in Microsoft Teams needs 3 elements:

1. manifest.xml: contains the definition of app
2. Transparent outline icon (32 x 32 pixels)
3. Color icon (192 x 192 pixels)

To publish your app, zip the above 3 files together and upload it to Microsoft Teams.

The app when added as a personal app, shows Power BI report as follows:

![](/media/2022-03-13-host-power-bi-report-teams-app/03.png)

## Challenge with SSO

In the manifest, if we can specify the Public URL of the Power BI report as contentUrl the app in Microsoft Teams works fine.

However, to enable a seamless experience for users, we need to configure the `webApplicationInfo` element from the manifest.

```json
"webApplicationInfo":{
    "id": "AAD App ID",
    "resource": "Resource URL for acquiring auth token for SSO"
}
```

The `id` specifies Azure Active Directory App ID (GUID) and the `resource` specifies the resource URL of an app for acquiring auth token for SSO.

I was taking a reference of how Power Apps app are being hosted as an MS Teams App. The `manifest.json` for Power Apps has an entry as follows:

```json
"webApplicationInfo":{
    "id": "9362bc14-3e81-4ef9-8b77-f1c40afe68e0",
    "resource": "https://apps.powerapps.com"
}
```

The next steps to identify to make SSO works for Power BI in MS Teams is to identify answers to the below questions:

- Where does this id (9362bc14-3e81-4ef9-8b77-f1c40afe68e0) come from?
- How to get a similar id for Power BI?

## Summary

Power BI app is available in Microsoft Teams as an app. When we have a Power BI report which shows specific information of a logged-in user, it is difficult to make it available as an app to the entire organization. I am still in the process to identify to make SSO works for Power BI in MS Teams.

## References
- [Manifest schema for Microsoft Teams](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema?WT.mc_id=M365-MVP-5003693)
