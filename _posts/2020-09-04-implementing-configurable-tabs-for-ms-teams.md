---
title: "Implementing Configurable Tabs for MS Teams with Yo Teams"
date: "2020-09-04"
share: true
header:
  image: media/2020-09-04-implementing-configurable-tabs-for-ms-teams/07.png
  teaser: media/2020-09-04-implementing-configurable-tabs-for-ms-teams/07.png
categories:
  - MS Teams
tags:
  - "2020"
  - September 2020
last_modified_at: 2020-09-04T00:00:00-00:00
---

## Overview

Custom tabs can be used with the app to embed our own content in MS Teams. MS Teams supports 2 types of tabs.

- Channel/group tab: delivers content to channels and group chats.
- Personal tab: scoped to a single user.

In this article, we will explore web experiences with custom configurable tabs in Microsoft Teams using Yo Teams.


## Configurable Tabs overview

Tabs are Teams-aware webpages embedded in Microsoft Teams. Configurable tabs give you the opportunity to allow the user to specify some configuration information before the tab is added.


## Prerequisites

Install below to get started:

- [Node.js](https://nodejs.org/) - v10.* (or higher)
- NPM (installed with Node.js) - v6.* (or higher)
- [Gulp](https://gulpjs.com/) - v4.* (or higher)
- [Yeoman](https://yeoman.io/) - v3.* (or higher)
- [Yeoman Generator for Microsoft Teams](https://github.com/OfficeDev/generator-teams) - v2.13.0 (or higher)
- [Visual Studio Code](https://code.visualstudio.com/)

Install Yeoman, Gulp global command-line interface, and Typescript compiler globally using NPM.

```
npm install yo gulp-cli typescript --global
```

Install generator-teams globally using NPM.

```
npm install generator-teams --global
```

To install preview versions of the generator, run below command.

```
npm install generator-teams@preview --global
```


## Implement Configurable Tabs with Yo Teams

We will generate a solution using Yo Teams.

1. On the command prompt, type below command.

    ```
    yo teams
    ```

2. Follow the wizard to set up the solution.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/01.png)

3. On the command prompt, type code . to open the solution in visual studio code.
4. If you want to test the configurable app locally, on the command prompt type ```gulp serve``` and the information will be displayed from the file "src\app\web\index.html".
5. When we are running the configurable app from within MS Teams, the information will be rendered from "src\app\scripts\configurableTab\ConfigurableTab.tsx".


## Test the solution

Follow the below steps to test the configurable tab locally with ngrok.

1. On the command prompt, run below command.

    ```
    gulp ngrok-serve
    ```

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/02.png)

2. Open MS Teams.
3. Under **Apps**, click **Upload a custom app**.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/03.png)

4. Navigate to any Team.
5. Click **Add a tab**.
6. Search and add **ConfigurableTab**.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/04.png)

7. Click **Add**.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/05.png)

8. Specify the title, click **Save**.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/06.png)

9. Once added, the configurable tab looks as below.

    ![](/media/2020-09-04-implementing-configurable-tabs-for-ms-teams/07.png)


## Summary

Tabs are Teams-aware webpages embedded in Microsoft Teams. Configurable tabs give you the opportunity to allow the user to specify some configuration information before the tab is added. ```Yo Teams``` helps to build configurable apps with ease.


## References

- [Extend your Teams app with a custom tab](https://docs.microsoft.com/en-us/microsoftteams/platform/tabs/how-to/add-tab)
- [Yo Teams](https://docs.microsoft.com/en-us/microsoftteams/platform/tutorials/get-started-yeoman)


## Code Download

The code developed during this article can be found at: [https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/configurable-tab](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/configurable-tab)
