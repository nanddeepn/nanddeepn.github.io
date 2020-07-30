---
title: "Getting Started with MS Teams Development"
date: "2019-09-10"
share: true
categories:
  - MS Teams
header:
  image: media/2019-09-10-getting-started-with-ms-teams-development/03.png
  teaser: media/2019-09-10-getting-started-with-ms-teams-development/03.png
tags:
  - "2019"
  - September 2019
last_modified_at: 2019-09-10T00:00:00-00:00
---

## Overview

In a nutshell, MS Teams is a chat-based workspace in Office 365. It goes beyond that and provides integration with various apps and services, which helps users to get their work done. To start developing apps for MS Teams involves preparation of both Office 365 tenant and developer workspace.

In this article, we will prepare Office 365 tenant and our development environment to get started with MS Teams development.


## Prepare Office 365 Tenant for MS Teams

Any of below Office 365 plan is a pre-requisite to start developing apps for MS Teams.

- Developer
- Enterprise E1, E3, and E5
- Education, Education Plus, and Education E5
- Business Essentials
- Business Premium


### Turn on MS Teams on Office 365 Tenant

By default, MS Teams is enabled in the Office 365 Tenants, which supports it. However, follow the below steps to verify the MS Teams enablement. The below instructions are applicable for the new admin center experience.

1. Login to Office 365 portal ([http://portal.office.com](http://portal.office.com/)) as a global administrator.
2. From the left menu, under Settings, click **Services & add-ins**.
3. From the list of Services & add-ins, click **Microsoft Teams**.

    ![](/media/2019-09-10-getting-started-with-ms-teams-development/01.png)

4. The Microsoft Teams settings screen will open.
5. Under **Settings by user/license type** section, verify that **Microsoft Teams** is turned **on**.

    ![](/media/2019-09-10-getting-started-with-ms-teams-development/02.png)

6. Click **Save**.

The apps are now enabled in MS Teams to upload and test. The new MS Teams admin center can be accessed from [https://admin.teams.microsoft.com](https://admin.teams.microsoft.com/)


## MS Azure Subscription

If you are planning to develop Bot for MS Teams, then the Azure Bot service requires MS Azure subscription. If you do not have the MS Azure subscription, you can also use the legacy portal to register the Bot at [https://dev.botframework.com/bots/new](https://dev.botframework.com/bots/new). It accepts a work, school account, or Microsoft account.


## Setup Development Environment

To develop MS Teams app, we need to install below developer tools to our development environment.

### ngrok

MS Teams is a cloud based product. It requires all the services it accesses to be available in the cloud over HTTPS endpoints. While developing the apps locally, we need a tunneling application to work within MS Teams. The ngrok provides tunneling HTTPS endpoints to a web server running locally on a developer workstation. Ngrok is a command line application. It can be downloaded from [https://ngrok.com/](https://ngrok.com/)


### Visual Studio Bot Template

The Microsoft Bot Framework V4 Templates for Visual Studio can be downloaded from here ([https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4)). It is available as a VSIX package. It helps to build new conversational AI bots using the Microsoft Bot Framework v4. Both V3 and V4 bots built on the Bot Framework work well with MS teams.

Once installed, the Bot Framework project creation option will be available for new project creation from Visual Studio.

![](/media/2019-09-10-getting-started-with-ms-teams-development/03.png)


## Summary

MS Teams provides integration with various apps and services. Developing apps for MS Teams involves preparation of both Office 365 tenant and developer workspace. This article should help to lay down the foundation for MS Teams development.

This content was originally posted [here](https://www.c-sharpcorner.com/article/getting-started-with-ms-teams-development/).
