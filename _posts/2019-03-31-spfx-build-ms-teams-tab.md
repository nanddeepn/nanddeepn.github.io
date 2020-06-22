---
title: "SharePoint Framework - Build Microsoft Teams Tab"
date: "2019-03-31"
---

## Overview

The SPFx release of version 1.8 eases the implementation of Microsoft Teams tabs using SharePoint Framework. This gives possibility to various scenarios in developing Microsoft Teams tabs using SharePoint Framework.

In this article, we will explore building and deploying the Microsoft Teams tabs.

## Create SPFx WebPart

1. Open command prompt. Create a directory for SPFx solution.

md spfx-teams-tab

1. Navigate to above created directory.

cd spfx-teams-tab

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-323.png)

- **Solution Name:** Hit enter to have default name (spfx-teams-tab in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 or 2019 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Use the current folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: Y
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension.
    - Selected choice: WebPart
- **Web Part Name:** Hit enter to select the default name or type in any other name.
    - Selected choice: TeamsTab
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: First Teams Tab
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

Teams in SPFx Solution

The SPFx solution contains additional Teams folder in the solution structure with default configurations. This helps to get started with Teams development.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-324.png)

\[webpartid\]\_color.png: Default small picture for tab

\[webpartid\]\_outline.png: Default large picture for tab

**Add support for Microsoft Teams**

1. Open web part manifest.json file.
2. To the “supportedHosts” property, add “TeamsTab”.

{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",  
  "id": "0172ff63-158d-44b5-aa23-99e72a812c02",  
  "alias": "TeamsTabWebPart",  
  "componentType": "WebPart",  
  
  // The "\*" signifies that the version should be taken from the package.json  
  "version": "\*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false,  
  "supportedHosts": \["SharePointWebPart", "TeamsTab"\],  
  
  "preconfiguredEntries": \[{  
    "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other  
    "group": { "default": "Other" },  
    "title": { "default": "TeamsTab" },  
    "description": { "default": "First teams tab" },  
    "officeFabricIconFontName": "Page",  
    "properties": {  
      "description": "TeamsTab"  
    }  
  }\]  
}

Code for Microsoft Teams Context

1. Open web part file (src\\webparts\\teamsTab\\TeamsTabWebPart.ts).
2. Add below import.

import \* as microsoftTeams from '@microsoft/teams-js';

1. In a class, define a variable to store Microsoft Teams context.

private \_teamsContext: microsoftTeams.Context;

1. Add onInit() method to set the Microsoft Teams context.

protected onInit(): Promise<any> {  
  let retVal: Promise<any> = Promise.resolve();  
  if (this.context.microsoftTeams) {  
    retVal = new Promise((resolve, reject) => {  
      this.context.microsoftTeams.getContext(context => {  
        this.\_teamsContext = context;  
        resolve();  
      });  
    });  
  }  
  return retVal;  
}

1. Update render() method.

public render(): void {  
  let title: string = '';  
  let siteTabTitle: string = '';  
  
  if (this.\_teamsContext) {  
    // We have teams context for the web part  
    title = "Welcome to Teams!";  
    siteTabTitle = "Team: " + this.\_teamsContext.teamName;  
  }  
  else  
  {  
    // We are rendered in normal SharePoint context  
    title = "Welcome to SharePoint!";  
    siteTabTitle = "SharePoint site: " + this.context.pageContext.web.title;  
  }  
  
  this.domElement.innerHTML = \`  
    <div class="${ styles.teamsTab }">  
      <div class="${ styles.container }">  
        <div class="${ styles.row }">  
          <div class="${ styles.column }">  
            <span class="${ styles.title }">${title}</span>  
            <p class="${ styles.description }">${siteTabTitle}</p>  
            <p class="${ styles.description }">Description property value - ${escape(this.properties.description)}</p>  
            <a href="https://aka.ms/spfx" class="${ styles.button }">  
              <span class="${ styles.label }">Learn more</span>  
            </a>  
          </div>  
        </div>  
      </div>  
    </div>\`;  
}

Package and Deploy web part to SharePoint

1. On the command prompt, run below command to bundle the solution.

gulp bundle --ship

1. Run below command to package the solution.

gulp package-solution --ship

This will create spfx-teams-tab.sppkg package under sharepoint/solution folder.

1. Open SharePoint App catalog site.
2. Upload spfx-teams-tab.sppkg to the app catalog.
3. Check “Make this solution available to all sites in the organization” option.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-325.png)

1. Click Deploy.

## Test the WebPart in SharePoint

1. Open SharePoint site
2. Add the webpart to page.
3. The web part shows SharePoint site context information.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-326.png)

## Make web part available in Microsoft Teams

To make the web part available in Microsoft Teams, we will have to synchronize the solution with teams.

1. Select the solution in app catalog.
2. From ribbon, click Files.
3. Select Sync to Teams.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-327.png)

## Test web part in Microsoft Teams

1. Open Microsoft Teams.
2. Under existing team, select channel (by default, General channel is available under team).
3. Click + to add new tab.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-328.png)

1. Select our custom tab from the list.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-329.png)

1. Click Install.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-330.png)

1. Click Save.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-331.png)

1. Our web part will appear as custom tab in Microsoft Teams.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-332.png)

## Summary

SharePoint Framework version 1.8 eases the development of Microsoft Teams tabs using SharePoint Framework. The same web part solution can work on both SharePoint and Microsoft Teams based on the context under which it is running.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-build-microsoft-teams-tab/).
