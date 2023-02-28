---
title: "Using MGT with SPFx"
date: "2023-02-28"
share: true
header:
  image: media/2023-02-28-using-mgt-spfx/04.png
  teaser: media/2023-02-28-using-mgt-spfx/04.png
categories:
  - SharePoint Framework
tags:
  - "2023"
  - February 2023
last_modified_at: 2023-02-28T00:00:00-00:00
---
## Overview

Microsoft Graph Toolkit (MGT) components can now be easily used with SharePoint Framework (SPFx) solutions to build the solutions quickly with fewer coding efforts.

In this article, we will explore how to use the MGT components in SPFx solutions.


## MGT Overview

Microsoft Graph Toolkit (MGT) is a collection of reusable components to work with Microsoft Graph. It offers providers to enable authentication to get the access token and Microsoft Graph client to call Microsoft Graph APIs.

More information about MGT can be found at [aka.ms/mgt](https://aka.ms/mgt).


## Deploy MGT package to Tenant

Before you start using MGT components in your SPFx solution, you first need to deploy the MGT package to your tenant app catalog and deployed it to all sites.

The recent or previous version of the MGT package can be downloaded from [here](https://github.com/microsoftgraph/microsoft-graph-toolkit/releases).

![](/media/2023-02-28-using-mgt-spfx/01.png)

> Note: Only one version of the MGT package for SPFx can be deployed in a tenant. Make sure you are targeting the correct version.


## Create SPFx solution

Let us start by creating an SPFx solution by running the below command on the command prompt.

```
yo @microsoft/sharepoint
```

![](/media/2023-02-28-using-mgt-spfx/02.png)

The solution is created with SPFx version 1.16.1 as a No Framework solution.


## Install Microsoft Graph Toolkit SharePoint Framework package

Execute the below command to add the `@microsoft/mgt-spfx package`. This will help to load the MGT components from the library.

```
npm install @microsoft/mgt-spfx
```

## Import the SharePoint Provider

To start with, import `Microsoft Graph Toolkit Provider` and `SharePointProvider` from the `@microsoft/mgt-spfx package`.

```typescript
import { Providers, SharePointProvider } from '@microsoft/mgt-spfx';
```

Update the OnInit method as follows:

```typescript
protected async onInit() {
    if (!Providers.globalProvider) {
        Providers.globalProvider = new SharePointProvider(this.context);
    }
}
```

## Use MGT components

We will use the Person component in MGT to display the current user information. Use the below code to load the MGT components in the web part.

```typescript
public render(): void {
    this.domElement.innerHTML = `
        <div>
            <mgt-person-card person-query="me"></mgt-person-card>
        </div>`;
}
```

## Microsoft Graph permissions

To work with the MGT components, we need certain Graph API permissions that can be handled from config\package-solution.json

```json
"solution": {
    ...
    "webApiPermissionRequests": [
        {
            "resource": "Microsoft Graph",
            "scope": "User.ReadBasic.All"
        }
    ],
    ...
}
```


## Grant the API Permissions

Now, we need to prepare the SPFx package and deploy it to the app catalog. To prepare an SPFx solution package, execute the below set of commands:

Bundle the client-side solution:

```
gulp bundle --ship
```

Package the client-side solution:

```
gulp package-solution --ship
```

Upload the package to the SharePoint tenant app catalog. Grant the needed API permissions by navigating to the **API Access** tab from SharePoint Admin Center.

![](/media/2023-02-28-using-mgt-spfx/03.png)


## The end result

The web part will render the current user information as follows:

![](/media/2023-02-28-using-mgt-spfx/04.png)


## Summary

Microsoft Graph Toolkit (MGT) is a collection of reusable components to work with Microsoft Graph. MGT components can now be easily used with SharePoint Framework (SPFx) solutions to build the solutions quickly with fewer coding efforts.


## Code Download

The code developed during this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-mgt-no-framework).


## References

- [Microsoft Graph Toolkit](https://github.com/microsoftgraph/microsoft-graph-toolkit)
- [Using Microsoft Graph Toolkit with SPFx solutions](https://pnp.github.io/blog/post/spfx-17-microsoft-graph-toolkit-spfx/)
- [SharePoint Framework library for Microsoft Graph Toolkit](https://learn.microsoft.com/en-us/graph/toolkit/get-started/mgt-spfx?WT.mc_id=M365-MVP-5003693)
- [Person component in Microsoft Graph Toolkit](https://learn.microsoft.com/en-us/graph/toolkit/components/person?WT.mc_id=M365-MVP-5003693)
