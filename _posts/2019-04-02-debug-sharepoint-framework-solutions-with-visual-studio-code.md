---
title: "Debug SharePoint Framework Solutions with Visual Studio Code"
date: "2019-04-02"
---

## Overview

Debugging is an integral part of software developer’s life. It helps to analyze the code scenarios during run time. In the previous article, we explored debugging SPFx solutions from browser.

In this article, we will explore how we can debug the SPFx solutions with Visual Studio code.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-debug-vscode

1. Navigate to above created directory.

cd spfx-debug-vscode

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-333.png)

- **Solution Name:** Hit enter to have default name (spfx-debug-vscode in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the current folder or create a subfolder for our solution.
    - Selected choice: Use the current folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: Y
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: SPFxDebug
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Debug SPFx with VS Code
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

 

Get the Debugger Extension

1. In visual studio code, click Extensions from left menu.
2. Search for debugger.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-334.png)

1. Select and install “Debugger for Chrome”, if not installed already.
2. Open launch.json under “.vscode” folder. This file contains configurations for local workbench and hosted workbench.

{  
  /\*\* 
   \* Install Chrome Debugger Extension for Visual Studio Code to debug your components with the 
   \* Chrome browser: https://aka.ms/spfx-debugger-extensions 
   \*/  
  "version": "0.2.0",  
  "configurations": \[{  
      "name": "Local workbench",  
      "type": "chrome",  
      "request": "launch",  
      "url": "https://localhost:4321/temp/workbench.html",  
      "webRoot": "${workspaceRoot}",  
      "sourceMaps": true,  
      "sourceMapPathOverrides": {  
        "webpack:///.././src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../../src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../../../src/\*": "${webRoot}/src/\*"  
      },  
      "runtimeArgs": \[  
        "--remote-debugging-port=9222"  
      \]  
    },  
    {  
      "name": "Hosted workbench",  
      "type": "chrome",  
      "request": "launch",  
      "url": "https://enter-your-SharePoint-site/\_layouts/workbench.aspx",  
      "webRoot": "${workspaceRoot}",  
      "sourceMaps": true,  
      "sourceMapPathOverrides": {  
        "webpack:///.././src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../../src/\*": "${webRoot}/src/\*",  
        "webpack:///../../../../../src/\*": "${webRoot}/src/\*"  
      },  
      "runtimeArgs": \[  
        "--remote-debugging-port=9222",  
        "-incognito"  
      \]  
    }  
  \]  
}

 

Debugging on Local Workbench

1. Open the main webpart file (src\\webparts\\spFxDebug\\SpFxDebugWebPart.ts) and set debug point to render method.
2. Click View > Terminal to open the terminal window.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-335.png)

1. On the terminal window type below command.

gulp serve --nobrowser

Now that we are ready to debug our code in VS Code. Use any of below option to start debugging.

1. On keyboard press F5
2. From main menu, click Debug > Start Debugging
3. From left menu, click Debug and start debugging by clicking green arrow.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-336.png)

## Debugging on Hosted Workbench

Follow below steps to debug on real SharePoint site.

1. From VS Code open launch.json under .vscode folder.
2. In the Hosted workbench configuration, type your SharePoint online site address for url property.
3. Optionally remove -incognito from runtimeArgs.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-337.png)

1. From debug menu, select Hosted workbench.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-338.png)

## Summary

SPFx solutions can be easily debugged in VS Code using debugger extensions. This gives an comfort of developing, testing and debugging the SPFx solution from same IDE.

This content was originally posted [here](https://www.c-sharpcorner.com/article/using-react-components-in-spfx-extension-application-customizer/).
