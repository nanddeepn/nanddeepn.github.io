---
title: "Build MS Teams Personal Apps in SPFx"
date: "2020-01-14"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - MS Teams
header:
  image: media/2020-01-14-spfx-build-ms-teams-personal-app/08.png
  teaser: media/2020-01-14-spfx-build-ms-teams-personal-app/08.png
tags:
  - "2020"
  - January 2020
last_modified_at: 2020-01-14T00:00:00-00:00
---

## Overview

SharePoint Framework v1.8 supported hosting SPFx web parts as a tab inside MS Teams. SPFx v1.10 now supports creating solutions in Microsoft Teams for building the personal app. Personal apps offer a view of your key application data from across teams and can either be acquired individually or deployed across Teams.

In this article, we will get an overview of the personal app and how to build it with SPFx.


## Upgrade to SPFx v1.10

On the command prompt, execute the below command to list out the outdated npm packages.

```
npm outdated --global
```

![](/media/2020-01-14-spfx-build-ms-teams-personal-app/01.png)

Upgrade to SPFx v1.10 by running below command:

```
npm update -g @microsoft/generator-sharepoint
```


## Overview of Personal Apps in MS Teams

A personal app is an app with a personal scope. As a developer, you can develop a version of your app that is built for the individual user. It can have a collection of tabs and a bot. A personal app is available from the personal app flyout.

![](/media/2020-01-14-spfx-build-ms-teams-personal-app/02.png)


## Develop SPFx solution for MS Teams Personal App

Follow the below steps to develop the SPFx solution.

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-ms-teams-personal-app
    ```

2. Navigate to the directory.

    ```
    cd spfx-ms-teams-personal-app
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2020-01-14-spfx-build-ms-teams-personal-app/03.png)

    When prompted:

    - Accept the default **spfx-ms-teams-personal-app** as your solution name, and then select Enter.
    - Select **SharePoint Online only (latest)**, and then select Enter.
    - Select **Use the current folder** as the location for the files.
    - Select **y** to ensure that your web part is automatically deployed tenant-wide when it's added to the tenant App Catalog.
    - Select **N** on the question if solution contains unique permissions.
    - Select **WebPart** as the client-side component type to be created.
    - Enter **MyTeamsPersonalApp** for the web part name, and then select Enter.
    - Enter the description of the web part.
    - Select **No JavaScript framework** to develop the web part.

5. Yeoman generator will perform the scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
6. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```


## Update web part manifest

Update the web part manifest to make the SPFx web part available as a personal app in MS Teams.

1. Open manifest json file for the web part.
2. Modify the **supportedHosts** properties to include "**TeamsPersonalApp**".

    ```
    {  
      "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",  
      "id": "0648ced7-f57d-40f3-b9ed-b4152d877b52",  
      "alias": "MyTeamsPersonalAppWebPart",  
      "componentType": "WebPart",  
      
      // The "*" signifies that the version should be taken from the package.json  
      "version": "*",  
      "manifestVersion": 2,  
      
      // If true, the component can only be installed on sites where Custom Script is allowed.  
      // Components that allow authors to embed arbitrary script code should set this to true.  
      // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
      "requiresCustomScript": false,  
      "supportedHosts": ["SharePointWebPart", "TeamsPersonalApp"],  
      
      "preconfiguredEntries": [{  
        "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other  
        "group": { "default": "Other" },  
        "title": { "default": "MyTeamsPersonalApp" },  
        "description": { "default": "MyTeamsPersonalApp description" },  
        "officeFabricIconFontName": "Page",  
        "properties": {  
          "description": "MyTeamsPersonalApp"  
        }  
      }]  
    }
    ```

3. Open web part file at "src\\webparts\\myTeamsPersonalApp\\MyTeamsPersonalAppWebPart.ts"
4. Update the render method as below:

```typescript
public render(): void {  
  let title: string = '';  
  let subTitle: string = '';  
  let siteTabTitle: string = '';  
  
  if (this.context.sdks.microsoftTeams) {  
    // We have teams context for the web part  
    title = "Welcome to Teams!";  
    subTitle = "Building custom enterprise tabs for your business.";  
    siteTabTitle = "We are in the context of following Team: " + this.context.sdks.microsoftTeams.context.teamName;  
  }  
  else {  
    // We are rendered in normal SharePoint context  
    title = "Welcome to SharePoint!";  
    subTitle = "Customize SharePoint experiences using Web Parts.";  
    siteTabTitle = "We are in the context of following site: " + this.context.pageContext.web.title;  
  }  
  
  this.domElement.innerHTML = `  
  <div class="${ styles.myTeamsPersonalApp}">  
    <div class="${ styles.container}">  
      <div class="${ styles.row}">  
        <div class="${ styles.column}">  
          <span class="${ styles.title}">${title}</span>  
          <p class="${ styles.subTitle}">${subTitle}</p>  
          <p class="${ styles.description}">${siteTabTitle}</p>  
          <p class="${ styles.description}">Description property value - ${escape(this.properties.description)}</p>  
          <a href="https://aka.ms/spfx" class="${ styles.button}">  
            <span class="${ styles.label}">Learn more</span>  
          </a>  
        </div>  
      </div>  
    </div>  
  </div>`;  
}
```


## Package and deploy the web part to SharePoint

1. Execute the below command to build bundle your solution.

    ```
    gulp bundle --ship
    ```

2. Execute the below command to package your solution.

    ```
    gulp package-solution --ship
    ```

The output package (.sppkg) is available under the sharepoint/solution folder.

3. Upload the package (.sppkg) to the SharePoint app catalog.
4. Ensure that the Make this solution available to all sites in the organization option is selected, so that the web part can be used from the Microsoft Teams side.

    ![](/media/2020-01-14-spfx-build-ms-teams-personal-app/04.png)

5. Click **Deploy**.
6. Select the spfx-ms-teams-personal-app package in the SharePoint tenant App Catalog and click the Sync to Teams button in the ribbon at the Files tab to make the web part available in Microsoft Teams.

![](/media/2020-01-14-spfx-build-ms-teams-personal-app/05.png)


## Test Personal App in MS Teams

Follow the below steps to verify the SPFx web part appearing as Personal App in MS Teams.

1. Open MS Teams desktop application.
2. Click **Apps** > **Built for < tenant >**.
3. Click our app (MyTeamsPersoanlApp).

    ![](/media/2020-01-14-spfx-build-ms-teams-personal-app/06.png)

4. Click **Add**.

    ![](/media/2020-01-14-spfx-build-ms-teams-personal-app/07.png)

5. The SPFx web part is added as a personal app.

![](/media/2020-01-14-spfx-build-ms-teams-personal-app/08.png)


## Summary

A personal app in MS Teams is an app with a personal scope. SPFx v1.10 now supports creating solutions in Microsoft Teams for building the personal app. Personal apps offer a view of your key application data from across teams and can either be acquired individually or deployed across Teams.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-v1-10-build-ms-teams-personal-app/).
