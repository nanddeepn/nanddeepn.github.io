---
title: "SharePoint Framework - Test SPFx WebParts on Modern Pages"
date: "2018-12-20"
---

## Overview

SharePoint Framework client web parts works well on both classic and modern SharePoint. SharePoint provides workbench to test the web parts individually. However the web part needs to be added to actual SharePoint page to see how it looks and fits the design. Adding SPFx web part on actual page will help us better judge the entire look and feel of the page by taking the decision on its position, layout and overall composition of page.

In this article, we will explore how we can test the SharePoint framework web parts on a modern page instead of testing them on workbench.

## Workbench

SharePoint workbench is developer design surface which helps developer to preview and test the web part in isolation without deploying it to actual SharePoint site. SharePoint offers two types of workbenches.

**Local Workbench**

Local workbench is html based page which mimics SharePoint UI. It starts after running the “gulp serve” command and usually opens at url [https://localhost:4321/temp/workbench.html](https://localhost:4321/temp/workbench.html)

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-289.png)

**SharePoint workbench**

SharePoint workbench can be accessed from SharePoint site by navigating to “/\_layouts/15/workbench.aspx”. The gulp can be run with command “gulp serve --nobrowser”

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-290.png)

## Develop SharePoint Framework Web Part

1. Open command prompt. Create a directory for SPFx solution.

md spfx-hello-world

1. Navigate to above created directory.

cd spfx-hello-world

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-291.png)

- **Solution Name:** Hit enter to have default name (spfx-hello-world in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Same folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: N (install on each site explicitly)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: HelloWorld
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Test SPFx on Modern Pages
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

Configure SPFx web parts for Modern Pages

1. On the command prompt, run below command.

gulp serve

1. Or run below command, without opening browser instance.

gulp serve --nobrowser

1. In the browser, navigate to your SharePoint page. Append below text to url in browser.

?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js

1. Click “Load debug scripts”, when prompted.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-292.png)

1. Edit the page, click Add icon to add web part

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-293.png)

1. Our SPFx web part is available in the list of web parts to add.

The web part can be added to page and can be tested along with other content on the page.

## Summary

SharePoint workbench helps to preview and test the web parts in isolation. However using above mentioned trick, the web part can be test on the page itself along with the other content. This will help to visualize the final content with SPFx web part on the page.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-test-spfx-webparts-on-modern-pages/).
