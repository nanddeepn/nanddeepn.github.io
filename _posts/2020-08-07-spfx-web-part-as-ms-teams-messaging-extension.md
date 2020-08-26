---
title: "SPFx web part as MS Teams messaging extension"
date: "2020-08-07"
share: true
header:
  image: media/2020-08-07-spfx-web-part-as-ms-teams-messaging-extension/04.gif
  teaser: media/2020-08-07-spfx-web-part-as-ms-teams-messaging-extension/04.gif
categories:
  - MS Teams
  - SharePoint
  - SharePoint Framework
tags:
  - "2020"
  - August 2020
last_modified_at: 2020-08-07T00:00:00-00:00
---

## Overview

SharePoint Framework v1.11 supports to host our SPFx web parts as MS Teams messaging extension. This offers more power to the developers so that they can now extend SPFx web parts to MS Teams as messaging extension.
 In this article, we will develop the SPFx web part &quot;Planet Explorer&quot;, which helps us to explore the planet information and later add it as MS Teams messaging extension.


## What do we need?

We will need to set up below things:

1. Bot
2. SPFx web part


## Implement Bot

Please follow my previous article - [Implementing MS Teams Bot with Yo Teams to set up a Bot](/posts/2020-08-04-implementing-ms-teams-bot-with-yoteams/). We will extend the same Bot and add some intelligence to it which will return the planet information by its name.

Add below JSON files to Bot:

1. planets.json: Information about planets.
2. planetDisplayCard.json: Adaptive display card to show information about a specific planet.

