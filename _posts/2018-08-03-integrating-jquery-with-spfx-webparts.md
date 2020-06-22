---
title: "SharePoint Framework - Integrating JQuery with SPFx WebParts"
date: "2018-08-03"
---

## Overview

SharePoint Framework Client side webparts are developed using JavaScript framework. JQuery is one of the largely used JavaScript framework.

In this article, we will explore how to integrate JQuery with SPFx web parts.

## Create SPFx Solution

1. Create a directory for SPFx solution.

md spfx-jqueryintegration

1. Navigate to above created directory.

cd spfx-jqueryintegration

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-73.png)

- **Solution Name:** Hit enter to have default name (spfx-jqueryintegration in this case) or type in any other name for your solution.
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
    - Selected choice: SPFxJqueryIntegration
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Hit enter (default description)
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework. We will integrate JQuery to this solution.

## Configured Required Packages and Dependencies

**Include Packages**

We will now include the JQuery packages. Type in below commands on command prompt.

npm i jquery jqueryui combokeys --save

The --save option enables NPM to include the packages to dependencies section of package.json file.

 

**Include Typings**

Typings will help for auto complete while writing the code in code editor. Type below command.

tsd install jquery jqueryui combokeys --save

 

**Lock down the package dependencies**

Type in below command to lock down the package dependencies.

npm shrinkwrap

 

Solution Changes

1. In the command prompt, type below command to open the solution in code editor of your choice.

code .

1. Expand node\_module folder to see the npm packages being added for jQuery.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-74.png)

1. Open config.json under config folder.
2. Under externals node, add jQuery references.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-75.png)

1. Open package.json and verify jQuery dependencies are listed.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-76.png)

1. Open webpart file SpFxJQueryIntegrationWebPart.ts and import jQuery.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-77.png)

1. Define a constructor and load external jQuery UI css from it.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-78.png)

1. Right click on spFxJQueryIntegration folder, click New file

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-79.png)

1. Name the file as AccordianTemplate.ts and add below content.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-80.png)

1. Use the accordion template in main webpart class.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-81.png)

## Test the web part

1. On command prompt, type "gulp serve".
2. Open any SharePoint site in your tenant or use SharePoint local workbench.
3. Add the App to your site from “Add an App” menu.
4. Edit any page and add the web part.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-82.png)

1. See the web part working.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-83.png)

## Summary

JavaScript frameworks like JQuery can be easily integrated with SPFx client web parts. They also supports typings which helps for intellisense while developing the code in editors.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-integrating-jquery-with-spfx-webparts/).
