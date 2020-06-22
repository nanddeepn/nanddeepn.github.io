---
title: "SharePoint Framework - Configure SPFx WebPart Icon"
date: "2018-12-13"
---

## Overview

By the saying “A picture speaks a thousand words”, we should be able to represent our work in a pictorial view which can help audiences to recognize the purpose. In SharePoint terminology, if we are developing a component the icon of the component should be self-explanatory. SharePoint framework client web parts when being added to page, displays icon and title. The icons helps end users to build some perspective about the web part.

In this article, we will explore how we can configure icons for SharePoint framework client web parts and various available mediums to refer for icons.

## The out of box web part look and feel

Please refer my previous article to get started with developing SharePoint framework web part.

**How to get started**

1. Download the attached code
2. Open command prompt, navigate to the folder
3. On the command prompt, type below command

npm install

1. Once the npm packages are installed, on the command prompt type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-278.png)

Our web part appears in the list with default icon. The icon is defined in the web part manifest as a predefined entry.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-279.png)

Let us explore various ways to set new icon for our web part.

 

## Office UI Fabric Icons

Office UI fabric provides various icons to be used in SharePoint framework web parts. Here is a list of icons provided by Office UI Fabric ([https://developer.microsoft.com/en-us/fabric#/styles/icons](https://developer.microsoft.com/en-us/fabric#/styles/icons))

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-280.png)

To use a specific icon, mouse over the icon to get the name of the icon and set it to officeFabricIconFontName property in the web part manifest file which defines the icon to use.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-281.png)

While adding the web part to the page, this icon will be displayed.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-282.png)

**Advantages of using Office Fabric Icon**

- The icon need not to be deployed along with web part assets
- Icon automatically adjust the screen resolution

 

## Use external icon image

If we do not find any matching image from Office UI fabric offerings, we can use our custom image to be displayed as an icon. Any external icon can be specified in iconImageUrl property as an absolute url.

{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",  
  "id": "2e093467-be35-4f8c-a72d-0206c8357c5b",  
  "alias": "HelloWorldWebPart",  
  "componentType": "WebPart",  
  
  // The "\*" signifies that the version should be taken from the package.json  
  "version": "\*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false,  
  
  "preconfiguredEntries": \[{  
    "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other  
    "group": { "default": "Other" },  
    "title": { "default": "HelloWorld" },  
    "description": { "default": "HelloWorld description" },  
    "iconImageUrl": "https://contoso.sharepoint.com/PublishingImages/baloon.png",  
    "properties": {  
      "description": "HelloWorld"  
    }  
  }\]  
}

In this example, an image is uploaded to SharePoint image library (e.g. "https://contoso.sharepoint.com/PublishingImages/baloon.png") and this absolute path is specified in iconImageUrl property of web part manifest. Please note to take away officeFabricIconFontName property from the web part manifest.

While adding the web part to the page, this icon will be displayed.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-283.png)

**Advantages of using an external image as icon:**

- It gives flexibility to choose our own image as an icon
- However the image has to be deployed as an web part asset
- Our own custom image might lose quality on higher DPIs

## Use base64 encoded image

We can also have our custom image base64 encoded and use the base64 string instead of absolute url to image. We can use any of the online services to base64 encode our image. Here is an example of such encoding service [https://www.base64-image.de/](https://www.base64-image.de/)

Follow below steps to encode the image

1. In the browser, open [https://www.base64-image.de/](https://www.base64-image.de/)
2. Upload the image

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-284.png)

1. Click “show code” to get the base64 encoded string

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-285.png)

1. Click “copy to clipboard” to get the base64 encoded string and set it to iconImageUrl property in the web part manifest.

{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",  
  "id": "2e093467-be35-4f8c-a72d-0206c8357c5b",  
  "alias": "HelloWorldWebPart",  
  "componentType": "WebPart",  
  
  // The "\*" signifies that the version should be taken from the package.json  
  "version": "\*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false,  
  
  "preconfiguredEntries": \[{  
    "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other  
    "group": { "default": "Other" },  
    "title": { "default": "HelloWorld" },  
    "description": { "default": "HelloWorld description" },  
    "iconImageUrl": "data:image/png;base64,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",  
    "properties": {  
      "description": "HelloWorld"  
    }  
  }\]  
}

**Advantages of using base64 encoded image**

- Base 64 encoding works on almost all formats (e.g. bitmap, jpg, png, etc.)
- The image need not to be deployed along with web part assets

 

## General Observation

If you specify both the officeFabricIconFontName and the iconImageUrl properties in web part manifest, icon specified in officeFabricIconFontName will be used.

 

## Summary

Each web part should have an icon which will speak about itself. Prefer to use Office UI fabric available icons in your web parts. In case the Office UI fabric icons does not work for you, then you may choose to use an external icon image or base64 encoded image.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-configure-spfx-webpart-icon/).
