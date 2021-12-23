---
title: "Customize Profile Cards with Graph API"
date: "2021-12-23"
share: true
header:
  image: media/2021-12-23-customize-profile-cards-graph-api/02.png
  teaser: media/2021-12-23-customize-profile-cards-graph-api/02.png
categories:
  - MS Graph
  - Office 365
tags:
  - "2021"
  - December 2021
last_modified_at: 2021-12-23T00:00:00-00:00
---
## Overview

Profile cards in Office 365 surface information of a user when we select someone's name or picture in Outlook or other apps in Office 365 (e.g. MS Teams, SharePoint). It is also referred to as a contact card or people card too.

In this article, we will explore how we can customize the profile card by adding a few additional properties to it using Graph API.

## Properties in Profile Card

The profile card by default shows the properties from Azure AD including below:

- Profile picture
- Contact information (including email, office location, department, job title, etc.)
- Organization information (including reports to and works with)
- Files (shared with you)
- Emails (emails between you and the user)
- LinkedIn (matching profiles), subject to if the LinkedIn feature is configured in Office 365. Please see my previous post on [LinkedIn feature in Office 365](../2021-06-12-linkedin-feature-o365/)

![](/media/2021-12-23-customize-profile-cards-graph-api/01.png)

## Extend Profile Card with Graph API

Graph API makes it possible to make additional attributes visible on the card as well as add custom attributes from Azure AD.

To list all `profileCardProperties`:

```
GET https://graph.microsoft.com/beta/organization/{organizationId}/settings/profileCardProperties
```

Below additional attributes can be displayed from Azure AD on the profile card:

| **Azure AD attribute** | **Graph User entity property** |
| --- | --- |
| UserPrincipalName | userPrincipalName |
| Fax | faxNumber |
| StreetAddress | streetAddress |
| PostalCode | postalCode |
| StateOrProvince | state |
| Alias | mailNickname |

To add an additional attribute (e.g. Alias) on the profile card, use below:

```
POST https://graph.microsoft.com/beta/organization/{tenantid}/settings/profileCardProperties

Content-Type: application/json

{
    "directoryPropertyName": "Alias"
}
```

## Custom attributes on a profile card

We can add any of the Azure AD custom extension attributes to users' profile cards. Custom properties are not searchable.

Adding custom attributes also supports localization. Custom attribute can be added as follows:

```
POST https://graph.microsoft.com/beta/organization/{tenantid}/settings/profileCardProperties

Content-Type: application/json

{
    "directoryPropertyName": "customAttribute1",
    "annotations": [
        {
            "displayName": "Cost center",
            "localizations": [
                {
                    "languageTag": "de",
                    "displayName": "Kostenstelle"
                }
            ]
        }
    ]
}
```

![](/media/2021-12-23-customize-profile-cards-graph-api/02.png)

## Update Custom Attributes

The custom attributes are the exchange properties. To update an individual user's custom attributes, follow below steps:

1. Open Microsoft 365 Admin Center.
2. Navigate to **Users** > **Active Users**.
3. Select any user.

    ![](/media/2021-12-23-customize-profile-cards-graph-api/03.png)

4. Under **Mail**, click **Edit Exchange properties**.

    ![](/media/2021-12-23-customize-profile-cards-graph-api/04.png)

Once you set the custom attributes, they will appear on the profile card.

> Note: 
> It may take up to 24 hours to get the custom attributes reflected on the profile card.

## Permissions

`User.ReadWrite`, `User.ReadWrite.All` delegated permissions will be needed for adding properties to the profile card for an organization.

## Use cases

If you want to surface the information from any third-party services, those can be populated to custom attributes in Azure AD.

## References

- [Profile cards in Microsoft 365](https://support.microsoft.com/en-us/office/profile-cards-in-microsoft-365-e80f931f-5fc4-4a59-ba6e-c1e35a85b501?WT.mc_id=M365-MVP-5003693)
- [Customize the profile card (preview)](https://docs.microsoft.com/en-us/graph/add-properties-profilecard?WT.mc_id=M365-MVP-5003693)
- [profileCardProperty resource type](https://docs.microsoft.com/en-us/graph/api/resources/profilecardproperty?WT.mc_id=M365-MVP-5003693)