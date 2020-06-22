---
title: "Host Bot in Azure for MS Teams"
date: "2019-09-21"
---

## Overview

During previous article - [Develop Bot For MS Teams With Bot Builder V4 SDK](https://nanddeepnachanblogs.com/2019/09/develop-bot-for-ms-teams-with-bot-builder-v4-sdk/), we developed an echo bot with Bot Builder v4 utilizing ngrok to develop and test locally. ngrok is used during the development phase. It allows us to build and test the bot without publishing the app on MS Azure (or other hosting environments).

In this article, we will publish our app (Bot) to MS Azure and channelize to MS Teams.

### Publish Bot to MS Azure

Follow below steps to publish the Bot application to MS Azure App Service.

1. Open the Bot application (i.e. office-content-management) in Visual Studio.
2. Open “\\office-content-management\\Manifest\\manifest.json”
3. Update the package name as a generic name representing Bot.
4. Update websiteUrl, privacyUrl, and termsOfUseUrl as an organization url.

{  
  "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.3/MicrosoftTeams.schema.json",  
  "manifestVersion": "1.3",  
  "version": "1.0.0",  
  "id": "619674bb-c724-4fa0-b42d-22787d35f569",  
  "packageName": "com.contoso.contentmanagement",  
  "developer": {  
    "name": "Office Developer",  
    "websiteUrl": "https://www.microsoft.com",  
    "privacyUrl": "https://www.microsoft.com/privacy",  
    "termsOfUseUrl": "https://www.microsoft.com/termsofuse"  
  },  
  "icons": {  
    "color": "bot-icon-color-192x192.png",  
    "outline": "bot-icon-outline-32x32.png"  
  },  
  "name": {  
    "short": "office-content-management-bot ",  
    "full": "Office Content Management Bot"  
  },  
  "description": {  
    "short": "Office Content Management",  
    "full": "Sample bot for the Office Content Management"  
  },  
  "accentColor": "#AB00B0",  
  "bots": \[  
    {  
      "botId": "619674bb-c724-4fa0-b42d-22787d35f569",  
      "supportsFiles": true,  
      "scopes": \[  
        "personal",  
        "team"  
      \]  
    }  
  \],  
  "composeExtensions": \[\],  
  "permissions": \[  
    "identity",  
    "messageTeamMembers"  
  \],  
  "validDomains": \[\]  
}

1. Click Build menu, select “Publish office-content-management”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-505.png)

1. Pick a publish target as “App Service”.
2. Select to create new App Service.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-506.png)

1. Click “Advanced…”
2. Set Configuration to Release.
3. Under “File Publish Options”, select option “Remove additional files at destination” to remove outdated files during deployment.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-507.png)

1. Click Save to close the Advanced dialog.
2. Click Publish on the main screen.
3. In the “Create App Service” dialog, specify the needed information.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-508.png)

1. Click Create.
2. The Bot application will be deployed to MS Azure App Service.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-509.png)

1. Post successful deployment, the app service url will open in a browser. (e.g. [https://office-content-management20190918031143.azurewebsites.net/](https://office-content-management20190918031143.azurewebsites.net/))

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-510.png)

## Update Manifest

Follow below steps to update the manifest with App service details.

1. In the visual studio, open “\\office-content-management\\Manifest\\manifest.json”.
2. Set websiteUrl, privacyUrl, and termsOfUseUrl to the app service url.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-511.png)

1. Build the solution. The AfterBuild target will run and compressed .zip file will be available under the bin folder.
2. Upload app into Microsoft Teams. (Follow instruction from previous article [https://www.c-sharpcorner.com/article/develop-bot-for-ms-teams-with-bot-builder-v4-sdk/](https://www.c-sharpcorner.com/article/develop-bot-for-ms-teams-with-bot-builder-v4-sdk/), section Upload app into Microsoft Teams).

## Update Bot Channels Registration

From Azure portal, open the Bot Channels Registration, under settings, set the messaging endpoint as app service url.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-512.png)

## Test the Bot in MS Teams

1. Open MS Teams.
2. From the left menu, click Chat.
3. Our Bot (named OfficeContentManagement) should be listed out.
4. Send the messages to Bot, it should echo back.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-513.png)

## Summary

ngrok is used during the development phase. Once the Bot is ready it can be deployed hosted in Azure for public use. Visual Studio offers an easy option to deploy the app in Azure.

This content was originally posted [here](https://www.c-sharpcorner.com/article/host-ms-teams-bot-in-azure/).
