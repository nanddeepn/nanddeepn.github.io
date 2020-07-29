---
title: "Isolated web parts in SPFx"
date: "2019-11-04"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-11-04-isolated-web-parts-in-spfx/01.png
  teaser: media/2019-11-04-isolated-web-parts-in-spfx/01.png
tags:
  - "2019"
  - November 2019
last_modified_at: 2019-11-04T00:00:00-00:00
---

## Overview

SharePoint Framework v1.6.0 onwards supported consuming the MS Graph APIs and custom APIs. This also implies that any of the script running on the tenant will eventually get access to these APIs. SPFx v1.8.0 onwards has addressed this situation by introducing isolated web parts. If your web part requires permission to access any backend API or Graph, it is strongly recommended to have the web part isolated.

In this article, we will explore the concept behind isolated web parts, how the isolated web part works, and how to develop & deploy them.


## The concept behind isolated web parts

Securing the Azure functions or other APIs with Azure AD authentication might not be enough. We should protect the access token which is used behind the scenes and make sure that no other script on the page will piggy-back the permissions we are using. Isolated web part is one step towards handling security concerns.


## How Isolated web parts work?

The SPFx package containing an isolated web part runs on a unique domain and is hosted in an iframe. The permissions granted only applies to code running on that unique domain. A dedicated Azure AD registration gets created for this SPFx solution which handles the authentication.

![Architectural overview illustrating how isolated web parts work](/media/2019-11-04-isolated-web-parts-in-spfx/01.png)

Image source: docs.microsoft.com


The isolated web part when added to the page, runs in an iframe.

![](/media/2019-11-04-isolated-web-parts-in-spfx/02.png)

The source is a dynamically created domain (mostly like SharePoint hosted apps)


## Develop SharePoint Framework Web Part

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-isolated-webpart
    ```

2. Navigate to the above created directory.

    ```
    cd spfx-isolated-webpart
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-11-04-isolated-web-parts-in-spfx/03.png)

    - **Solution Name:** Hit enter to have default name (spfx-isolated-webpart in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for the component:** Here we can select the target environment where we are planning to deploy the client web part i.e. SharePoint Online or SharePoint On-Premises (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the same folder or create a sub-folder for our solution.
        - Selected choice: Same folder
    - **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
        - Selected choice: N (install on each site explicitly)
    - **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
        - Selected choice: Y (isolated web part)
    - **Type of client-side component to create:** Generator will only allow to create a web part.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: IsolatedMSGraphClient
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Connect to MS Graph with isolated web part.
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


## Set Permission Scopes

To consume MS Graph or any third-party REST APIs, the permissions need to be explicitly set in the solution manifest.

Open "config\package-solution.json" and add below permission scope to give read permission on MS Graph for all users.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",  
  "solution": {  
    "name": "spfx-isolated-webpart-client-side-solution",  
    "id": "87a3cdd0-b3cd-4468-b973-3a147881a177",  
    "version": "1.0.0.0",  
    "includeClientSideAssets": true,  
    "isDomainIsolated": true,  
    "webApiPermissionRequests": [  
      {  
        "resource": "Microsoft Graph",  
        "scope": "User.ReadBasic.All"  
      }  
    ]  
  },  
  "paths": {  
    "zippedPackage": "solution/spfx-isolated-webpart.sppkg"  
  }  
}
```


## Deploy the SPFx Package to SharePoint App Catalog

Follow the below steps to deploy the SPFx package (.sppkg) to SharePoint app catalog.

**Build minified assets**

On the command prompt, type the below command.

```
gulp bundle --ship
```


**Prepare the package**

On the command prompt, type the below command.

```
gulp package-solution --ship
```

The .sppkg package will be available inside the "sharepoint\solution" folder.


**Deploy the package to app catalog**

1. Open the SharePoint app catalog site.
2. Upload and deploy the package to the app catalog.

![](/media/2019-11-04-isolated-web-parts-in-spfx/04.png)


## API Management

After deploying the web part, follow the below steps to approve API requests.

1. Open SharePoint Admin Center ([https://\[tenant\]-admin.sharepoint.com](https://[tenant]-admin.sharepoint.com)).
2. From left navigation, click "API Management".
3. Approve the pending request.

![](/media/2019-11-04-isolated-web-parts-in-spfx/05.png)


## Test the WebPart

1. On the command prompt, type ```gulp serve```.
2. Open SharePoint site.
3. Add the web part to the page.

![](/media/2019-11-04-isolated-web-parts-in-spfx/06.png)


## UI Considerations

Since the isolated web parts run in an iframe, you need to consider the situation that the content of your web part will be displayed in an iframe that has got pre-defined height and width. Certain UI components might not work well within iframe. If you are planning to implement popup functionality, then you will need to reserve the space in advance by adjusting dimensions of an iframe.


## Convert existing web parts to isolated

Yes, it is possible to convert an existing web part to an isolated web part. However, below are few of the considerations:

1. Your SPFx solution should be using at least v1.8.0.
2. In the 'config/package-solution.json' file, set the isDomainIsolated property to true.
3. Your project should contain only web parts.
4. After the changes, you should redeploy your solution and the new API permission must be approved by the tenant admin.


## Summary

SPFx v1.8.0 onwards has introduced isolated web parts. If your web part requires permission to access any backend API or Graph, it is strongly recommended to have the web part isolated. The SPFx package containing an isolated web part runs on a unique domain and is hosted in an iframe.

This content was originally posted [here](https://www.c-sharpcorner.com/article/isolated-web-parts-in-spfx/).
