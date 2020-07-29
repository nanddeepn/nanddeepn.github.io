---
title: "Accelerate SPFx Development with Microsoft Graph Toolkit"
date: "2019-09-29"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - Microsoft Graph
header:
  image: media/2019-09-29-accelerate-spfx-development-with-microsoft-graph-toolkit/03.png
  teaser: media/2019-09-29-accelerate-spfx-development-with-microsoft-graph-toolkit/03.png
tags:
  - "2019"
  - September 2019
last_modified_at: 2019-09-29T00:00:00-00:00
---

## Overview

The Microsoft Graph Toolkit (MGT) was introduced in May 2019 and is now generally available. It offers reusable, framework agnostic web-based UI components that works with Microsoft Graph without us writing a single line of code to connect with MS Graph and fetch the data. It does it all for us in the background.

In this article, will explore Microsoft Graph Toolkit, various controls offered by MGT, and how we can use Microsoft Graph Toolkit with SPFx.


## Microsoft Graph Toolkit Overview

Microsoft Graph Toolkit offers various controls to access Microsoft Graph without any need of customization. It is useful to develop web app, MS Teams tab and SharePoint web part. The MGT UI components provides consistent look and feel like Microsoft 365. If needed, the components can be customized using CSS.

Below are the major UI controls provided by MGT.

- Login: Provides sign in button to authenticate the user and display user profile information.
- Person: Displays person information like photo, name, email address.
- People: Displays group of people.
- Agenda: Displays user or group’s calendar events.
- Tasks: Displays users’ tasks from MS Planner or MS ToDo.
- People Picker: Control to search and select people.
- Person card: Displays detailed profile data about a person.


## Develop SharePoint Framework Web Part

1. Open a command prompt. Create a directory for SPFx solution.

    ```
    md spfx-mgt-components
    ```

2. Navigate to the above created directory.

    ```
    cd spfx-mgt-components
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-09-29-accelerate-spfx-development-with-microsoft-graph-toolkit/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-mgt-components in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for the component:** Here we can select the target environment where we are planning to deploy the client web part i.e. SharePoint Online or SharePoint On-Premises (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the same folder or create a sub-folder for our solution.
        - Selected choice: Same folder
    - **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
        - Selected choice: N (install on each site explicitly)
    - **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
        - Selected choice: N (solution contains unique permissions)
    - **Type of client-side component to create:** We can choose to create client side web part or an extension. Choose web part option.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: MGTDemo
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Use MGT with SPFx
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: No JavaScript Framework

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. In the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```


## NPM Packages Used

**@microsoft/mgt** (Microsoft Graph Toolkit)

On the command prompt, run below command to include the npm package.

```
npm install @microsoft/mgt --save
```


## Set Permission Scopes

To consume MS Graph or any third-party REST APIs, the permissions need to be explicitly set in the solution manifest.

Open "config\package-solution.json" and add below permission scopes.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",  
  "solution": {  
    "name": "spfx-mgt-components-client-side-solution",  
    "id": "4fd19c95-2757-49fe-ae53-0fdc2c1b3bbb",  
    "version": "1.0.0.0",  
    "includeClientSideAssets": true,  
    "isDomainIsolated": false,    
    "webApiPermissionRequests": [    
      { "resource": "Microsoft Graph", "scope": "User.Read" },  
      { "resource": "Microsoft Graph", "scope": "People.Read" },  
      { "resource": "Microsoft Graph", "scope": "Contacts.Read" },  
      { "resource": "Microsoft Graph", "scope": "User.ReadBasic.All" },  
      { "resource": "Microsoft Graph", "scope": "Calendars.Read" }  
    ]    
  },  
  "paths": {  
    "zippedPackage": "solution/spfx-mgt-components.sppkg"  
  }  
}  
```

Each of the MGT UI Component uses specific Microsoft Graph APIs and permissions, which should be specified in permission scopes.


## Use MGT in web part

1. Open web part file at "src\webparts\mgtDemo\MgtDemoWebPart.ts"
2. Use SharePoint provider to empower components with Microsoft Graph access.

    ```typescript
    import {Providers, SharePointProvider} from '@microsoft/mgt';
    ```

3. Initialize the provider inside onInit method.

    ```typescript
    protected async onInit() {  
      Providers.globalProvider = new SharePointProvider(this.context);  
    }
    ```

4. Implement render method to display person and agenda UI controls.

```typescript
public render(): void {  
  this.domElement.innerHTML = `  
    <div class="${ styles.mgtDemo }">  
      <div class="${ styles.container }">  
        <div class="${ styles.row }">              
            <mgt-person person-query="me" person-card="hover"></mgt-person>              
  
            <mgt-agenda group-by-day></mgt-agenda>  
        </div>  
      </div>  
    </div>`;  
}  
```


## Deploy SPFx Package to SharePoint App Catalog

Follow below steps to deploy the SPFx package (.sppkg) to SharePoint app catalog.

1. Build minified assets.

    ```
    gulp bundle --ship
    ```

2. Prepare the package.

    ```
    gulp package-solution --ship
    ```

3. The .sppkg package will be available inside the "sharepoint\\solution" folder. Upload package to the app catalog.
4. Please go to the API Management Page to approve pending permissions.

![](/media/2019-09-29-accelerate-spfx-development-with-microsoft-graph-toolkit/02.png)


## Test the WebPart

1. On the command prompt, type "gulp serve".
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Locate and add the webpart (named MGTDemo) to page.

![](/media/2019-09-29-accelerate-spfx-development-with-microsoft-graph-toolkit/03.png)


## What’s Good about MGT?

One good thing to notice about the MGT components, it does not generate an alert "To view the information on this page, you need to verify your identity". This was one of the common issue with using MSGraphClient to connect to the MS Graph.


## Summary

Microsoft Graph Toolkit (MGT) offers reusable, framework agnostic web-based UI components that works with Microsoft Graph without us writing a single line of code to connect with MS Graph and fetch the data. It is useful to develop web app, MS Teams tab and SharePoint web part. The MGT UI components provides consistent look and feel like Microsoft 365.

This content was originally posted [here](https://www.c-sharpcorner.com/article/accelerate-spfx-development-with-microsoft-graph-toolkit/).
