---
title: "Microsoft Ignite 2020 Takeaways: New SharePoint Admin Capabilities"
date: "2020-09-27"
share: true
header:
  image: media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/SP-admin_Ignite20_002_home-page-1.png
  teaser: media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/SP-admin_Ignite20_002_home-page-1.png
categories:
  - MS Teams
tags:
  - "2020"
  - September 2020
last_modified_at: 2020-09-27T00:00:00-00:00
---

## Overview

Microsoft Ignite 2020 was an exciting event as ever. Many new features were announced giving more power to the users.
In this article, we will have a look at newer SharePoint admin capabilities.

> **Note**
>
> Please note, this article is my notes to self, based on my takeaways from Microsoft Ignite 2020. I am waiting for a few features to roll out to my M365 tenant. The reference included at the end of this article will give you more insights.


## SharePoint admin announcements

SharePoint admin center is gradually getting aligned with Microsoft 365 suite design. During Microsoft Ignite 2020, Microsoft announced several new updates to the overall SharePoint admin center experience.

We have a huge increase in the adoption of MS Team due to the pandemic situation. MS Teams is supported by a SharePoint site. As there is a new team, a new SharePoint site gets created in the background. With the increase in the number of teams getting created, there will be more number of SharePoint sites too, eventually more SharePoint administration responsibilities.

The newer enhancement capabilities are mostly focused on:

- Admin roles and experiences
- New tools to manage the sites better
- Insights for monitoring and reporting


## Admin roles and experiences

While on-premises the management of SharePoint sites was only focused on SharePoint. But, as we make a move to the cloud with Microsoft 365, a SharePoint site is associated with many other things like MS Teams, Outlook, Office 365 Group, etc. In turn, a lot of roles are changing.

To manage all these things, we have many admin centers to work with including MS Azure AD admin center, M365 admin center, Security and compliance center, OneDrive for Business admin center, Exchange admin center, Stream admin center, Yammer admin center, MS Teams admin center, SharePoint admin center. Each one has its own user experience.

Microsoft is working to unify this experience by making a consistent design, naming conventions, and look &amp; feel of all admin centers.


## Roles

Over time, Microsoft has added many granular roles to help administrators to limit the privileges of the user. This helps to keep in control the security risks and achieve the principle of least privileges.

> **Note**
> 
> Microsoft recommends no more than 5 global administrators (even for large organizations).

**Global reader** is one of the recently introduced role. Users in this role can view admin features and settings in all admin centers that the Global admin can view, but can't edit any settings.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/01.png)

With the role-based access became available, we can create roles specific to certain functionality (e.g. Exchange admin, Compliance admin, Guest inviter, etc.) to help assist the Global administrator at the same time ensure that they will not have any additional roles apart from their designated responsibilities to avoid any possible security breaches.


## Coherent SharePoint Admin Center Experience

You can create customizable search cards including below:

- Site search
- Message center
- Service health
- Active users
- SharePoint site usage
- SharePoint file activity
- SharePoint storage usage
- Sites creation breakdown
- OneDrive usage
- OneDrive file activity
- Term store operations
- Sensitivity labels across sites


## Manage Active Sites from central admin

The SharePoint admin center now supports below:

**Edit Active Site Properties**

From the SharePoint Admin site, we can edit the properties of an active site.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/02.png)


**Site permissions**

The permissions now can be managed from the central admin.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/03.png)


**Activity**

Shows the activities on the site.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/04.png)


**Policy**

Shows the policies applied to the site.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/05.png)


**Bulk edit**

Now you can bulk edit the properties of sites.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/06.png)


**Views**

Various out of the box views available helps us to get more insights about a site.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/07.png)


**Filter and sort**

We can easily filter or sort the sites based on certain parameters.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/08.png)


**Custom views**

Now you can create your custom views. Each view has a unique URL that can be shared with other admins. You can also set any OOB or custom view as your default view.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/09.png)


**Export**

The site's information can be easily exported in CSV based on the currently applied filters.

**Source of site creation**

SharePoint is now a destination for MS Teams. That means anyone including SharePoint admin and end-users can create the SharePoint site directly or indirectly. As a SharePoint admin, we should be able to track the source of site creation (who created the site and from where).

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/10.png)
 
*As this feature has not yet shown up in my M365 tenant, including the image reference from [SharePoint admin and migration announcements at Ignite 2020](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/sharepoint-admin-and-migration-announcements-at-ignite-2020/ba-p/1694309?WT.mc_id=M365-MVP-5003693){:target="_blank"}.*


**Teams connected sites**

We have a new view available as &quot;Sites connected to Teams&quot;, also a column Teams that help administrators identify the sites connected to the MS Teams.

*Awaiting this feature to turn up in my tenant, will add reference later.*


## Swap the root site from UI

Earlier we had a PowerShell option [Invoke-SPOSiteSwap](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/invoke-spositeswap?WT.mc_id=M365-MVP-5003693) to swap the root site. The Replace Site button from UI helps to do this without any PowerShell involvement.

![](/media/2020-09-27-ms-ignite-2020-spo-admin-capabilities/11.png)


## Summary

Microsoft Ignite 2020 was full of exciting features. The SharePoint Admin Capabilities announced will help the administrators to excel in their day to day activities.


## References

- [SharePoint admin and migration announcements at Ignite 2020](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/sharepoint-admin-and-migration-announcements-at-ignite-2020/ba-p/1694309?WT.mc_id=M365-MVP-5003693){:target="_blank"} by Mark Kashman
- [Innovations for workplace communications and employee engagement in Microsoft 365](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/innovations-for-workplace-communications-and-employee-engagement/ba-p/1696149?WT.mc_id=M365-MVP-5003693){:target="_blank"} by Dan Holme
