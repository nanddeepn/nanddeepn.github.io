---
title: "Shared Channels in MS Teams"
date: "2022-05-16"
share: true
header:
  image: media/2022-05-27-shared-channels-ms-teams/08.png
  teaser: media/2022-05-27-shared-channels-ms-teams/08.png
categories:
  - MS Teams
tags:
  - "2022"
  - May 2022
last_modified_at: 2022-05-27T00:00:00-00:00
---

## Overview
The Shared Channels feature in MS Teams is still in preview and is rolling to the M365 tenants. It provides a flexible way to collaborate with people from different teams and organizations which makes it easy to work without tenant switching for vendors, partners, and customers.

In this article, we will cover how to work with the shared channels.

## Enable Shared Channels
Once the Shared Channel feature is available in your tenant, it will be turned on by default. MS Teams administrator can configure the Shared channel settings as follows:

1. Open [Teams admin center](https://admin.teams.microsoft.com/).
2. From the left menu, click **Teams** > **Teams policies**.

   ![](/media/2022-05-27-shared-channels-ms-teams/01.png)

3. Configure your desired settings.
    1. **Create shared channels**: Team owners can create shared channels for people within and outside the organization. Only people added to the shared channel can read and write messages.
    2. **Invite external users to shared channels**: Owners of a shared channel can invite external users to join the channel, if Azure AD external sharing policies are configured.
    3. **Join external shared channels**: Users and teams can be invited to external shared channels, if Azure AD external sharing policies are configured.

## Enable developer preview

At the moment, you will have to enable the developer preview in MS Teams to use the Shared Channel experience.

Follow the below steps to enable the developer preview:

1. Open **MS Teams**.
2. Click **…** > **About** > **Developer preview**.
    ![](/media/2022-05-27-shared-channels-ms-teams/02.png)

## Invite external teams or users to the Shared channel

People outside your organization can be invited to collaborate in the Shared channel using Azure AD B2B direct connect.

Follow the below steps to enable the B2B collaboration:
1. Open [Azure portal](https://portal.azure.com/).
2. Click **Azure Active Directory**
3. Select **External Identities**
4. Select **Cross-tenant access settings (Preview)**
5. Under **Organization settings**, click **+ Add organization**.
6. Type the invitee's Tenant ID or domain name.
7. Click **Add**.

![](/media/2022-05-27-shared-channels-ms-teams/03.png)

**Inbound access settings**:

B2B collaboration inbound access settings determine whether users from the invited domain can be invited to your organization and added to your tenant as guests.

![](/media/2022-05-27-shared-channels-ms-teams/04.png)

**Outbound access settings**: 

B2B collaboration outbound access settings determine whether your users can be invited to other domain for B2B collaboration and added to their directory as guests.

![](/media/2022-05-27-shared-channels-ms-teams/05.png)

## Configurations at the partner end
The same settings also need to be enabled on the partner side. Ask the partner tenant admin to perform the below steps to enable the B2B collaboration:
1. Open [Azure portal](https://portal.azure.com/).
2. Click **Azure Active Directory**
3. Select **External Identities**
4. Select **Cross-tenant access settings (Preview)**
5. Under **Organization settings** , click **+ Add organization**.
6. Type the invitee's Tenant ID or domain name.
7. Click **Add**.

![](/media/2022-05-27-shared-channels-ms-teams/06.png)

Configure the `Inbound access settings` and `Outbound access settings` as shown previously.

## Create Shared Channel
In the Microsoft Teams team, choose to create a new channel by selecting privacy as the shared channel.

![](/media/2022-05-27-shared-channels-ms-teams/07.png)

Once the channel is created, you can share the channel with external users.

![](/media/2022-05-27-shared-channels-ms-teams/08.png)

Add the user from another domain and share the channel with the user.

![](/media/2022-05-27-shared-channels-ms-teams/09.png)

## The Partner Experience
Now, let us see the experience from the invited partner.

The invited user from another tenant will get a notification in MS Teams as follows:

![](/media/2022-05-27-shared-channels-ms-teams/10.png)

The invited user will be able to see the team from the invitee tenant and can collaborate without tenant switching.

![](/media/2022-05-27-shared-channels-ms-teams/11.png)

## Summary
Shared channels provide a flexible way to collaborate with people from different teams and organizations which makes it easy to work without tenant switching for vendors, partners, and customers.

## References
- [Shared channels in Microsoft Teams (Preview)](https://docs.microsoft.com/en-us/microsoftteams/shared-channels?WT.mc_id=M365-MVP-5003693)
- [Plan external collaboration](https://docs.microsoft.com/en-us/microsoft-365/solutions/plan-external-collaboration?WT.mc_id=M365-MVP-5003693)