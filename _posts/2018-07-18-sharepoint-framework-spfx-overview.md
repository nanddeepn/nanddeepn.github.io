---
title: "SharePoint Framework (SPFx) Overview"
date: "2018-07-18"
---

## Overview

SharePoint Online has started moving to modern experience developed using HTML, CSS, and JavaScript which reduces the page load time compared to the postback model from classic SharePoint based on ASP .Net.

The customizations in modern sites are supported using SharePoint Framework (SPFx). SPFx is an open and connected platform. SPFx is a page and web part model. It can be entirely developed using client-side languages and open source tooling. SPFx provides easy integration with SharePoint data.

## Key Features of SharePoint Framework

1. No iFrames. Runs within the context of the user browser and connection in the browser.
2. Faster rendering on the browser as all controls all rendered in normal DOM.
3. Controls are responsive.
4. Runs in the context of the current user.
5. Gives controls to access the life-cycle of the SharePoint Framework web part (component) (Init, render, load, serialize, de-serialize, configuration changes and many more).
6. No dependency on underlying Framework. You can use any framework like React, Angular, Knockout and more.
7. Open source Development tools are used (npm, TypeScript, Yeoman, webpack, and Gulp).
8. Can be added on both classic pages and modern pages.
9. Safe and Secure, need tenant access to deploy/make changes to the SPFx web part.
10. Controlled visibility, we can decide who can view this web part in the App Catalog of the site contents.
11. You can leverage your earlier knowledge of CSOM, as the data models are not changed and are completely transferable.
12. SPFx web parts can be used with classic or modern sites in SharePoint
13. Supports mobile views of SharePoint Online sites.

 

## Script Editor WebParts vs App Parts vs SPFx WebParts

**Script Editor WebParts**

1. The obvious choice of developers for customizing DOM on classic SharePoint sites.
2. The script can be edited by any users easily.
3. Cannot be added to “NoScript” sites.

**App Parts**

1. Developed using Add-in model.
2. Uses iframe.
3. Cannot access DOM of SharePoint page.
4. Development and deployment is a bit complicated.

**SPFx WebParts**

1. Client-side web parts leverages modern JavaScript frameworks.
2. Can be used with classic SharePoint pages.
3. Provides modern experience, responsiveness out of the box.
4. A free, open-source tool-chain.

 

## Light weight Components / Tools Used

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image.png)

1. **Node.js**
    1. Open-source JavaScript run-time
    2. Used to build and run the applications which are equivalent to the .Net
    3. SPFx supports the latest LTS (Long Term Support) version
2. **NPM Packages**
    1. Stands for Node Package Manager
    2. Installs modules and its dependencies (equivalent to Nuget Package)
    3. Packages can be installed globally (-g switch) or locally
    4. Installed packages go inside node\_modules folder
3. **Gulp**
    1. Automates SPFx development and deployment tasks
    2. Bundle and minify JavaScript and CSS files
    3. Run tools to call the bundling and minification tasks before each build
    4. Compiles LESS or SASS files to CSS
    5. Compiles TypeScript files to JavaScript
    6. Equivalent to MSBuild in Microsoft World
    7. Compiles, bundle and copies files to deployment folder for packaging
4. **Yeoman**
    1. Relies on NPM and Gulp
    2. Scaffolding tool for Modern web apps
    3. Used as SPFx solution generator and builds the required project structure
    4. ‘yo’ is the command-line utility for creation of projects
5. **TypeScript**
    1. Strongly typed language
    2. Adds compile-time syntax and type checking for JavaScript
    3. Help to build the Applications which will be then compiled to clean JS code
6. **Visual Studio Code**
    1. Interface for working with SPFx solutions
    2. Fast and lightweight IDE
    3. Can work on Windows, Mac OS, and Linux

 

## The flow of Client-Side Web Part

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-1.png)

Follow below set of commands to get your developer environment ready for SPFx:

**Install NodeJS**

- Install latest LTS version from [https://nodejs.org](https://nodejs.org)
- If you already have NodeJS installed, check the version

node -v

**Install Code Editor**

Install any of below code editor:

- Visual Studio Code ([https://code/visualstudio.com](https://code/visualstudio.com))
- Atom ([https://atom.io](https://atom.io))
- Webstorm ([https://www.jetbrains.com/webstorm](https://www.jetbrains.com/webstorm))
- Visual Studio SPFx Project Template ([https://marketplace.visualstudio.com/items?itemName=SharePointPnP.SPFxProjectTemplate](https://marketplace.visualstudio.com/items?itemName=SharePointPnP.SPFxProjectTemplate))

**Install Yeoman and gulp**

Run below command:

npm install -g yo gulp

**Install Yeoman SharePoint Generator**

Run below command:

npm install -g @microsoft/generator-sharepoint

**Note:**

> You may create a batch file to install the same packages (with a specific version) to multiple developer machines.

## Updating NPM packages

Yo, Gulp, Yeoman SharePoint Generator gets installed as NPM packages. Use the below commands to check and update them.

To check outdated packages:

npm outdated --global

This command will report packages that need an update. Use below command to update the package.

npm update -g <package-name>

 

Summary

In this article, we had a walk-through on a high level understanding of the SharePoint Framework. We also discussed the key features of the SharePoint Framework. We also discussed the tool comparison which will be used in SharePoint Framework development along with that we discussed the high level flow of Client-Side web parts development.

This content was originally posted [here](https://www.c-sharpcorner.com/article/spfx-part-1-sharepoint-framework-spfx-overview/).
