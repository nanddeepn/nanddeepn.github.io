---
title: "Installing the PnP PowerShell to Azure Automation Account"
date: "2023-08-25"
share: true
header:
  image: media/2023-08-25-install-pnp-ps-az-automation/04.png
  teaser: media/2023-08-25-install-pnp-ps-az-automation/04.png
categories:
  - SharePoint
  - Azure
tags:
  - "2023"
  - August 2023
last_modified_at: 2023-08-25T00:00:00-00:00
---
## Overview

PnP (Patterns and Practices) PowerShell is one of the widely used PowerShell modules for SharePoint related operations. The PnP PowerShell module is used for managing SharePoint and Microsoft 365 environments through PowerShell scripting. It can be easily used with Azure Automation and Azure Function app.

In this article, we will explore using PnP PowerShell with Azure Automation and targeting its specific version.

## Install PnP PowerShell to Azure Automation

Azure Automation account comes with pre-installed Azure PowerShell modules. Follow the below steps to install a custom PowerShell module (e.g., PnP PowerShell).

1. Open the Automation Account.
2. From the left menu, under **Shared Resources** click **Modules**.
3. Click **+ Add a module**.

    ![](/media/2023-08-25-install-pnp-ps-az-automation/01.png)

4. In the **Add a module** wizard, select **Browse from gallery**.
5. Search and select the PnP.PowerShell module.
6. Select your preferred runtime version.
7. Click **Import**.

    ![](/media/2023-08-25-install-pnp-ps-az-automation/02.png)

Grab a coffee and when you are back, you should have the PnP.PowerShell module added to your Automation account.

Easy! You should have the latest version of PnP.PowerShell module is available to use in the Automation account.

## Install from PowerShell Gallery

As an alternative, you can install the PnP PowerShell module from the PowerShell Gallery available at [https://www.powershellgallery.com](https://www.powershellgallery.com/)

**Install a specific PnP PowerShell version**

From the PowerShell Gallery, you can install previous versions of the PowerShell module by navigating to the Version History section.

![](/media/2023-08-25-install-pnp-ps-az-automation/03.png)

Follow the below steps to install to the Azure Automation:

1. Select the version of PnP PowerShell you want to install.
2. Under **Installation Options**, select **Azure Automation**.
3. Click **Deploy to Azure Automation**.

    ![](/media/2023-08-25-install-pnp-ps-az-automation/04.png)

4. This will take you to the Azure portal. Select the Automation account to install the module.

    ![](/media/2023-08-25-install-pnp-ps-az-automation/05.png)


**Limitation**

I have observed that if you have a huge listing of Automation accounts, this screen does not have the flexibility to search for your desired Automation account using search or paging. In such situations, you can use the PowerShell approach.

## PowerShell approach

To install the PowerShell module into Azure Automation, you can use the New-AzAutomationModule cmdlet.

Below is an example of installing PnP PowerShell v1.12.0 to the Azure automation account created in this article.

```powershell
New-AzAutomationModule
    -Name "PnP.PowerShell"
    -AutomationAccountName "az-pnp-automation"
    -ContentLinkUri "https://psg-prod-eastus.azureedgs.net/packages/pnp.powershell.1.12.0.nupkg"
    -ResouceGroupName "PnP-Automation"
```

`ContentLinkUri` refers to the URL to a module zip package. The question is - how to get ContentLinkUri?

Follow the below steps to get the URL to a module zip package:

1. Navigate to the PowerShell gallery page with the module to install.
2. In the browser, open Developer Tools (F12).
3. Under **Installation Options**, click **Manual Download**.
4. Click **Download the raw nupkg file**.
5. In the Developer tools, watch the **Network** tab.
6. Note down the URL of nupkg.

    ![](/media/2023-08-25-install-pnp-ps-az-automation/06.png)


## Why does a specific version of PnP PowerShell need to be installed?

Installing a specific version of the PnP PowerShell module might be necessary for various reasons including below:

- Compatibility: Different versions of the PnP PowerShell module might be designed to work with specific versions of SharePoint or Microsoft 365. Newer versions of the module might have features or capabilities that are only compatible with certain versions of the target environment.

- Bug Fixes and Enhancements: Newer versions of the module often include bug fixes, performance improvements, and new features. If you're experiencing issues or limitations with an older version, upgrading to a specific version might address those problems.

- Deprecation and API Changes: Microsoft periodically updates its APIs and services. As a result, older versions of the PnP PowerShell module might not be compatible with the latest changes in SharePoint or Microsoft 365. Installing a specific version that aligns with the current API and service versions can ensure that your scripts continue to function correctly.

- Feature Availability: Different versions of the PnP PowerShell module might expose different sets of cmdlets and functionalities. If you need to use specific features or cmdlets that are only available in certain versions, you'll need to install the appropriate version.

It's important to consult the documentation and release notes for the PnP PowerShell module to understand the specific changes and improvements introduced in each version. This will help you make an informed decision about which version to install based on your requirements and the compatibility with your target environment.


## Summary

PnP (Patterns and Practices) PowerShell is one of the widely used PowerShell modules for SharePoint related operations. There are various ways to import a specific PnP PowerShell version to Azure Automation, as demonstrated in this article.


## References

- [PowerShell Gallery](https://www.powershellgallery.com/packages/PnP.PowerShell/)
- [New-AzAutomationModule](https://learn.microsoft.com/en-us/powershell/module/az.automation/new-azautomationmodule?view=azps-10.2.0)
