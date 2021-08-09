---
title: "Information Barriers in MS Teams"
date: "2021-08-02"
share: true
header:
  image: media/2021-08-09-information-barriers-ms-teams/16.png
  teaser: media/2021-08-09-information-barriers-ms-teams/16.png
categories:
  - Office 365
  - MS Teams
tags:
  - "2021"
  - August 2021
last_modified_at: 2021-08-09T00:00:00-00:00
---
## Overview

Information barriers helps to restrict or allow users from certain segments from interacting with each other. It is a useful feature to maintain compliance across the organization.

In this article, we will define information barrier for MS Teams.

## Prerequisites for Information Barrier

To define the information barrier, we need to meet below prerequisites:

**Prerequisite# 1: licenses and permissions**

Information barriers are available in below subscriptions:

- Microsoft 365 E5/A5
- Office 365 E5/A5
- Office 365 Advanced Compliance
- Microsoft 365 Compliance E5/A5
- Microsoft 365 Insider Risk Management

To define information barrier policies, you must of either of below roles:

- Microsoft 365 global administrator
- Office 365 global administrator
- Compliance administrator
- IB Compliance Management


**Prerequisite# 2: Directory Data**

The organization's structure must be reflected in Azure AD by populating the correct user account attributes, such as group membership, department name, etc.

The common attributes including Company, Department, ExtensionAttribute(s), MemberOf, UsageLocation, and PostalCode can be used with information barriers.

Please see the complete list of attributes [here](https://docs.microsoft.com/en-us/microsoft-365/compliance/information-barriers-attributes?WT.mc_id=M365-MVP-5003693).

In our case, we have users from 3 departments including HR, Sales, and Research as follows:

![](/media/2021-08-09-information-barriers-ms-teams/01.png)


**Prerequisite# 3: Scoped directory search**

Before defining the information barrier policy, we should enable the scoped directory search in Microsoft Teams. It allows organizations to create virtual boundaries that control how users can find and communicate with other users in their organization.

Follow below steps to turn on scoped directory search:

1. Open MS Teams admin center.
2. From left menu, click **Org-wide settings** > **Teams settings**.
3. Toggle on the **Scope directory search using an Exchange address book policy** setting.

![](/media/2021-08-09-information-barriers-ms-teams/02.png)


**Prerequisite# 4: EXO license**

The target user must have an EXO license.


**Prerequisite# 5: Audit logging**

Office 365 audit logging must be turned on. From Office 365 Compliance Center, make sure you have it turned on.


**Prerequisite# 6: No Exchange address book policies**

Information barriers are based on address book policies, but the two kinds of policies are not compatible.
To see address book policies, run below PowerShell cmdlets:

```powershell
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline
Get-AddressBookPolicy
```


**Prerequisite# 7: PowerShell modules**

Information barrier policies are defined and managed in the Office 365 Security &amp; Compliance Center using PowerShell cmdlets. Install below modules:

```powershell
Install-Module -Name Az
Install-Module ExchangeOnlineManagement
```


**Prerequisite# 8: Admin consent for information barriers in Microsoft Teams**

Run below PowerShell

```powershell
Connect-AzAccount -Tenant "<tenant>.onmicrosoft.com"
$appId = "bcf62038-e005-436d-b970-2a472f8c1982"

$sp = Get-AzureADServicePrincipal -Filter "appid eq '$($appid)'"
if ($sp -eq $null) { New-AzureADServicePrincipal -ApplicationId $appId }

Start-Process "https://login.microsoftonline.com/common/adminconsent?client_id=$appId"
```

When prompted, sign into your Office 365 account.

![](/media/2021-08-09-information-barriers-ms-teams/03.png)

Review the requested permissions and **Accept**.


## Segment the users

Once the prerequisites are met, the first step is to define segments for your users.

**Determine the policies**

Determine the policies needed to block or allow communication between groups. We will implement the below scenario.

![](/media/2021-08-09-information-barriers-ms-teams/04.png)

Image reference: [https://docs.microsoft.com/en-us/sharepoint/information-barriers](https://docs.microsoft.com/en-us/sharepoint/information-barriers?WT.mc_id=M365-MVP-5003693)

**Determine the segments**

Based on the Department attribute, we will define 3 segments.

Segment 1: HR Segment

![](/media/2021-08-09-information-barriers-ms-teams/05.png)

Segment 2: Sales Segment

![](/media/2021-08-09-information-barriers-ms-teams/06.png)

Segment 3: Research Segment

![](/media/2021-08-09-information-barriers-ms-teams/07.png)


## Define Information Barrier Policies

Based on block or allow communication between groups, we will define below 2 scenarios:

### Scenario 1: Block communications between segments

Policy 1: Prevent Sales from communicating with Research

![](/media/2021-08-09-information-barriers-ms-teams/08.png)

Policy 2: Prevent Research from communicating with Sales

![](/media/2021-08-09-information-barriers-ms-teams/09.png)

### Scenario 2: Allow communications between segments

Policy 3: Allow Manufacturing to communicate with HR and Sales, Research only

![](/media/2021-08-09-information-barriers-ms-teams/10.png)

See all Information barrier policies using `Get-InformationBarrierPolicy`.

![](/media/2021-08-09-information-barriers-ms-teams/11.png)


## Apply information barrier policies

Set a policy to active status, using the `Set-InformationBarrierPolicy`.

![](/media/2021-08-09-information-barriers-ms-teams/12.png)

Repeat the above step for all the policies to make them active.

Use the `Start-InformationBarrierPoliciesApplication` cmdlet to apply active information barrier policies in the Microsoft 365 compliance center.

![](/media/2021-08-09-information-barriers-ms-teams/13.png)

Use the `Get-InformationBarrierPoliciesApplicationStatus` cmdlet to view the application status of information barrier policies.

At first place, the application status will be `NotStarted`

![](/media/2021-08-09-information-barriers-ms-teams/14.png)

The status will be turned to `Completed` when the active information barrier policies will be applied.

![](/media/2021-08-09-information-barriers-ms-teams/15.png)


## Test the Information Barriers

Follow below steps to test the information barriers in MS Teams:
1. Open MS Teams application with a user from Research department.
2. In the Add members screen of an existing team, search for user from Sales department. Due to information barrier policy "Research-Sales", users from Research department will not be able to search users from Sales department.

    ![](/media/2021-08-09-information-barriers-ms-teams/16.png)

3. On the other hand, users from HR department are searchable.

    ![](/media/2021-08-09-information-barriers-ms-teams/17.png)

## References

- [Information barriers in Microsoft 365](https://docs.microsoft.com/en-us/microsoft-365/compliance/information-barriers-solution-overview?WT.mc_id=M365-MVP-5003693)
- [Define information barrier policies](https://docs.microsoft.com/en-us/microsoft-365/compliance/information-barriers-policies?WT.mc_id=M365-MVP-5003693)
