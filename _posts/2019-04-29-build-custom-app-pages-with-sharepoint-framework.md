---
title: "Build Custom App Pages with SharePoint Framework"
date: "2019-04-29"
---

## Overview

Single part app pages are recently introduced in Modern SharePoint. We can extend out of box page templates by creating our own templates, however they are limited to only that site. To use the page templates all across the tenant, we will have to create custom app pages using SharePoint framework. We can expose app pages by using new page template selection with SPFx web part on it.

In this article, we will explore how we can create, deploy and use custom app pages built using SharePoint Framework.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md single-part-app-page

1. Navigate to above created directory.

cd single-part-app-page

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-372.png)

- **Solution Name:** Hit enter to have default name (single-part-app-page in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 or 2019 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Use the current folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: Y
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension.
    - Selected choice: WebPart, since single part app pages are web parts.
- **Web Part Name:** Hit enter to select the default name or type in any other name.
    - Selected choice: SinglePartPage
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Hit enter
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. In the command prompt type below command to open the solution in code editor of your choice.

code .

Web Part Configuration

In order to expose this web part as a single part app page, we will have to make below configurations.

1. Open SinglePartPageWebPart.manifest.json file under “\\src\\webparts\\singlePartPage”.
2. In the supportedHosts, add option SharePointFullPage.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-373.png)

We will go ahead with the default implementation of custom app page, without any customization to see how it looks out of the box.

## Deploy the Package

**Bundle the solution**

On the command prompt, type below command to bundle the solution.

gulp bundle --ship

**Package the solution**

On the command prompt, type below command to package the solution.

gulp package-solution --ship

Locate the single-part-app-page.sppkg file at “\\single-part-app-page\\sharepoint\\solution”.

**Add Package to App Catalog**

1. Open SharePoint Online App Catalog site.
2. Add single-part-app-page.sppkg to app catalog.
3. Select “Make this solution available to all sites in the organization”, as this custom app page should be available across the tenant.
4. Click Deploy.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-374.png)

## Test the Custom App Page

1. Open Modern SharePoint site.
2. Click New > Page.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-375.png)

1. The out of box page templates are displayed.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-376.png)

1. Click Apps tab. The Apps tab is only displayed when custom app pages are deployed to tenant.
2. Click our custom app, SinglePartPage. It displays the preview on right side.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-377.png)

1. Click Create page.
2. Specify the page title.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-378.png)

Our custom app page is now ready with our SPFx web part added to it.

## Summary

We can extend out of box page templates by creating our own templates, however they are limited to only that site. To use the page templates all across the tenant, we will have to create custom app pages using SharePoint framework.

This content was originally posted [here](https://www.c-sharpcorner.com/article/build-custom-app-pages-with-sharepoint-framework/).
