---
title: "SharePoint Framework – Deploy SPFx WebParts to Azure CDN"
date: "2018-07-28"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/03.png
  teaser: media/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/03.png
tags:
  - "2018"
  - July 2018
last_modified_at: 2018-07-28T00:00:00-00:00
---

## Overview

In the previous article Develop First Client Side Web Part, we developed SharePoint Framework client side web parts and tests it on local SharePoint workbench. The local SharePoint workbench allows to quickly test the SPFx web parts. The Node.js provides the hosting platform to run on local environment. However in the real life scenarios we will have to deploy these SPFx web parts to some hosting environments from where it can be served to SharePoint.

Majorly the SPFx web parts are deployed on either of below CDN (Content Delivery Network) options:

1. Azure CDN (This article)
2. [Office 365 Public CDN](/posts/2018-08-02-deploy-spfx-webparts-to-office-365-public-cdn/)
3. [SharePoint Library in your tenant](/posts/2018-08-01-deploy-spfx-webparts-to-sharepoint-library/)

In this article, we will have a look at how SPFx web parts can be deployed to Azure CDN.


## Configure MS Azure Storage Account

Follow below steps to configure MS Azure storage account and integrate it with CDN:

1. Login to MS Azure Portal ([https://portal.azure.com](https://portal.azure.com)).
2. Click **Create a resource**.
3. Click **Storage account - blob, file, table, queue**.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/01.png)

4. Fill out the form.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/02.png)

5. Click **Create.**
6. The storage account endpoint can be referred by URI – http://<StorageAccountName>.blob.core.windows.net


## Configure BLOB Container

1. Select storage account from dashboard.
2. Click **Blobs**.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/03.png)

3. Click **+ Container**.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/04.png)

4. Click Access keys and note down any of the access key.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/05.png)


## Enable Azure CDN for Storage Account

We will enable CDN for the above created storage account.

1. Select storage account from dashboard.
2. Under **BLOB Service**, select **Azure CDN**.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/06.png)

3. Click **Create**.
4. The endpoint will appear in the endpoint list.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/07.png)

5. The BLOB container can be accessed from url: http://<EndpointName>.azureedge.net/<myPublicContainer>/<BlobName>


## Configure SPFx Solution for Azure CDN

**Update package details**

1. Open command prompt.
2. In the command prompt, navigate to SPFx solution folder.
3. Type **code .** to open the solution in code editor of your choice.
4. Open **package-solution.json** file from **config** folder. This file takes care of solution packaging.
5. Set **includeClientSideAsserts** value as **false**. The client side assets will not be packaged inside final package (sppkg file) because these will be hosted on external CDN.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/08.png)


**Update Azure storage account details to Solution**

1. Open **deploy-azure-storage.json** from **config** folder to specify the Azure storage account details.
2. Update values as below:
    - account: storage account name
    - container: BLOB container
    - accessKey: storage account access key (primary or secondary)

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/09.png)


**Update CDN Path**

1. Open **write-manifests.json** from **config** folder.
2. Update CDN base path as BLOB container end point.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/10.png)


**Prepare the package**

In the command prompt, type below command:

```
gulp bundle --ship
```

This will minify the required assets to upload to CDN. The ship switch denotes distribution. The minified assets are located at "temp\deploy" folder.


**Deploy assets to Azure Storage**

In the command prompt, type below command:

```
gulp deploy-azure-storage
```

This will deploy the assets (JavaScript, CSS files) to Azure CDN.


## Deploy Package to SharePoint

The next step is to deploy the app package (sppkg) to SharePoint App catalog.

In the command prompt, type below command:

```
gulp package-solution --ship
```

This will create the solution package (sppkg) in sharepoint\solution folder.


**Upload package to app catalog**

1. Open the SharePoint app catalog site.
2. Upload the solution package (sppkg) from sharepoint\solution folder to app catalog.
3. Make sure the url is pointing to Azure CDN.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/11.png)

4. Click **Deploy.**


## Test the web part

1. Open any SharePoint site in your tenant.
2. Add the App to your site from **Add an App** menu.
3. Edit any page and add the web part.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/12.png)

4. Click F12 to open developer toolbar. Confirm that it is served from Azure CDN.

    ![](/media/2018-07-28-sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/13.png)


## Summary

Azure CDN is one of the preferred option to deploy the SPFx client side webpart. SharePoint framework provides out of box support to deploy assets to Azure CDN. You can update the CDN path later in SPFx solution and redeploy it.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-deploy-spfx-webparts-to-azure-cdn/).
