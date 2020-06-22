---
title: "IE 11 Polyfill Package for SPFx"
date: "2019-05-28"
---

## Overview

SharePoint Framework client-side web parts are developed using HTML, CSS and JavaScript code. Microsoft claims SPFx to be compatible with all modern browsers including mainly Google Chrome, IE Edge, and IE 11. SPFx web parts seem to be working well with all browsers except few cases in IE11. From a long time, IE 11 has been a nightmare for developers. The code which runs fine on all browsers did not work as expected in IE 11 and we have been patching to make it work. SPFx development is also not an exception to this scenario.

In this article, we will explore common issues for SPFx with IE 11. Reasons behind it and how SharePoint PnP (Patterns and Practices) has helped to overcome these issues.

## What IE 11 is lacking?

Comparatively, IE 11 is an older browser. However, there is a significant user base still using IE 11 as a primary browser. Being an old browser, IE 11 lacks a few features like fetch, map, and proxy. During SPFx development we normally make use of these features and they fail to work in IE 11.

## Polyfilling IE 11

There are lots of browsers and their versions available and each has different sets of features to offer. The latest browser supports most modern things, while older browser versions do not. However, we still have to support our applications on older browser versions. Polyfill makes it simpler to support all browser versions by recreating the missing features. Generally, Polyfill is a service which accepts a request for a set of browser features and returns only missing functionality or polyfills that are needed for the requesting browser.

IE 11 does not support fetch, map, and proxy which is commonly used in SPFx development. SharePoint PnP has designed a polyfill package for IE 11 targeting SPFx support.

## Create SPFx Solution

1. Open a command prompt. Create a directory for SPFx solution.

md pnp-polyfill-ie11

1. Navigate to the above created directory.

cd pnp-polyfill-ie11

1. Run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-385.png)

- **Solution Name:** Hit enter to have default name (pnp-polyfill-ie11 in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for the component:** Here we can select the target environment where we are planning to deploy the client web part i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 or 2019 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Use the current folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: Y
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create client side web part or an extension.
    - Selected choice: WebPart
- **Web Part Name:** Hit enter to select the default name or type in any other name.
    - Selected choice: PnPPolyfill
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: PnP Polyfill for IE11
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Once the scaffolding is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt, type below command to open the solution in the code editor of your choice.

code .

Add @pnp/sp Package

We will include @pnp/sp to simplify common operations with SharePoint and SPFx. On the command prompt, type the below command to add @pnp/sp npm package to project.

npm i @pnp/logging @pnp/common @pnp/odata @pnp/sp --save

Code the WebPart

Let us create a basic SPFx web part to display items from SharePoint list and use map function to render them.

1. Open main web part file at “src\\webparts\\pnPPolyfill\\PnPPolyfillWebPart.ts”
2. Add below import.

// @pnp/sp imports    
import { sp, Web } from '@pnp/sp';

1. Update Render method as below to show items from a SharePoint list.

public render(): void {  
  sp.web.lists.getByTitle("KBArticles").items.filter(\`ID gt 0\`).get().then(r => {    
    this.domElement.innerHTML += r.map(l => \`${l.Title}<br />\`);    
  });  
}

Test the WebPart

1. On the command prompt, type “gulp serve”.
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the web part to the page.
5. Observe the web part in IE 11 and Google Chrome.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-386.png)

The web part displays list items in Google Chrome. However, it fails to render the same in IE 11. The reason is, we have used filter, map operations in our SPFx web part.

## IE 11 Polyfill package

Polyfill package supports all browser versions by recreating the missing features. On the command prompt, run below command to include the IE 11 Polyfill package.

npm install --save @pnp/polyfill-ie11

Import Polyfill to our class

1. Open main web part file at “src\\webparts\\pnPPolyfill\\PnPPolyfillWebPart.ts”
2. Add below import.

// IE 11 Polyfill import
import "@pnp/polyfill-ie11";

That’s it. We do not need to make any code level changes for IE 11. Let us try loading our web part in IE 11 again and it works now.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-387.png)

## Summary

In this article, we explored common issues for SPFx with IE 11. Reasons behind it and how SharePoint PnP (Patterns and Practices) has helped to overcome these issues by introducing IE 11 Polyfill package. The Polyfill package supports all browser versions by recreating the missing features. IE 11 is now relatively old browser. However, we still need to support our applications on IE 11. Polyfill helps to fill in the gaps for missing functionalities in older browsers. PnP has provided IE 11 Polyfill package targeting SPFx support, which includes needed functionality without having us to determine which polyfills to add.

This content was originally posted [here](https://www.c-sharpcorner.com/article/ie-11-polyfill-package-for-spfx/).
