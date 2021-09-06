---
title: "Information Barriers in SharePoint and OneDrive"
date: "2021-09-06"
share: true
header:
  image: media/2021-09-06-information-barriers-spo-odb/01.png
  teaser: media/2021-09-06-information-barriers-spo-odb/01.png
categories:
  - Office 365
  - SharePoint
  - OneDrive
tags:
  - "2021"
  - September 2021
last_modified_at: 2021-09-06T00:00:00-00:00
---
## Overview

Information barriers helps to restrict or allow users from certain segments from interacting with each other. It is a useful feature to maintain compliance across the organization.

In this article, we will define information barrier for SharePoint Online and OneDrive.

## Enable Information Barriers in SharePoint and OneDrive

SharePoint Administrators or Global Administrators can enable information barriers in SharePoint Online.

Download and install the latest version of SharePoint Online Management Shell. Run below command to enable information barriers in SharePoint and OneDrive:

```powershell
Set-Spotenant -InformationBarriersSuspension $false
```

> Wait for approximately 1 hour for the changes to take effect.

## Manage segments

On the Active Sites tab in SharePoint admin center, the Segments column displays the first segment associated with the site.

![](/media/2021-09-06-information-barriers-spo-odb/01.png)

The segments can be managed from Policies section at an individual site.

![](/media/2021-09-06-information-barriers-spo-odb/02.png)

Select the **Edit** to add / remove segments to SharePoint site.

![](/media/2021-09-06-information-barriers-spo-odb/03.png)

## PowerShell to manage segments on SharePoint site

On the PowerShell, run below command

```powershell
Import-Module Az

Import-Module ExchangeOnlineManagement

Connect-AzAccount -Tenant "TENANT.onmicrosoft.com"

Connect-IPPSSession

Get-OrganizationSegment | ft Name, EXOSegmentID
```

![](/media/2021-09-06-information-barriers-spo-odb/04.png)

Apply the segment to SharePoint site by running below command:

```powershell
Connect-SPOService

Set-SPOSite -Identity <site URL> -AddInformationSegment <segment GUID>
```

> Note: Specify OneDrive site URL to apply segment to OneDrive sites.

To get the segment associated with the site, use below command:

```powershell
Set-SPOSite -Identity <site URL> | Select InformationSegment
```

## Auditing

Below audit events are available in Office 365 audit log:

- Segments are added to a site
- Segments are changed on a site
- Segments are removed from a site

## Site creation and management

Below points will be applicable on the site creation and management:

- When a segmented user creates a SharePoint site, the site is associated with the user's segment.
- Site owners can add more segments to the site.
- Site owners cannot remove added segments from sites.
- Up to 100 compatible segments can be associated with a site.
- Segments associated with the Microsoft Team team's members are automatically associated with the site within 24 hours.
- SharePoint admins can't change the segments associated with a site when the site is connected to a team.

## Site sharing

Sharing a site will have below effects when a segment is associated with site:

- Share with "Anyone with the link" option is disabled.
- The site and its content can be shared only with users whose segment matches that of the site.
- New users can be added to the site as site members only if their segment matches that of the site.

## Summary

Information barriers helps to restrict or allow users from certain segments from interacting with each other. Segments can be applied to SharePoint and OneDrive sites to allow or restrict communication between two groups.

## References

- [Use information barriers with SharePoint](https://docs.microsoft.com/en-us/sharepoint/information-barriers?WT.mc_id=M365-MVP-5003693)
- [Use information barriers with OneDrive](https://docs.microsoft.com/en-us/onedrive/information-barriers?WT.mc_id=M365-MVP-5003693)