I have reused the above JSON files from the MS Learn module ([https://docs.microsoft.com/en-us/learn/modules/msteams-messaging-extensions/](https://docs.microsoft.com/en-us/learn/modules/msteams-messaging-extensions/)).

After receiving the notification, the bot can process the retrieved data and post some information to the conversation. For this, we will implement handleTeamsMessagingExtensionSubmitAction method.

```typescript
protected async handleTeamsMessagingExtensionSubmitAction(context: TurnContext, action: MessagingExtensionAction): Promise<MessagingExtensionActionResponse> {
    const userData: string = action.data;

    // load planets
    const planets: any = require("./planets.json");

    // get the selected planet
    const selectedPlanet: any = planets.filter((planet) => planet.name === userData)[0];
    const adaptiveCard = this.getPlanetDetailCard(selectedPlanet);

    await context.sendActivity({ attachments: [adaptiveCard] });
    return Promise.resolve({});
}
```

The getPlanetDetailCard method will return the adaptive card to the conversation.

```typescript
private getPlanetDetailCard(selectedPlanet: any): MessagingExtensionAttachment {
    // load display card
    const adaptiveCardSource: any = require("./planetDisplayCard.json");

    // update planet fields in display card
    adaptiveCardSource.actions[0].url = selectedPlanet.wikiLink;
    find(adaptiveCardSource.body, { "id": "cardHeader" }).items[0].text = selectedPlanet.name;
    const cardBody: any = find(adaptiveCardSource.body, { "id": "cardBody" });
    find(cardBody.items, { "id": "planetSummary" }).text = selectedPlanet.summary;
    find(cardBody.items, { "id": "imageAttribution" }).text = "*Image attribution: " + selectedPlanet.imageAlt + "*";
    const cardDetails: any = find(cardBody.items, { "id": "planetDetails" });
    cardDetails.columns[0].items[0].url = selectedPlanet.imageLink;
    find(cardDetails.columns[1].items[0].facts, { "id": "orderFromSun" }).value = selectedPlanet.id;
    find(cardDetails.columns[1].items[0].facts, { "id": "planetNumSatellites" }).value = selectedPlanet.numSatellites;
    find(cardDetails.columns[1].items[0].facts, { "id": "solarOrbitYears" }).value = selectedPlanet.solarOrbitYears;
    find(cardDetails.columns[1].items[0].facts, { "id": "solarOrbitAvgDistanceKm" }).value = Number(selectedPlanet.solarOrbitAvgDistanceKm).toLocaleString();

    // return the adaptive card
    return CardFactory.adaptiveCard(adaptiveCardSource);
}
```

Once done, publish the Bot again to MS Azure Web App.


## Implement SPFx Web Part

Now, we will develop the SPFx web part as shown below.

![](/media/2020-08-07-spfx-web-part-as-ms-teams-messaging-extension/01.png)

We don't need to use a specific host in the supportedHosts property (like TeamsTab or TeamsPersonalApp) inside web parts's manifest.json. However, we just need to extend the teams manifest in your SharePoint Framework solution with **composeExtension**.

In the **teams** folder, create new file **manifest.json**

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/teams/v1.5/MicrosoftTeams.schema.json",
  "manifestVersion": "1.5",
  "packageName": "Planet explorer",
  "id": "e3cfb794-9ef7-4562-918b-347e96580baf",
  "version": "0.1",
  "developer": {
    "name": "Nanddeep Nachan",
    "websiteUrl": "https://aka.ms/sppnp",
    "privacyUrl": "https://privacy.microsoft.com/privacystatement",
    "termsOfUseUrl": "https://www.microsoft.com/servicesagreement"
  },
  "name": {
    "short": "Planet explorer"
  },
  "description": {
    "short": "SPFx Teams Planet explorer",
    "full": "SPFx Teams Planet explorer"
  },
  "icons": {
    "outline": "e3cfb794-9ef7-4562-918b-347e96580baf_outline.png",
    "color": "e3cfb794-9ef7-4562-918b-347e96580baf_color.png"
  },
  "accentColor": "#004578",
  "staticTabs": [
    {
      "entityId": "com.contoso.personaltab.spfx",
      "name": "My SPFx Personal Tab",
      "contentUrl": "https://{teamSiteDomain}/_layouts/15/TeamsLogon.aspx?SPFX=true&dest=/_layouts/15/teamshostedapp.aspx%3Fteams%26personal%26componentId=e3cfb794-9ef7-4562-918b-347e96580baf%26forceLocale={locale}",
      "scopes": [
        "personal"
      ]
    }
  ],
  "configurableTabs": [
    {
      "configurationUrl": "https://{teamSiteDomain}{teamSitePath}/_layouts/15/TeamsLogon.aspx?SPFX=true&dest={teamSitePath}/_layouts/15/teamshostedapp.aspx%3FopenPropertyPane=true%26teams%26componentId=e3cfb794-9ef7-4562-918b-347e96580baf%26forceLocale={locale}",
      "canUpdateConfiguration": true,
      "scopes": [
        "team"
      ]
    }
  ],
  "bots": [
    {
      "botId": "c3b26a3e-6623-4782-856f-4a1d83819f0c",
      "needsChannelSelector": false,
      "isNotificationOnly": false,
      "scopes": [
        "team",
        "personal"
      ]
    }
  ],
  "composeExtensions": [
    {
      "botId": "c3b26a3e-6623-4782-856f-4a1d83819f0c",
      "canUpdateConfiguration": true,
      "commands": [
        {
          "id": "planetExplorer",
          "type": "action",
          "title": "Explore a planet",
          "description": "Find and explore a planet",
          "initialRun": false,
          "fetchTask": false,
          "context": [
            "commandBox",
            "compose"
          ],
          "taskInfo": {
            "title": "Explore a planet",
            "width": "1100",
            "height": "665",
            "url": "https://{teamSiteDomain}/_layouts/15/TeamsLogon.aspx?SPFX=true&dest=/_layouts/15/teamstaskhostedapp.aspx%3Fteams%26personal%26componentId=e3cfb794-9ef7-4562-918b-347e96580baf%26forceLocale={locale}"
          }
        }
      ]
    }
  ],
  "validDomains": [
    "*.login.microsoftonline.com",
    "*.sharepoint.com",
    "*.sharepoint-df.com",
    "spoppe-a.akamaihd.net",
    "spoprod-a.akamaihd.net",
    "resourceseng.blob.core.windows.net",
    "msft.spoppe.com"
  ],
  "webApplicationInfo": {
    "resource": "https://{teamSiteDomain}",
    "id": "00000003-0000-0ff1-ce00-000000000000"
  }
}
```

In the composeExtensions, the URL of taskInfo property must match the URL should have the componentId query string parameter set to the ID of the SharePoint Framework web part that should be exposed in the messaging extension.


**Respond to user interaction**

SPFx web part when exposed as a messaging extension should respond to user interaction by posting an adaptive card to the conversation. This requires using a task module and a bot. The task module notifies the bot of the event that the user triggered, and the bot will post data back to the conversation.

```typescript
private _planetClicked(planet: string): void {
    const host: string = this.props.host._teamsManager._appContext.applicationName;
    if (host !== 'TeamsTaskModuleApplication') {
      return;
    }

    this.setState({ submitCardDialogVisible: true });
    this.props.teamsContext.tasks.submitTask(planet);
}
```

After receiving the notification, the bot will process the retrieved data (i.e. planet name) and post the planet information to the conversation as an adaptive card.


## Deploy SPFx web part as messaging extension

Follow below steps to deploy SPFx web part as messaging extension:

1. On the command prompt, run below commands to prepare the package.

    ```
    gulp bundle --ship
    gulp package-solution --ship
    ```

2. Deploy the sppkg to the SharePoint tenant app catalog.
3. Navigate to teams folder in SPFx solution and zip the content (2 png files, and manifest.json). Don't zip the folder.
4. Open **MS Teams**.
5. Click **Apps**.
6. Click **Upload a custom app** > **Upload for \<tenant\>**.

    ![](/media/2020-08-07-spfx-web-part-as-ms-teams-messaging-extension/02.png)

7. Select **Planet explorer** , click **Add**.

    ![](/media/2020-08-07-spfx-web-part-as-ms-teams-messaging-extension/03.png)

8. Click the **Planet explorer** messaging extension.


## Summary

SharePoint Framework v1.11 supports to host our SPFx web parts as MS Teams messaging extension. We don't need to use a specific host in the supportedHosts property (like TeamsTab or TeamsPersonalApp) inside web parts's manifest.json. However, we just need to extend the teams manifest in your SharePoint Framework solution with composeExtension.
