---
title: "SharePoint Framework - SPFx Development with SharePoint 2016"
date: "2018-12-18"
---

## Overview

SharePoint Framework (SPFx) is popular for developing the solutions for modern sites in SharePoint Online. However it is not only limited to the cloud environment. SharePoint Framework development is also supported on SharePoint OnPremise environment (SharePoint 2016 onwards).

In this article, we will explore how we can get our SharePoint 2016 OnPremise environment ready for SharePoint Framework and start developing SPFx client web parts.

## Decide Upon SharePoint Framework Version

SharePoint Online follows frequent release cycles as compared to SharePoint OnPremise versions. SharePoint online always uses latest version of SharePoint Framework. On the other hand, SharePoint 2016 only supports the version with matching server side dependencies.

## Get SharePoint 2016 OnPremise Ready for SPFx

**Install SharePoint 2016 Feature Pack 2**

In order to get the SharePoint 2016 OnPremise ready for SharePoint Framework development, we need to first install “SharePoint 2016 Feature Pack 2” on the server. If your SharePoint server have working internet connection and automatic windows updates on then this should get applied to your server as a part of windows update.

**Install NodeJS**

- Install latest LTS version from [https://nodejs.org](https://nodejs.org)
- If you already have NodeJS installed, check the version

node -v

**Install Code Editor**

Install any of below code editor

- Visual Studio Code ([https://code/visualstudio.com](https://code/visualstudio.com))
- Atom ([https://atom.io](https://atom.io))
- Webstorm ([https://www.jetbrains.com/webstorm](https://www.jetbrains.com/webstorm))

**Install Yeoman and gulp**

Run below command:

npm install -g yo gulp

**Install Yeoman SharePoint Generator**

Run below command:

npm install -g @microsoft/generator-sharepoint

Develop SharePoint Framework Web Part

1. Open command prompt. Create a directory for SPFx solution.

md spfx-onprem-hello-world

1. Navigate to above created directory.

cd spfx-onprem-hello-world

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-286.png)

- **Solution Name:** Hit enter to have default name (spfx-onprem-hello-world in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online only (latest) / SharePoint 2016 onwards, including 2019 and SharePoint Online / SharePoint 2019 onwards, including SharePoint Online.
    - Selected choice: SharePoint 2016 onwards, including 2019 and SharePoint Online
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Same folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: N (install on each site explicitly)
- **Type of client-side component to create:** We can choose to create client side webpart.
    - Selected choice: WebPart
    - Unless SharePoint Online, we do not have an option to create SharePoint extensions in SharePoint OnPremise 2016 version using SPFx generator.
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: OnPremHelloWorld
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: SPFx on SharePoint 2016
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

Run the SPFx WebPart

1. On the command prompt, type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-287.png)

## Troubleshooting

**Workbench does not load**

Although you have everything installed and correctly setup, you might face below issues on “gulp serve”

_Internet Explorer_

![](https://nanddeepnachanblogs.com/wp-content/uploads/2018/12/IE-Workbench-does-not-load.png)

_Google Chrome_

![](https://nanddeepnachanblogs.com/wp-content/uploads/2018/12/Chrome-Workbench-does-not-load.png)

In the command prompt, run below command to resolve the issue:

set NODE\_NO\_HTTP2=1

**“gulp serve” is not running**

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-288.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

## Summary

For the future compatibility of solution, it is recommended to start using SharePoint Framework based development in SharePoint OnPremise. Also one thing to note that SPFx solutions created for SharePoint Online might not work for SharePoint OnPremise.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-spfx-development-with-sharepoint-2016/).
