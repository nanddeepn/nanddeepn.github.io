---
title: "Working with Multi-tenant enterprise APIs secured with Azure AD in SPFx"
date: "2024-02-25"
share: true
header:
  image: media/2024-02-25-multi-tenant-spfx/11.png
  teaser: media/2024-02-25-multi-tenant-spfx/11.png
categories:
  - SharePoint
  - Azure
tags:
  - "2024"
  - February 2024
last_modified_at: 2024-02-25T00:00:00-00:00
---
## Overview

There are times when we need to connect to a multi-tenant enterprise API secured with Azure Active Directory from a SharePoint Framework solution. It is a bit tricky and needs some configurations.

In this article, we will explore the configurations and details of working with Multi-tenant enterprise APIs secured with Azure AD in SPFx.


## Create Azure Function App

Let's start by creating an Azure function app.

![](/media/2024-02-25-multi-tenant-spfx/01.png)

Once the function app is ready, create a new HTTP-triggered function with the Authorization level set to Anonymous.

![](/media/2024-02-25-multi-tenant-spfx/02.png)


## Secure Function App with Azure AD Authentication

We will not secure the function app with Azure AD Authentication so to access it one needs to sign in with an organization account.

1. Navigate to the function app.
2. From the left menu, under **Settings** click **Authentication**.
3. Click **Add identity provider**.

    ![](/media/2024-02-25-multi-tenant-spfx/03.png)

4. Select the **Identity Provider** as **Microsoft**.

    ![](/media/2024-02-25-multi-tenant-spfx/04.png)

5. Now that when you try to access the Function URL in the browser, you should get redirected to the Azure AD login page.

    ![](/media/2024-02-25-multi-tenant-spfx/05.png)


## Make the application multi-tenant

When the Azure function is secured with the Azure AD application, only the users from the current tenant can access it. When you want to extend the API to different tenants, change the Azure AD application to multi-tenant.

Follow the below steps to make the Azure AD application as a multi-tenant:

1. Open the app registration from the Azure portal.
2. From the left menu, select the **Expose an API**.
3. As the Azure AD app will be used by other tenants, to ensure its uniqueness across all Azure Active Directories, update the App ID URI field.

    ![](/media/2024-02-25-multi-tenant-spfx/06.png)

4. Under **Authentication**, make the app as a multi-tenant.

    ![](/media/2024-02-25-multi-tenant-spfx/07.png)


## Allows users from other Azure ADs to use your application

As we want the API to be used by users from other tenants as well, we have to adjust the security settings.

1. Open the Azure Function app.
2. From the left menu, click **Authentication**.
3. Edit the configured Identity provider.

    ![](/media/2024-02-25-multi-tenant-spfx/08.png)

4. Clear the value in the **Issuer Url** field. This will allow users from other Azure Active Directories to authenticate against your API.
5. Set the **Allowed token audiences** as function URL.

    ![](/media/2024-02-25-multi-tenant-spfx/09.png)


## Enable CORS on Azure Function

As the function app will be called from another tenant via JavaScript running on a SharePoint page, we need to enable the CORS policy.

![](/media/2024-02-25-multi-tenant-spfx/10.png)


## Consume API from SPFx

Start by creating an SPFx extension.

![](/media/2024-02-25-multi-tenant-spfx/11.png)


## Add permissions to the enterprise API

Issue a permission request from the SharePoint Framework project by making the below changes to the config/package-solution.json file.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",
    "solution": {
        "name": "spfx-extension-client-side-solution",
        "id": "57e2d608-d570-4317-aa47-1a32e58ba441",
        "version": "1.0.0.0",
        "includeClientSideAssets": true,
        "skipFeatureDeployment": true,
        "isDomainIsolated": false,
        "webApiPermissionRequests": [
        {
            "resource": "spo-caller",
            "scope": "user_impersonation"
        }
        ],
        ...
    }
    ...
}
```

The value of the resource property refers only to the name of the Azure AD application used to secure the API.

Use the below code:

```typescript
this.context.aadHttpClientFactory
    .getClient('https://spo-caller.azurewebsites.net')
    .then((client: AadHttpClient): void=> {
        console.log(client);
    });
```

## Deploy the solution

Go to the **API access** page and approve the API request.

![](/media/2024-02-25-multi-tenant-spfx/12.png)

You might see below error:

The requested permission isn't valid. Reject this request and contact the developer to fix the problem and redeploy the solution.

![](/media/2024-02-25-multi-tenant-spfx/13.png)

I came across [this article](https://www.eliostruyf.com/approve-multitenant-permission-scope-spfx-solution/]) by [Elio Struyf](http://be.linkedin.com/in/estruyf) that explains this issue and the resolution.

In the browser type below URL

https://login.microsoftonline.com/common/adminconsent?client_id=\<Client-id\>

![](/media/2024-02-25-multi-tenant-spfx/14.png)


## Summary

Consuming multi-tenant enterprise APIs secured with Azure AD in SharePoint Framework is a tricky configuration. This article explores the steps and their significance.


## References

- [Consume multi-tenant enterprise APIs secured with Azure AD in SharePoint Framework](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/use-aadhttpclient-enterpriseapi-multitenant?WT.mc_id=M365-MVP-5003693)
