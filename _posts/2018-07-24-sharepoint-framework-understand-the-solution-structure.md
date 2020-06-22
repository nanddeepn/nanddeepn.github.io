---
title: "SharePoint Framework – Understand the Solution Structure"
date: "2018-07-24"
---

## Overview

SharePoint Framework (SPFx) client side web parts are lightweight in nature. They can be developed using open source tools such as Node.JS, NPM, Yeoman generators and can be opened in code editors of our choice (Visual Studio Code, Atom, Webstorm). Node Package Manager (NPM) helps to install modules and its dependencies. Yeoman generator carries out the scaffolding and builds the required project structure.

In this article, we will have a close look at the solution structure of SPFx client side web part and understand major elements of it.

## Solution Structure

In the previous article, we developed our first SPFx client side web part.

In the command prompt, run below command to open the solution in code editor of your choice.

code .

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-23.png)

The solution structure consists of various folders and few configuration files at the root. The primary language used is TypeScript (which is a superset of JavaScript). The code mostly uses classes, modules and interfaces written in TypeScript.

## Major Elements of Solution

**Web Part Class (HelloWorldWebPart.ts)**

This defines the entry point of the web part. In our solution, we have only one web part class - HelloWorldWebPart.ts located at src\\webparts\\helloWorld folder. The web part class extends **BaseClientSideWebPart**. Each client side web part should extend from BaseClientSideWebPart, which provides basic functioning for web part.

**Property Type Interface (IHelloWorldWebPartProps)**

The interface resides in the same web part file. The web part class accepts the property type of interface. This interface is used to define our own custom properties for web part. We can add any of below type of property.

- Button
- Checkbox
- Choice group
- Dropdown
- Horizontal rule
- Label
- Link
- Slider
- Textbox
- Multi-line Textbox
- Toggle
- Custom

The properties can be used in web part as below:

<p class="${ styles.description }">${escape(this.properties.description)}</p>

**WebPart Manifest**

HelloWorldWebPart.manifest.json holds the metadata of web part such as display name, description, icon, version, id.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-24.png)

**Config.json**

The configuration file contains the bundling information, the components used in the solution and entry point of the solution.

This file also records the external references (for e.g. jQuery) and its dependencies. Also, references to localization resources are stored in this file.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-25.png)

**deploy-azure-storage.json**

This file is used while deploying the client-side web part to Azure CDN. This file contains Azure storage account details.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/deploy-azure-storage.png)

**package-solution.json**

This file contains solution path configurations.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/package-solution.png)

**gulpfile.js**

Defines gulp tasks to run.

**package.json**

Defines JavaScript library dependencies.

**tsconfig.json**

Defines settings for TypeScript compilation

# Summary

Yeoman generators help to generate SPFx client-side web part solution. The solution has a predefined structure. Each folder and file has its own significance. It is important to understand the role of each file for better development.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-understand-the-solution-structure/).
