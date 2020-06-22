---
title: "SharePoint Framework – Project Upgrade"
date: "2018-10-31"
---

## Overview

Every product goes under the upgrade cycle and SharePoint Framework is no exception to it. SPFx gets updated on a regular basis. With each update the SPFx provides more functionalities and bug fixes. SharePoint Framework client web parts and extensions are developed using JavaScript packages from various authors. The packages are available on npm site ([https://www.npmjs.com/](https://www.npmjs.com/)). These packages are also gets frequent updates. In order to use the latest offerings the need arises to upgrade our existing SPFx solutions to the newer versions

In this article, we will explore how to upgrade to newer versions of JavaScript packages.

## The Upgrade Confusion

The first question arises to mind that, is there a real need of upgrading existing solution which is tested and working fine for a while?

The second thought is, if you want to take advantage of latest and greatest of SharePoint Framework then upgrade is necessary.

It is an individual choice or a team decision to decide on the upgrade. However to be in sync with the SPFx happenings, it is better to upgrade.

**Is Upgrading Simple?**

The answer is not always. Upgrading SPFx project is not easy always. You may often run into build errors.

## The simple steps to upgrade

In this article, we will reuse the scenario from previous article - [CRUD Operations Using No Framework implemented](https://nanddeepnachanblogs.com/2018/08/spfx-crud-operations-no-framework/). Download the source code from the previous article to get started with upgrading the project.

At the time of writing the previous article, SPFx version available was 1.5.1.

1. Open command prompt.
2. Navigate to the SPFx project folder.
3. Type below command to open the project in code editor.

code .

1. Open package.json.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-246.png)

1. SharePoint Framework packages are denoted by prefix @microsoft/sp-
2. The version here is 1.5.1

**The upgrade process**

The upgrade process will help us to upgrade our solution to latest SPFx version (1.6 at the time of writing this article)

1. Open command prompt.
2. Navigate to the SPFx project folder.
3. Type below command to get the list of outdated packages.

npm outdated

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-247.png)

The list has below information:

- Current: Current version of package used in the solution
- Wanted: Version needed by the project (mentioned in package.json)
- Latest: Latest available version of package

**Upgrade the packages**

We need to upgrade each package reported as outdated.

To upgrade the package to specific version, use below command:

npm install \[package\]@\[version\] --save

To upgrade the package to latest version, use below command:

npm install \[package\]@latest --save

Example:

npm install @types/chai@latest --save

Or use below command:

npm install \[package\] --save

Example:

npm install @microsoft/sp-core-library --save

**Verify the updates**

1. In the command prompt, type below command to open it in code editor.

code .

1. Open package.json.
2. Verify the packages are updated.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-248.png)

1. Run below command to update config.json to the latest version.

gulp --update

1. Clean up the earlier packages by running below command.

gulp clean

1. Run below command to build the project.

gulp build

1. Run the project with below command.

gulp serve

If this goes well, you have your project upgraded to the new version. If for some reason, upgrade does not go well, follow below steps

1. Install latest version of Yeoman generator version.

npm update -g @microsoft/generator-sharepoint@latest

1. Create another SharePoint Framework solution (let’s call it as Latest-Version-Solution).
2. The Latest-Version-Solution has reference of all latest packages.
3. Compare and merge the package.json. I am using “Code Compare” from devart.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-249.png)

1. Below is merged package.json

{  
  "name": "spfx-crud-no-framework",  
  "version": "0.0.1",  
  "private": true,  
  "engines": {  
    "node": ">=0.10.0"  
  },  
  "scripts": {  
    "build": "gulp bundle",  
    "clean": "gulp clean",  
    "test": "gulp test"  
  },  
  "dependencies": {  
    "react": "15.6.2",  
    "react-dom": "15.6.2",  
    "@types/react": "15.6.6",  
    "@types/react-dom": "15.5.6",  
    "@microsoft/sp-core-library": "1.6.0",  
    "@microsoft/sp-webpart-base": "1.6.0",  
    "@microsoft/sp-lodash-subset": "1.6.0",  
    "@microsoft/sp-office-ui-fabric-core": "1.6.0",  
    "@types/webpack-env": "1.13.6",  
    "@types/es6-promise": "3.3.0"  
  },  
  "devDependencies": {  
    "@microsoft/sp-build-web": "1.6.0",  
    "@microsoft/sp-module-interfaces": "1.6.0",  
    "@microsoft/sp-webpart-workbench": "1.6.0",  
    "tslint-microsoft-contrib": "~5.0.0",  
    "@types/chai": "4.1.6",  
    "@types/mocha": "5.2.5",  
    "ajv": "6.5.4",  
    "gulp": "~3.9.1"  
  }  
}

1. Also, compare and merge tsconfig.json file.
2. Delete package-lock.json or npm-shrinkwrap.json from the solution.
3. Delete node\_modules folder.
4. Run below command to download the updated npm packages.

npm install

1. Delete previous build output by running below command.

gulp clean

1. Lock the package versions by running below command.

npm shrinkwrap

1. Run the SPFx solution.

gulp serve

Summary

It is not mandatory but advisable to upgrade SPFx projects to get benefits of newer functionalities from latest versions. The upgrade process might not be easy, but can be carefully planned and executed.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-project-upgrade/).
