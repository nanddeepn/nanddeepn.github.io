---
title: "Manage Conditional Access in Microsoft 365"
date: "2021-11-20"
share: true
header:
  image: media/2021-11-21-conditional-access-m365/06.png
  teaser: media/2021-11-21-conditional-access-m365/06.png
categories:
  - Office 365
tags:
  - "2021"
  - November 2021
last_modified_at: 2021-11-20T00:00:00-00:00
---
## Overview

Having access to everyone for every app in Microsoft 365 might not be a good idea. Access to each of the apps and services across Microsoft 365 should be restricted in a controlled way. Microsoft Azure's Conditional Access is helpful in these scenarios.

In this article, we will explore the importance of conditional access and how to use it in Microsoft 365 to control access to the apps.

## Why restrict access?

We have to think over whether it is nice to have access to all apps to everyone in the organization, even though an app is not relevant or not being used by them. Restricting access might be helpful in the below scenarios:

1. Limit the licensed use of the app.
2. Roll out of the service in the organization and be available to pilot users.
3. Better assignment of licenses via groups instead of to an individual at random.
4. Help secure the app and organization.

# What is Conditional Access?

Conditional access is like an if-then statement. If a user needs access to the resource, then they must complete the action.

# Apply Conditional Access to Microsoft 365 Apps

**Azure AD Security Group**

Consider planning for the Azure AD security group to have access to the app.

![](/media/2021-11-21-conditional-access-m365/01.png)

**Conditional Access Policy**

Create a new conditional access policy by following below steps:

1. Open [MS Azure portal](https://portal.azure.com).
2. Search for **Azure AD Conditional Access**.
3. Click **New Policy** > **Create new policy**.
4. Specify the name as **SPO Access Policy**.
5. Under **Assignments** > **Users or workload identities** , choose to **exclude** our AAD security group.

  ![](/media/2021-11-21-conditional-access-m365/02.png)

6. For **Cloud apps or actions**, choose the apps to include in the policy. E.g. **Office 365 SharePoint Online**.

  ![](/media/2021-11-21-conditional-access-m365/03.png)

7. Under **Conditions**, choose to include apps as shown below.

  ![](/media/2021-11-21-conditional-access-m365/04.png)

8. Under **Access Controls** > **Grant**, choose **Block Access**.

  ![](/media/2021-11-21-conditional-access-m365/05.png)

9. Enable the policy.
10. Click **Save**.

The policy can be read as &quot;Block access to everyone except AAD security group named SPO Access on SharePoint Online&quot;.

# Test the Policy

Members of the AAD security group named SPO Access should be able to browse the SharePoint sites. However, non-members should get the below error.

![](/media/2021-11-21-conditional-access-m365/06.png)

## Debug the access issue

When any of the users are facing an access issue because of the conditional access policy, follow the below steps to find out the root cause:

1. Open Azure AD.
2. Browse to Users section.
3. Search and select the user, facing an access issue.
4. Under **Activity**, click **Sign-in logs**.
5. Locate the one that resulted in a failure.

  ![](/media/2021-11-21-conditional-access-m365/07.png)

## Summary

Access to each of the apps and services across Microsoft 365 should be restricted in a controlled way. Microsoft Azure's Conditional Access is helpful in these scenarios.

## References
- [What is Conditional Access?](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/overview?WT.mc_id=M365-MVP-5003693)
- [Control access from unmanaged devices](https://docs.microsoft.com/en-us/sharepoint/control-access-from-unmanaged-devices?WT.mc_id=M365-MVP-5003693)