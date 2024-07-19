---
title: "Managing SharePoint Site Permissions with Sites.Selected permission"
date: "2024-07-19"
share: true
header:
  image: media/2024-07-19-spo-sites-selected/02.png
  teaser: media/2024-07-19-spo-sites-selected/01.png
categories:
  - SharePoint
tags:
  - "2024"
  - July 2024
last_modified_at: 2024-07-19T00:00:00-00:00
---
## Overview

Traditionally, SharePoint permissions have been managed at the site level, with administrators granting access based on group memberships and roles. However, the introduction of the `Sites.Selected` permission provides a more granular approach to managing access, enhancing security and compliance. Microsoft Graph offers the option to implement granular permission levels using the `Sites.Selected` application permission for Microsoft Entra applications, rather than granting access to all sites within the tenant.

In this article, we will explore the SharePoint `Sites.Selected` permission, its benefits, and how to configure it.

## What is "Sites.Selected"?

`Sites.Selected` is a new permission model in SharePoint Online that allows administrators to specify access to particular sites for applications registered in Microsoft Entra ID (aka Azure AD). This permission model is designed to give organizations more control over which applications can access specific SharePoint sites, rather than granting broad permissions that cover the entire SharePoint environment.

## Key Features of "Sites.Selected"

- **Granular Access Control:** Instead of providing applications with access to all sites, administrators can now specify which sites an application can access. This minimizes the risk of unauthorized data access and ensures that applications only have access to necessary resources.
- **Enhanced Security:** By restricting application permissions to selected sites, organizations can better protect sensitive information and comply with data protection regulations.
- **Simplified Management:** Administrators can easily manage site-specific permissions through the SharePoint admin center or by using PowerShell commands. This streamlined approach simplifies the process of granting and revoking access.

## Set up Microsoft Entra ID (Azure AD) Application Registration

Firsly, we need to set up the Microsoft Entra ID (Azure AD) Application Registration with `Sites.Selected` permission for SharePoint.

![](/media/2024-07-19-spo-sites-selected/01.png)

## Set up authentication

We will connect to this app registration using PnP PowerShell. We will use AppID and Certificate approach to get connected.

Use below command to generate a self-signed certificate:

```powershell
New-PnPAzureCertificate -OutPfx pnp.pfx -OutCert pnp.cer
```

**Upload certificate**

Follow the below steps to upload the certificate to the Microsoft Entra ID (Azure AD) Application Registration

1. Open Microsoft Entra ID (Azure AD) Application Registration.
2. Under **Manage**, click **Certificates & secrets**.
3. Upload the "pnp.cer" certificate.
4. Note down the Thumbprint.

    ![](/media/2024-07-19-spo-sites-selected/02.png)

Double click "pnp.pfx" certificate and follow the wizard to get it imported to your machine.

**Connect to SharePoint**

Use below command to connect to the SharePoint:

```powershell
$siteUrl = "https://contoso.sharepoint.com/sites/spdemo"
$clientId = "<App registration Client ID>"
$certThumbprint = "<Certificate thumbprint>"
$tenant = "contoso.onmicrosoft.com"

Connect-PnPOnline -Url $siteUrl -ClientId $clientId -Thumbprint $certThumbprint -Tenant $tenant
```

As now we are connected the SharePoint, let us try to add an item to an existing SharePoint list using below command:

```powershell
Add-PnPListItem -List "Demo List" -Values @{"Title"="Nanddeep Nachan"}
```

The command fails with error: Attempted to perform an unauthorized operation

![](/media/2024-07-19-spo-sites-selected/03.png)

This is an expected error as we have not yet specified our SharePoint site to have permission to this app registration.

Let us add permission to the application registration by running the below command:

```powershell
Grant-PnPAzureADAppSitePermission -AppId $clientId -DisplayName "SharePoint Permission" -Permissions FullControl -Site <Site-URL>
```

![](/media/2024-07-19-spo-sites-selected/04.png)

## Test the solution

With the `Sites.Selected` permission in place for the selected SharePoint site, let us try adding the item to SharePoint list again.

![](/media/2024-07-19-spo-sites-selected/05.png)

And, it works this time!

## Conclusion

The `Sites.Selected` permission model in SharePoint Online provides a powerful option for administrators to control access to specific sites for registered applications. By implementing this, organizations can enhance security, ensure compliance, and streamline access management processes. As SharePoint continues to evolve, adopting granular permission models like `Sites.Selected` will be crucial for maintaining a secure and efficient collaboration environment.

## References

- [SharePoint now supports delegated Sites.Selected authentication](https://devblogs.microsoft.com/microsoft365dev/sharepoint-now-supports-delegated-sites-selected-authentication/?WT.mc_id=M365-MVP-5003693)
- [Develop Applications that use Sites.Selected permissions for SPO sites](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/develop-applications-that-use-sites-selected-permissions-for-spo/ba-p/3790476?WT.mc_id=M365-MVP-5003693)
- [New-PnPAzureCertificate](https://pnp.github.io/powershell/cmdlets/New-PnPAzureCertificate.html)
- [Grant-PnPAzureADAppSitePermission](https://pnp.github.io/powershell/cmdlets/Grant-PnPAzureADAppSitePermission.html)
