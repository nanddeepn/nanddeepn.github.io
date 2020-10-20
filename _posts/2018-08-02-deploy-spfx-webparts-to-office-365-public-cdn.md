---
title: "SharePoint Framework - Deploy SPFx WebParts to Office 365 Public CDN"
date: "2018-08-02"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/09.png
  teaser: media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/09.png
tags:
  - "2018"
  - August 2018
last_modified_at: 2018-08-02T00:00:00-00:00
---

## Overview

Please read through my previous article Develop First Client Side Web Part ) to get started, develop first SPFx client side web part and test it on a local SharePoint workbench. In the real life scenarios we will have to deploy these SPFx webparts to some hosting environments from where it can be served to SharePoint.

Below are the more common followed deployment approaches

1. [Azure CDN](/posts/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/)
2. Office 365 Public CDN (This article)
3. [SharePoint Library in your tenant](/posts/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/)

In this article, we will explore how SPFx webparts can be deployed to office 365 CDN.


## Configure CDN in Office 365 Tenant

Any of the document library in SharePoint Online tenant can be promoted as a CDN, which will help to serve the JS files for SPFx client webparts hosted in SharePoint. This CDN location being public can be accessed easily.

To configure the CDN in Office 365 follow below steps:

1. Download and install latest version of SharePoint Online Management Shell from [https://www.microsoft.com/en-us/download/details.aspx?id=35588](https://www.microsoft.com/en-us/download/details.aspx?id=35588)
2. Open the SharePoint Online Management Shell.
3. Connect to SharePoint Online tenant using below cmdlet.

    ```
    Connect-SPOService -Url https://[tenant]-admin.sharepoint.com
    ```

    Replace ```[tenant]``` with your O365 tenant name.

4. Run below set of commands to review the existing Office 365 public CDN settings on your tenant.

    ```
    Get-SPOTenantCdnEnabled -CdnType Public
    ```

    This command will return the status of CDN. True if CDN is enabled, false otherwise.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/01.png)

    ```
    Get-SPOTenantCdnOrigins -CdnType Public
    ```

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/02.png)

    This command will return the location of already configured CDN origins. The urls are relative. On first instance, it will not return any values as CDN is not yet setup.

    ```
    Get-SPOTenantCdnPolicies -CdnType Public
    ```

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/03.png)

    This command will return the policy settings for CDN

5. Run below command to enable the CDN.

    ```
    Set-SPOTenantCdnEnabled -CdnType Public
    ```

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/04.png)

    After enabling the CDN, \*/CLIENTSIDEASSETS origin is by default added as valid origin. By default allowed file extensions are: CSS, EOT, GIF, ICO, JPEG, JPG, JS, MAP, PNG, SVG, TTF, and WOFF.

    The configuration takes up to 15 to 20 minutes.

6. To check the current status of CDN end points, run below command.

    ```
    Get-SPOTenantCdnOrigins -CdnType Public
    ```

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/05.png)

    The origin will be listed without (configuration pending) status when it is ready. Please be patient for 15 to 20 minutes until it is ready.

    Once CDN origin is ready, the output will be as below.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/06.png)


## Setup New Office 365 CDN in Tenant

Follow below steps to setup new CDN location

1. Open SharePoint site (e.g. https://[tenant].sharepoint.com/sites/[site-collection-name].
2. Create a document library (e.g. CDN).
3. Run below command in SharePoint Online Management Shell.

    ```
    Add-SPOTenantCdnOrigin -CdnType Public -OriginUrl sites/[site-collection-name]/[document-library]
    ```

    The OriginUrl is a relative url. New CDN origin configuration will again take 15 to 20 minutes.

4. Run Get-SPOTenantCdnOrigins to check the status.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/07.png)

5. Create a folder in document library preferably with name of SPFx web part (e.g. O365CDNDeploy).
6. To get the path of CDN type below url in browser.

    ```
    https://[tenant].sharepoint.com/_vti_bin/publiccdn.ashx/url?itemurl=https://[tenant].sharepoint.com/sites/[site-collection-name]/[document-library]/[folder]
    ```


## Configure SPFx Solution for Azure CDN

**Update package details**

1. Open command prompt.
2. In the command prompt, navigate to SPFx solution folder.
3. Type ```code .``` to open the solution in code editor of your choice.
4. Open package-solution.json file from config folder. This file takes care of solution packaging.
5. Set includeClientSideAssets value as false. The client side assets will not be packaged inside final package (sppkg file) because these will be hosted on external Office 365 public CDN.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/08.png)


**Update CDN Path**

1. Open write-manifests.json from config folder.
2. Update CDN base path as Office 365 CDN end point.

    ```
    https://publiccdn.sharepointonline.com/[tenant]/sites/[site-collection-name]/[document-library]/[folder]
    ```

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/09.png)


**Prepare the package**

In the command prompt, type the below command.

```
gulp bundle --ship
```

This will minify the required assets to upload to CDN. The ship switch denotes distribution. The minified assets are located at "temp\deploy" folder.

Upload the files from "temp\deploy" folder to CDN location (SharePoint document library setup as CDN).

![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/10.png)


**Deploy Package to SharePoint**

In the command prompt, type the below command.

```
gulp package-solution --ship
```

This will create the solution package (sppkg) in sharepoint\solution folder.


**Upload package to app catalog**

1. Open the SharePoint app catalog site.
2. Upload the solution package (sppkg) from sharepoint\solution folder to app catalog.
3. Make sure the url is pointing to Office 365 CDN.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/11.png)

4. Click Deploy.


## Test the web part

1. Open any SharePoint site in your tenant.
2. Add the App to your site from **Add an App** menu.
3. Edit any page and add the web part.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/12.png)

4. Click F12 to open developer toolbar. Confirm that it is served from Office 365 Public CDN.

    ![](/media/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/13.png)


## Summary

Office 365 CDN is one of the feasible option to deploy the SPFx client side webpart. It requires some manual steps to copy the assets to CDN path. You can update the Office 365 CDN path later in SPFx solution and redeploy it.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-integrating-jquery-with-spfx-webparts/).
