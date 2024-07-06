---
title: "Restricted SharePoint Search - Enhancing Data Security and Governance"
date: "2024-07-05"
share: true
header:
  image: media/2024-07-05-restricted-sharepoint-search/01.png
  teaser: media/2024-07-05-restricted-sharepoint-search/01.png
categories:
  - SharePoint
  - Copilot
tags:
  - "2024"
  - July 2024
last_modified_at: 2024-07-05T00:00:00-00:00
---
## Overview

Restricted SharePoint Search is a powerful feature that allows organizations to curate search results and enhance data security within their SharePoint environment. By maintaining an "allowed list" of SharePoint sites, administrators can control which content appears in organization-wide search results and Copilot suggestions. Let's explore the key aspects of this feature.

Restricted SharePoint Search directly impacts the quality and security of Copilot's suggestions, ensuring that users receive valuable insights while adhering to data governance policies.

In this article, we will explore the Restricted SharePoint Search feature, how to enable and disable it, and its impact on enterprise search and Microsoft Copilot.


## What is Restricted SharePoint Search?

SharePoint is a versatile collaboration platform used by organizations to store, manage, and share documents, files, and other content. While SharePoint search provides a comprehensive way to discover content, it's essential to balance openness with data security and governance.

Restricted SharePoint Search is a setting designed for customers of Copilot for Microsoft 365. It allows Global and SharePoint administrators to maintain an "allowed list" of SharePoint sites. This list includes sites where permissions have been verified, and data governance has been applied. By default, this setting is turned off, and the allowed list is empty. When enabled, it restricts both organization-wide search and Copilot experiences to a curated set of SharePoint sites.

Here are the key benefits:

- **Oversharing Concerns:** Organizations can prevent sites from appearing in organization-wide search results and Copilot suggestions until admins or site owners review permissions.
- **Data Security and Governance:** While implementing comprehensive data security with SharePoint Advanced Management and Microsoft Purview, Restricted SharePoint Search ensures a secure and managed data lifecycle.
- **Site Permissions Management:** Site owners can still manage individual site permissions, even with Restricted SharePoint Search enabled.

As a Global or SharePoint administrator, you can:

- Maintain an "allowed list" of SharePoint sites.
- Limit search results to sites on this list, frequently visited sites, sites with existing permissions, and recently accessed files.
- Enabling this feature impacts the overall search experience, even for non-Copilot users.


## How Restricted SharePoint Search and Microsoft Copilot are related?

When users interact with Copilot within Microsoft 365 applications, they often rely on search functionality to find relevant documents, emails, or other content. Copilot leverages SharePoint search results to provide contextually relevant suggestions, recommendations, and information. By implementing Restricted SharePoint Search, organizations influence the search results that Copilot presents to users.

When Restricted SharePoint Search is enabled, Copilot’s suggestions are limited to content from verified SharePoint sites. This ensures data security and governance. Copilot respects the allowed list of sites, preventing unauthorized or sensitive content from appearing in its recommendations.

In simple words, Restricted SharePoint Search directly impacts the quality and security of Copilot’s suggestions, ensuring that users receive valuable insights while adhering to data governance policies.


## Enable Restricted SharePoint Search

Restricted SharePoint Search can be enabled or disabled via PowerShell as outlined below:

- Connect to SharePoint.

    ```powershell
    Connect-SPOService -Url <https://nachan2023-admin.sharepoint.com>
    ```

- Run below command to get the existing allowed list in the tenant. Result can be 'Enabled' or 'Disabled' based on the current setting.

    ```powershell
    Get-SPOTenantRestrictedSearchMode
    ```

Run below command to enable or disable the Restricted Search setting.

- Enable the Restricted Tenant Search mode for the tenant.

    ```powershell
    Set-SPOTenantRestrictedSearchMode -Mode Enabled
    ```

- Disable the Restricted Tenant Search mode for the tenant.

    ```powershell
    Set-SPOTenantRestrictedSearchMode -Mode Disabled
    ```

When Restricted SharePoint Search is enabled, you can add site URLs to the allowed list in string or csv file.

- Add site URLs

    ```powershell
    Add-SPOTenantRestrictedSearchAllowedList -SitesList @("https://contoso.sharepoint.com/sites/Marketing", "https://contoso.sharepoint.com/sites/Benefits")
    ```

- Add from csv file

    ```powershell
    Add-SPOTenantRestrictedSearchAllowedList  -SitesListFileUrl C:\UrlList.csv
    ```

Run below command to remove sites from the allow list.

- Remove site URLs

    ```powershell
    Remove-SPOTenantRestrictedSearchAllowedList -SitesList @("https://contoso.sharepoint.com/sites/Marketing", "https://contoso.sharepoint.com/sites/Benefits")
    ```

- Remove from URLs specified in csv file

    ```powershell
    Remove-SPOTenantRestrictedSearchAllowedList -SitesListFileUrl C:\UrlList.csv
    ```

Run below command to get the existing list of URLs in the allowed list:

```powershell
Get-SPOTenantRestrictedSearchAllowedList
```

After Restricted SharePoint Search is enabled, Copilot users will see the below message.

![](/media/2024-07-05-restricted-sharepoint-search/01.png)


## Summary

Restricted SharePoint Search strikes a balance between openness and security. Organizations can fine-tune their search experience while safeguarding sensitive data.


## References

- [Use PowerShell Scripts for Restricted SharePoint Search](https://learn.microsoft.com/en-us/sharepoint/restricted-sharepoint-search-admin-scripts?WT.mc_id=M365-MVP-5003693)
- [Introducing Restricted SharePoint Search to help you get started with Copilot for Microsoft 365](https://techcommunity.microsoft.com/t5/copilot-for-microsoft-365/introducing-restricted-sharepoint-search-to-help-you-get-started/ba-p/4071060?WT.mc_id=M365-MVP-5003693)
