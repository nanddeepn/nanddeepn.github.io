---
title: "SharePoint Framework - Develop First Client Side Web Part"
date: "2018-07-20"
---

## Overview

Client Side WebParts developed using SharePoint Framework (SPFx) are future of Modern SharePoint. Client Side WebParts are developed using modern UI standards, modern JavaScript tools and libraries. They run inside context of SharePoint page and are responsive in nature.

Features of SPFx Client Side Web Parts:

- Lightweight - Developed using JavaScript libraries and HTML
- Responsive - Renders on any device
- Environments - Work on both SharePoint Online and OnPremise (SharePoint 2016 onwards)

In this article, we will learn how to build a simple SPFx based Client Side WebPart.

## Create SPFx Solution

1. Create a directory for SPFx solution.

md firstspfx-webpart

1. Navigate to above created directory.

cd firstspfx-webpart

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-16.png)

- **Solution Name:** Hit enter to have default name (firstspfx-webpart in this case) or type in any other name for your solution.
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
    - Selected choice: Hit enter (default name)
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Hit enter (default description)
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Yeoman generator will start the scaffolding process to create the solution and will install required dependencies. This process will take significant amount of time.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-17.png)

1. Once scaffolding is completed, Yeoman will show the below message

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-18.png)

## Open Solution in Code Editor

Any of below code editor can be used to open the SPFx client side solution.

- Visual Studio Code ([https://code.visualstudio.com/](https://code.visualstudio.com/))
- Atom ([https://atom.io/](https://atom.io/))
- Webstorm ([https://www.jetbrains.com/webstorm](https://www.jetbrains.com/webstorm))

Install any of above code editor. Being in the solution folder in command prompt, type below command to open the solution in code editor.

code .

Run WebPart on local server

As client side toolchain uses HTTPS endpoint, we need to install the developer certificate that comes with SPFx toolchain.

To install developer certificate, run below command from the command prompt.

gulp trust-dev-cert

Run below command to preview the web part.

gulp serve

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-19.png)

This command runs series of gulp tasks that performs below:

- Minify and bundle JavaScript and CSS files.
- Compile SASS (Syntactically Awesome Style Sheets) to CSS.
- Compile TypeScript to JavaScript.

Upon successful compilation and running the gulp tasks, it will open up SharePoint local workbench.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-20.png)

Click Add (+) icon. Select the web part to add it on page.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-21.png)

Congratulations! The first client side web part is ready.

Click Edit icon, modify description from properties pane and it will be reflected on client side web part.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-22.png)

## SharePoint Local Workbench

SharePoint Local Workbench is HTML page which helps to preview and test client side web parts without deploying to SharePoint. It gets served locally usually on url - [https://localhost:4321/temp/workbench.html](https://localhost:4321/temp/workbench.html)

## Summary

SPFx client side web parts are based on modern JavaScript libraries, which can be created using Yeoman generator. We can choose the JavaScript library (No JavaScript Framework, React, and Knockout) to build the client side web part. They can be tested locally on SharePoint Workbench.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-develop-first-client-side-web-part/).
