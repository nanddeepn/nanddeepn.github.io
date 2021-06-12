---
title: "LinkedIn feature in Office 365"
date: "2021-06-12"
share: true
header:
  image: media/2021-06-12-linkedin-feature-o365/01.png
  teaser: media/2021-06-12-linkedin-feature-o365/01.png
categories:
  - Office 365
tags:
  - "2021"
  - June 2021
last_modified_at: 2021-06-12T00:00:00-00:00
---

## Overview

LinkedIn feature in Office 365 provides more better collaboration, when connection between Office 365 accounts matches with the LinkedIn profiles.

In this article, we will explore this LinkedIn feature in Office 365 and how to configure it.

## The LinkedIn feature

With the LinkedIn feature, we can allow users from the organization to access their LinkedIn connections from Microsoft apps including SharePoint, Outlook, etc.

The LinkedIn account connections setting are enabled by default on Azure AD organizations (except Government clouds). The account connection works after the users consent to apps accessing company data on their behalf.

In SharePoint, the profile card shows the LinkedIn feature:

![](/media/2021-06-12-linkedin-feature-o365/01.png)

## Configure LinkedIn account connections

The LinkedIn account connections can be configured from Azure AD admin center as follows:

1. Open [Azure AD admin center](https://aad.portal.azure.com).

2. Click **Users** > **User settings**.

    ![](/media/2021-06-12-linkedin-feature-o365/02.png)

3. From **LinkedIn account connections** , we can configure to allow users to connect their work or school account with LinkedIn.

    **Yes** : Enables LinkedIn account connections to all users in the organization.

    **Selected group** : Enables LinkedIn account connections to only a single group in the organization. We can specify security group or Microsoft 365 group.

    **No** : Disables LinkedIn account connections to all users in the organization. If your organization is concerned about the data sharing with LinkedIn, this feature can be simply turned off by setting the value as No.

> Note:
> Data sharing between O365 tenant and LinkedIn is not enabled until users consent to connect their Microsoft work or school account with their LinkedIn account.

## Summary

With the LinkedIn feature, we can allow users from the organization to access their LinkedIn connections from Microsoft apps including SharePoint, Outlook, etc. This feature can be enabled for a set of users or can be turned off if the Organization is concerned about the data sharing with LinkedIn.

## References

- [Integrate LinkedIn account connections in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/linkedin-integration?WT.mc_id=M365-MVP-5003693)
- [LinkedIn account connections data sharing and consent](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/linkedin-user-consent?WT.mc_id=M365-MVP-5003693)
