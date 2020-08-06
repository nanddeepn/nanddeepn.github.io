---
title: "Office 365 CLI Overview"
date: "2018-11-06"
share: true
categories:
  - Office 365 CLI
header:
  image: media/2018-11-06-office-365-cli-overview/02.png
  teaser: media/2018-11-06-office-365-cli-overview/02.png
tags:
  - "2018"
  - November 2018
last_modified_at: 2018-11-06T00:00:00-00:00
---

## Overview

Microsoft technology stack is well known to be supported on Windows operating system. PowerShell has been developer and administrators best friend in Office 365 to execute powerful commands and automate the process. However PowerShell is only supported on windows platform.

Office 365 CLI helps to manage the Office 365 tenant and SharePoint framework (SPFx) projects on any platform. It can work well on Windows, Linux, or macOS. Office 365 CLI is supported on PowerShell, Cmder, and Bash. Office 365 CLI also supports to build automation scripts.

In this article, we will see how to install Office 365 CLI and use it to manage Office 365 and SharePoint Framework projects.


## Installing Office 365 CLI

Office 365 CLI is available as NPM package.

Use below command to install Office 365 CLI globally:

```
npm i -g @pnp/office365-cli
```

To install the beta version of Office 365 CLI, use below command:

```
npm i -g @pnp/office365-cli@next
```


## How to use Office 365 CLI

Once the installation is finished, we can start using Office 365 CLI by typing office365 on the command line:

```
office365
```

This will start the Office 365 CLI with its own command prompt:

![](/media/2018-11-06-office-365-cli-overview/01.png)


## Managing Office 365 Tenant

1. To login to the Office 365 tenant, use below command.

    ```
    o365$ spo login https://[tenant]-admin.sharepoint.com
    ```

    Replace the token ```[tenant]``` in the above command with your Office 365 tenant name.

    Depending upon which settings you would like to manage, specify tenant admin site (i.e. -admin) or regular SharePoint site.

2. The command will return a code to authenticate. The code is generated unique each time. Copy the code for future reference.

    ![](/media/2018-11-06-office-365-cli-overview/02.png)

3. Open url ([https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)) in the browser.

    ![](/media/2018-11-06-office-365-cli-overview/03.png)

4. Type in the code.

    ![](/media/2018-11-06-office-365-cli-overview/04.png)

5. Click Continue.
6. Enter Office 365 credentials or select from list.

    ![](/media/2018-11-06-office-365-cli-overview/05.png)

7. The app will require permission on Office 365 tenant.

    ![](/media/2018-11-06-office-365-cli-overview/06.png)

8. Click Accept to grant the permissions.
9. Once the permission is granted, you will be signed in to PnP Office 365 management shell application from your device.

    ![](/media/2018-11-06-office-365-cli-overview/07.png)

10. You may close the browser window.
11. Return back to the command prompt.


## Office 365 CLI Prompt

To get list of all available commands, type below command.

```
o365$ help
```

![](/media/2018-11-06-office-365-cli-overview/08.png)


**AAD commands**

The aad* commands helps to work with Azure AD.

![](/media/2018-11-06-office-365-cli-overview/09.png)


**Azmgmt commands**

The azmgmt* commands helps to work with Azure Management Service.

![](/media/2018-11-06-office-365-cli-overview/10.png)


**Graph commands**

The graph* commands helps to work with Microsoft graph.

![](/media/2018-11-06-office-365-cli-overview/11.png)


**SPFx commands**

The SPFx* commands helps to manage SharePoint Framework projects.

![](/media/2018-11-06-office-365-cli-overview/12.png)

The only available command at the time of writing this article helps to upgrade the SharePoint Framework projects to the newer versions.


**SPO commands**

The SPO* (SharePoint Online) commands helps to manage SharePoint Online site.

![](/media/2018-11-06-office-365-cli-overview/13.png)

To exit the CLI, type below command:

```
o365$ exit
```

## Upgrading Office 365 CLI

As Office 365 CLI is available as NPM package, the bug fixes and newer functionalities are released as a new version of NPM package.

Run below command to see, if Office 365 CLI needs upgrade:

```
npm outdated --global
```

To update the Office 365 CLI, run below command:

```
npm update -g @pnp/office365-cli
```


## Summary

Office 365 CLI helps to manage the Office 365 tenant and SharePoint framework (SPFx) projects on any platform. It can be used effectively to upgrade SPFx projects and build automation scripts.

This content was originally posted [here](https://www.c-sharpcorner.com/article/office-365-cli-overview/).
