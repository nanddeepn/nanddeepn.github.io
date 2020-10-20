---
title: "SharePoint Framework - Consume Microsoft Graph API using AadHttpClient"
date: "2018-09-05"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - MS Graph
header:
  image: media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/04.png
  teaser: media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/04.png
tags:
  - "2018"
  - September 2018
last_modified_at: 2018-09-05T00:00:00-00:00
---

## Overview

In the article Consume Microsoft Graph API Using MSGraphClient, we explored how to consume the Microsoft Graph using MSGraphClient.

In this article, we will explore to consume the Microsoft Graph in SharePoint Framework client side web parts using AadHttpClient.


## Brief about Microsoft Graph

MS Graph is a rich and fast growing set of REST APIs provided by Microsoft to access content and services provided by Office 365. For example using Microsoft graph we can access mailbox, calendar, and one drive for business (OD4B) of a user. As well as we can access Site collection, sites, and lists in SharePoint Online. Also, we can access Office 365 Groups, Teams using MS Graph.

![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/01.png)

Read more about MS Graph at [https://developer.microsoft.com/en-us/graph/docs/concepts/overview](https://developer.microsoft.com/en-us/graph/docs/concepts/overview)


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-msgraph-aadhttpclient
    ```

2. Navigate to above created directory.

    ```
    cd spfx-msgraph-aadhttpclient
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint --plusbeta
    ```

    Since AadHttpClient is in beta, use ```--plusbeta``` with Yeoman generator.

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/02.png)

    - **Solution Name:** Hit enter to have default name (spfx-msgraph-aadhttpclient in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
        - Selected choice: Same folder
    - **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
        - Selected choice: N (install on each site explicitly)
    - **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: MSGraphAADHttpClient
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Consume MS Graph with SPFx using AADHttpClient
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: React

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## Access MS Graph using AadHttpClient

Microsoft Graph can be accessed by low-level type used to access Azure AD secured REST API (AadHttpClient). AadHttpClient client object can be used to consume any REST API, whereas MSGraphClient client object can only consume Microsoft Graph.

In the MsGraphAadHttpClient.tsx file under "\src\webparts\msGraphAadHttpClient\components\" folder, add below import statement.

```typescript
import { AadHttpClient } from '@microsoft/sp-http';  
```


## Permissions

In order to consume MS Graph or any third party REST APIs, we need to explicitly specify the permissions in manifest of solution.

In the package-solution.json file under "config" folder, configure webApiPermissionRequests property to specify User.ReadBasic.All permission.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",  
  "solution": {  
    "name": "spfx-msgraph-aadhttpclient-client-side-solution",  
    "id": "f4213803-dc4b-42d9-b6c4-96e895ec02fe",  
    "version": "1.0.0.0",  
    "includeClientSideAssets": true,  
    "webApiPermissionRequests": [  
      {  
        "resource": "Microsoft Graph",  
        "scope": "User.ReadBasic.All"  
      }  
    ]  
  },  
  "paths": {  
    "zippedPackage": "solution/spfx-msgraph-aadhttpclient.sppkg"  
  }  
}
```

webApiPermissionRequests is an array of webApiPermissionRequest items where each item is defined as below:

- resource: name or the ObjectId (in Azure AD). E.g. Microsoft Graph
- scope: name or unique ID of the permission

Please refer the permission API documentation at - [https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)


## Configure Props

Define the context property in IMsGraphAadHttpClientProps.ts under "\src\webparts\msGraphAadHttpClient\components\".

```typescript
import { WebPartContext } from '@microsoft/sp-webpart-base';  
  
export interface IMsGraphAadHttpClientProps {  
  description: string;  
  context: WebPartContext;  
}
```


## Configure State

We will define the state for our React component.

To define interface to represent user, add file IUserItem.ts under "\src\webparts\msGraphAadHttpClient\components\".

```typescript
export interface IUserItem {  
    displayName: string;  
    mail: string;  
    userPrincipalName: string;  
}
```

Add file IMsGraphAadHttpClientState.ts under "\src\webparts\msGraphAadHttpClient\components\".

```typescript
import { IUserItem } from './IUserItem';  
  
export interface IMsGraphAadHttpClientState {  
    users: Array<IUserItem>;  
}
```


## Get User Details

Implement below method to get the user details from your tenant.

```typescript
private getUserDetails(): void {  
  const aadClient: AadHttpClient = new AadHttpClient(  
    this.props.context.serviceScope,  
    "https://graph.microsoft.com"  
  );  

  // Get users with givenName, surname, or displayName  
  aadClient  
    .get(  
      `https://graph.microsoft.com/v1.0/users?$select=displayName,mail,userPrincipalName`,  
      AadHttpClient.configurations.v1  
    )  
    .then(response => {  
      return response.json();  
    })  
    .then(json => {  
      // Prepare the output array  
      var users: Array<IUserItem> = new Array<IUserItem>();  

      // Log the result in the console for testing purposes  
      console.log(json);  

      // Map the JSON response to the output array  
      json.value.map((item: any) => {  
        users.push( {   
          displayName: item.displayName,  
          mail: item.mail,  
          userPrincipalName: item.userPrincipalName,  
        });  
      });  

      // Update the component state accordingly to the result  
      this.setState(  
        {  
          users: users,  
        }  
      );  
    })  
    .catch(error => {  
      console.error(error);  
    });  
}
```


## Enable Targeted Release on your Tenant

The MS Graph operation is part of an experimental feature and is only available in Targeted release (first release) tenants

1. Open Office 365 admin center.
2. Click **Settings** > **Organization profile**.
3. Click **Edit** against **Release preferences**.
4. Select the preference.

![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/03.png)

Please refer article here ([https://support.office.com/en-us/article/set-up-the-standard-or-targeted-release-options-in-office-365-3b3adfa4-1777-4ff0-b606-fb8732101f47#bkmk_setup](https://support.office.com/en-us/article/set-up-the-standard-or-targeted-release-options-in-office-365-3b3adfa4-1777-4ff0-b606-fb8732101f47#bkmk_setup)) to setup your tenant for targeted release.


## Test the WebPart

1. On the command prompt, type **gulp serve**.
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Add the webpart to page.

    ![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/04.png)


# API Management

In the Production environment, after deploying the web part follow below steps to approve API requests.

1. Open SharePoint Admin Center (https://[tenant]-admin.sharepoint.com)
2. Click **Try the preview**.

    ![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/05.png)

3. From left navigation, Click **API Management**.
4. Approve the pending requests.

![](/media/2018-09-05-spfx-consume-ms-graph-api-using-aadhttpclient/06.png)


## Summary

Microsoft Graph offers wide range of REST APIs to access content and services provided by Office 365. The MS Graph operation is part of an experimental feature and is only available in Targeted release (first release) tenants only. AadHttpClient is in preview mode, avoid using it in Production.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-consume-microsoft-graph-api-using-aadhttpclient/).
