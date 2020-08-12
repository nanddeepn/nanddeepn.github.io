---
title: "SharePoint Framework - Deploy SPFx WebParts to SharePoint Library"
date: "2018-08-01"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/03.png
  teaser: media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/03.png
tags:
  - "2018"
  - August 2018
last_modified_at: 2018-08-01T00:00:00-00:00
---

## Overview

Please refer my previous article [Develop First Client Side Web Part](/posts/2018-07-20-sharepoint-framework-develop-first-client-side-web-part/) to get started, develop first SPFx client side web part and test it on a local SharePoint workbench.


In the production ready scenario, the web parts can be deployed to any of below:

1. [Azure CDN](/posts/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/)
2. [Office 365 Public CDN](/posts/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/)
3. SharePoint Library in your tenant (This article)

In this article, we will explore the option of deploying the SPFx web parts using SharePoint library.


## Deployment to CDN

The build output of SPFx client side web part consists of solution package (.sppkg file) and various scripts, css and other assets. The sppkg file is deployed to SharePoint which includes manifest with url pointing to location where script files are deployed. The script files should exists at the location specified in manifest file, otherwise the web part will not work in SharePoint.

These files usually gets deployed to a CDN location. CDN location could be Azure BLOB storage or Office 365 public CDN. We can use any of this CDN location, however using CDN is not must. SharePoint Framework does not impose any restrictions that scripts should be deployed to any CDN. You can upload the scripts anywhere from where it can be accessed. That means, you can even upload the scripts to a SharePoint library inside your tenant to which everyone has an access. SharePoint Framework client webpart can access the scripts from SharePoint library.

In short, scripts can be deployed to any location including Azure BLOB storage, Office 365 CDN, any document library in SharePoint or even a physical web server from where it can be accessed. However each of these approaches has its own pros and cons with regards to cost, performance, and scalability.

If the organization has its users worldwide across different geo locations, having a CDN will help to serve as a central repository.

In package-solution.json, specifying includeClientSideAssets as true will include the assets in .sppkg file. After deploying the sppkg solution to app catalog, SharePoint will unpack the assets to predefined location in tenant.


## Create SharePoint Library as CDN

1. Open SharePoint Site.
2. Click **Settings (gear icon)** > **Add an App**.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/01.png)

3. Click Document Library tile.
4. Give it a name - SPFxDeploy.
5. Click **Settings (gear icon)** > **Library settings**.
6. Under **Permissions and Management**, click **Permissions for this document library**.
7. Give read permission to all users (use everyone user).


## Configure SPFx Solution for SharePoint Library

**Update package details**

1. Open command prompt.
2. In the command prompt, navigate to SPFx solution folder.
3. Type ```code .``` to open the solution in code editor of your choice.
4. Open package-solution.json file from config folder. This file takes care of solution packaging.
5. Set includeClientSideAssets value as false. The client side assets will not be packaged inside final package (sppkg file) because these will be hosted in SharePoint library.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/02.png)


**Update CDN Path**

1. Open write-manifests.json from config folder.
2. Update CDN base path as SharePoint library url.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/03.png)


**Prepare the package**

In the command prompt, type the below command.

```
gulp bundle --ship
```

This will minify the required assets to upload to CDN. The ship switch denotes distribution. The minified assets are located at "temp\deploy" folder.

Upload the files from "temp\deploy" folder to SharePoint library.

![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/04.png)


**Deploy Package to SharePoint**

In the command prompt, type the below command.

```
gulp package-solution --ship
```

This will create the solution package (sppkg) in sharepoint\solution folder.


**Upload package to app catalog**

1. Open the SharePoint app catalog site.
2. Upload the solution package (sppkg) from sharepoint\solution folder to app catalog.
3. Make sure the url is pointing to SharePoint library.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/05.png)

4. Click **Deploy**.


## Test the web part

1. Open any SharePoint site in your tenant.
2. Add the App to your site from **Add an App** menu.
3. Edit any page and add the web part.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/06.png)

4. Click F12 to open developer toolbar. Confirm that it is served from SharePoint library.

    ![](/media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/07.png)


## Summary

There are multiple options to deploy the SPFx client side webparts ranging from Azure BLOB storage, Office 365 public CDN, or even regular SharePoint library. Considering the pros and cons of each option, choose the one that suits you better.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-deploy-spfx-webparts-to-sharepoint-library/).
