---
title: "Office 365 CLI Overview"
date: "2018-11-06"
---

## Overview

Microsoft technology stack is well known to be supported on Windows operating system. PowerShell has been developer and administrators best friend in Office 365 to execute powerful commands and automate the process. However PowerShell is only supported on windows platform.

Office 365 CLI helps to manage the Office 365 tenant and SharePoint framework (SPFx) projects on any platform. It can work well on Windows, Linux, or macOS. Office 365 CLI is supported on PowerShell, Cmder, and Bash. Office 365 CLI also supports to build automation scripts.

In this article, we will see how to install Office 365 CLI and use it to manage Office 365 and SharePoint Framework projects.

## Installing Office 365 CLI

Office 365 CLI is available as NPM package.

Use below command to install Office 365 CLI globally:

npm i -g @pnp/office365-cli

To install the beta version of Office 365 CLI, use below command:

npm i -g @pnp/office365-cli@next

How to use Office 365 CLI

Once the installation is finished, we can start using Office 365 CLI by typing office365 on the command line:

office365

This will start the Office 365 CLI with its own command prompt:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-394.png)

## Managing Office 365 Tenant

1. To login to the Office 365 tenant, use below command.

o365$ spo login https://\[tenant\]-admin.sharepoint.com

Replace the token \[tenant\] in the above command with your Office 365 tenant name.

Depending upon which settings you would like to manage, specify tenant admin site (i.e. -admin) or regular SharePoint site.

1. The command will return a code to authenticate. The code is generated unique each time. Copy the code for future reference.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-395.png)

1. Open url ([https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)) in browser

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-396.png)

1. Type in the code

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-397.png)

1. Click Continue
2. Enter Office 365 credentials or select from list

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-398.png)

1. The app will require permission on Office 365 tenant.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-399.png)

1. Click Accept to grant the permissions
2. Once the permission is granted, you will be signed in to PnP Office 365 management shell application from your device.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-400.png)

1. You may close the browser window.
2. Return back to the command prompt.

## Office 365 CLI Prompt

To get list of all available commands, type below command.

o365$ help

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-401.png)

**AAD commands**

The aad\* commands helps to work with Azure AD.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-402.png)

**Azmgmt commands**

The azmgmt\* commands helps to work with Azure Management Service.![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-403.png)

**Graph commands**

The graph\* commands helps to work with Microsoft graph.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-404.png)

**Spfx commands**

SPFx commands helps to manage SharePoint Framework projects.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-405.png)

The only available command at the time of writing this article helps to upgrade the SharePoint Framework projects to the newer versions.

**SPO commands**

SPO (SharePoint Online) commands helps to manage SharePoint Online site.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-406.png)

To exit the CLI, type below command:

o365$ exit

Upgrading Office 365 CLI

As Office 365 CLI is available as NPM package, the bug fixes and newer functionalities are released as a new version of NPM package.

Run below command to see, if Office 365 CLI needs upgrade:

npm outdated --global

To update the Office 365 CLI, run below command:

npm update -g @pnp/office365-cli

Summary

Office 365 CLI helps to manage the Office 365 tenant and SharePoint framework (SPFx) projects on any platform. It can be used effectively to upgrade SPFx projects and build automation scripts.

This content was originally posted [here](https://www.c-sharpcorner.com/article/office-365-cli-overview/).
