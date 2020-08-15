---
title: "Limit MS Teams Creation to Security Group"
date: "2020-07-15"
share: true
header:
  image: media/2020-07-24-limit-ms-teams-creation-to-security-group/01.png
  teaser: media/2020-07-24-limit-ms-teams-creation-to-security-group/01.png
categories:
  - MS Teams
tags:
  - "2020"
  - July 2020
last_modified_at: 2020-07-24T00:00:00-00:00
---

## Overview

Microsoft Teams is your hub for teamwork, which brings together everything a team needs including chat, threaded conversations, meetings, video conferencing, and many more. Employees get benefited by collaborating through MS Teams. However, over time there will be a huge number of teams. The obvious question will be: Does the organization needs those many teams? Is there any way to set up governance around this?

In this article, we will explore how we can govern MS Teams' creation to a certain security group.


## The Problem Statement

When the MS Teams license is enabled in Office 365, everyone within an organization can create a Team. Eventually, everyone will start creating Teams as they need and there will be lots of Teams. This uncontrolled growth of Teams will confuse employees instead of having any real benefit of Teams. In the near future this can lead to many issues:

- Duplicate Teams being created by multiple people for the same purpose.
- Clearly not defining when to have a Teams
- Clearly not defining when to create a Channel in Teams


## Prevent uncontrolled growth of MS Teams

One of the options is to disable the option of MS Teams creation for everyone and have some kind of centralized mechanism for Teams creation for certain users. The MS Teams desktop client or MS Teams admin center does not provide any option on UI for this.

One option is to restrict group creation, which will prohibit group creation in MS Teams. Please note this will be also applicable to other group connected services like Planner, SharePoint, Stream, etc.


### Create a Security Group

We will start by defining a security group for the users who need to create Microsoft 365 groups.

Follow the below steps to set up a security group:

1. Open **Microsoft 365 admin center**.
2. From the left menu, click **Groups**.
3. Click **Add a group** > **Security**.

    ![](/media/2020-07-24-limit-ms-teams-creation-to-security-group/01.png)

4. Click **Next**.
5. Set up the basics by providing the group name and option description.

    ![](/media/2020-07-24-limit-ms-teams-creation-to-security-group/02.png)

6. Click **Next**.
7. Click **Create group**.


### Execute PowerShell

Install the AzureADPreview module to change the group-level guest access setting.

```powershell
# Uninstall Azure AD PowerShell module (AzureAD), if installed already
Uninstall-Module AzureAD

# Install the latest version of AzureADPreview
Install-Module AzureADPreview
```

Run the below script

```powershell
$GroupName = "Allow Group Creation"
$AllowGroupCreation = "False"

Connect-AzureAD

$settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id
if (!$settingsObjectID) {
    $template = Get-AzureADDirectorySettingTemplate | Where-object {$_.displayname -eq "group.unified"}
    $settingsCopy = $template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $settingsCopy
    $settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id
}

$settingsCopy = Get-AzureADDirectorySetting -Id $settingsObjectID
$settingsCopy["EnableGroupCreation"] = $AllowGroupCreation

if ($GroupName) {
    $settingsCopy["GroupCreationAllowedGroupId"] = (Get-AzureADGroup -Filter "DisplayName eq '$GroupName'").objectId
}
else {
    $settingsCopy["GroupCreationAllowedGroupId"] = $GroupName
}

Set-AzureADDirectorySetting -Id $settingsObjectID -DirectorySetting $settingsCopy

(Get-AzureADDirectorySetting -Id $settingsObjectID).Values
```

The Teams creation now will be disabled to all users, except one in our security group.


### Turn off group creation restriction

To turn off group creation restriction and again allow all users to create groups, set ```$GroupName``` to ```""``` and ```$AllowGroupCreation``` to ```True``` and rerun the script.


## Summary

When the MS Teams license is enabled in Office 365, everyone within an organization can create a Team. We explored one option to restrict group creation, which will prohibit group creation in MS Teams. Since MS Teams desktop client or Microsoft 365 Admin center does not provide any option in UI for this, this can be achieved with PowerShell by installing the AzureADPreview module.


## References

- [Manage who can create Groups](https://docs.microsoft.com/en-us/microsoft-365/admin/create-groups/manage-creation-of-groups?view=o365-worldwide)
