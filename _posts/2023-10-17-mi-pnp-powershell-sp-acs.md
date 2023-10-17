---
title: "Using Managed Identity with PnP PowerShell instead of SharePoint App-Only Principal"
date: "2023-10-17"
share: true
header:
  image: media/2023-10-17-mi-pnp-powershell-sp-acs/04.png
  teaser: media/2023-10-17-mi-pnp-powershell-sp-acs/04.png
categories:
  - SharePoint
tags:
  - "2023"
  - October 2023
last_modified_at: 2023-10-17T00:00:00-00:00
---
## Introduction

SharePoint App-Only is one of the older models of setting up app-principals which works well for SharePoint Online as well as on-premises. This model is getting deprecated and should not be used in future implementations.

In this article, we will explore using managed identities with PnP PowerShell for any SharePoint related operations.


## Grant access with SharePoint App-Only

In this model, we usually had the app registration done by creating an app at `https://tenant.sharepoint.com/_layouts/15/appregnew.aspx` and providing the needed permission. In most cases, `FullControl` permission on the tenant, I have seen. 😊

![](/media/2023-10-17-mi-pnp-powershell-sp-acs/sharepointapponly2.png)

Please refer to [Granting access using SharePoint App-Only](https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly-azureacs?WT.mc_id=M365-MVP-5003693) for revisiting the concept.


## Using Managed Identity in Azure Function with PnP PowerShell

**Create Function app**

Let us start with creating an Azure function.

From the Azure portal, create a Function app as follows:

![](/media/2023-10-17-mi-pnp-powershell-sp-acs/01.png)

In the Function app, let us create an `HTTP trigger-based` function with an Authorization level as a Function to perform SharePoint related operations.


**Add PnP PowerShell module to Azure function app**

Add the PnP PowerShell module to the Azure function app by following the below steps:

1. In the function app, from the left navigation under **Functions**, click **App files**.
2. Select the requirements.psd1 from the dropdown menu.
3. Add PnP.PowerShell module.

    ![](/media/2023-10-17-mi-pnp-powershell-sp-acs/02.png)


**Create system assigned managed identity for Azure function app**

Follow the below steps to create system assigned managed identity for the Azure function app:

1. In the Azure function app, from the left menu under **Settings**, click **Identity**.
2. Under the **system assigned** tab, make sure the status is turned **On**.

    ![](/media/2023-10-17-mi-pnp-powershell-sp-acs/03.png)


**Assign permissions to the managed identity**

We will use a managed identity in Azure to allow your Azure Function to connect to Microsoft Graph and SharePoint Online using PnP PowerShell.

To create a SharePoint site, we will need to set up below permissions:

| **Permission** | **Purpose** |
| --- | --- |
| Sites.FullControl.All | Create SharePoint site |
| Directory.ReadWrite.All | Create Group connected SharePoint site (e.g., Team site) |

Run the below cmdlets on PowerShell v7 to assign permissions to the managed identity.

```powershell
Add-PnPAzureADServicePrincipalAppRole -Principal "2db56a6c-9e95-4a2a-991b-b688bd11df4b" -AppRole "Directory.ReadWrite.All" -BuiltInType MicrosoftGraph
```

```powershell
Add-PnPAzureADServicePrincipalAppRole -Principal "2db56a6c-9e95-4a2a-991b-b688bd11df4b" -AppRole "Sites.FullControl.All" -BuiltInType SharePointOnline
```

The permissions can be seen from the corresponding Enterprise Application.

![](/media/2023-10-17-mi-pnp-powershell-sp-acs/04.png)


**Azure function to create the SharePoint site**

Use the below PnP PowerShell code to create the SharePoint site.

```powershell
using namespace System.Net

param($Request, $TriggerMetadata)

Connect-PnPOnline contoso.sharepoint.com -ManagedIdentity
Write-Host "Connected"

# Create communication site
New-PnPSite -Type CommunicationSite -Title "Contoso Communications" -Url "https://contoso.sharepoint.com/sites/contoso-communications" -Owner "admin@tenant.onmicrosoft.com"

# Create group connected team site
New-PnPSite -Type TeamSite -Title "Team Contoso" -Alias "ContosoTeam"

Disconnect-PnPOnline

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = [HttpStatusCode]::OK
})
```


## Summary

SharePoint App-Only model of setting up app-principals is getting deprecated and should not be used in future implementations. Instead, use managed identities with PnP PowerShell for any SharePoint related operations.


## References

- [Register SharePoint Add-ins](https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/register-sharepoint-add-ins?WT.mc_id=M365-MVP-5003693)
- [Granting access using SharePoint App-Only](https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly-azureacs?WT.mc_id=M365-MVP-5003693)
- [Using PnP PowerShell in Azure Functions](https://pnp.github.io/powershell/articles/azurefunctions.html)
- [Connect-PnPOnline](https://pnp.github.io/powershell/cmdlets/Connect-PnPOnline.html)
