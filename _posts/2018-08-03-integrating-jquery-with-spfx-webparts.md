---
title: "SharePoint Framework - Integrating JQuery with SPFx WebParts"
date: "2018-08-03"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-03-integrating-jquery-with-spfx-webparts/11.png
  teaser: media/2018-08-03-integrating-jquery-with-spfx-webparts/11.png
tags:
  - "2018"
  - August 2018
last_modified_at: 2018-08-03T00:00:00-00:00
---

## Overview

SharePoint Framework Client side webparts are developed using JavaScript framework. JQuery is one of the largely used JavaScript framework.

In this article, we will explore how to integrate JQuery with SPFx web parts.


## Create SPFx Solution

1. Create a directory for SPFx solution.

    ```
    md spfx-jqueryintegration
    ```

2. Navigate to above created directory.

    ```
    cd spfx-jqueryintegration
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/01.png)

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

```
npm i jquery jqueryui combokeys --save
```

The ```--save``` option enables NPM to include the packages to dependencies section of package.json file.


**Include Typings**

Typings will help for auto complete while writing the code in code editor. Type below command.

```
tsd install jquery jqueryui combokeys --save
```


**Lock down the package dependencies**

Type in below command to lock down the package dependencies.

```
npm shrinkwrap
```


## Solution Changes

1. In the command prompt, type below command to open the solution in code editor of your choice.

    ```
    code .
    ```

2. Expand node_modules folder to see the npm packages being added for jQuery.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/02.png)

3. Open config.json under config folder.
4. Under externals node, add jQuery references.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/03.png)

5. Open package.json and verify jQuery dependencies are listed.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/04.png)

6. Open webpart file SpFxJQueryIntegrationWebPart.ts and import jQuery.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/05.png)

7. Define a constructor and load external jQuery UI css from it.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/06.png)

8. Right click on spFxJQueryIntegration folder, click New file

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/07.png)

9. Name the file as AccordianTemplate.ts and add below content.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/08.png)

9. Use the accordion template in main webpart class.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/09.png)


## Test the web part

1. On command prompt, type ```gulp serve```.
2. Open any SharePoint site in your tenant or use SharePoint local workbench.
3. Add the App to your site from **Add an App** menu.
4. Edit any page and add the web part.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/10.png)

5. See the web part working.

    ![](/media/2018-08-03-integrating-jquery-with-spfx-webparts/11.png)


## Summary

JavaScript frameworks like JQuery can be easily integrated with SPFx client web parts. They also supports typings which helps for intellisense while developing the code in editors.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-integrating-jquery-with-spfx-webparts/).
