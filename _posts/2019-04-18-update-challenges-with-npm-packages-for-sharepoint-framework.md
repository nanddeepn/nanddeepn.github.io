---
title: "Update Challenges with npm Packages for SharePoint Framework"
date: "2019-04-18"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/01.png
  teaser: media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/01.png
tags:
  - "2019"
  - April 2019
last_modified_at: 2019-04-18T00:00:00-00:00
---

## Overview

SharePoint Framework (SPFx) supports modern development toolchain, which utilizes various npm packages to build the solution. Updating the npm packages to latest version is important to get benefits of latest and greatest features offered by that npm package.

In this article, we will explore the need for npm package update, how to update the npm packages, and common issues and resolutions while updating to newer npm packages. This article is not only specific to npm package update with SharePoint Framework scenario, however it can be used generically.


## Need for npm package update

The npm packages gets updated by vendor based on user feedback and issues raised at times. Also to start using newer capabilities and functionalities offered by npm package, it is important to update ourselves to newer npm package version.

One common scenario for npm package update need is the updates to SharePoint Framework version itself. Microsoft releases new version of SharePoint Framework with new capabilities, bug fixes, etc. The updates are not too frequent always, but they get released at regular intervals, which is a good thing. Eventually we have to move towards using new SharePoint Framework version.

One point to note here is that not all the package versions are compatible with each other. Therefore it is important to understand the npm package dependencies and then plan the update accordingly.


## NPM package Update

To find out the outdated npm packages, run below command.

```
npm outdated --global
```

This will list the packages which needs update. To update the individual package, run below command.

```
npm update -g <package-name>
```

This is an ideal scenario. However, in real case we may face challenges getting the outdated npm packages.


## Challenge with npm update

There have been few challenges recently with npm to find out the outdated packages. The npm outdated command fails to get the outdated packages. The error will say: ```Cannot read property 'length' of undefined.```

![](/media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/01.png)

The complete error log is available at mentioned location under "AppData\Roaming\npm-cache\_logs". However it does not help to clearly understand the exact cause of issue and its resolution.

![](/media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/02.png)

Although npm fails to list out the outdated packages, if you know the name of npm package to update, you can still use ```npm update``` command to update that specific npm package.


## Option 1: Update npm

The easy and first option is to update the npm. Run below command to update npm.

```
npm i -g npm
```


## Option 2: Reinstall Node

If updating the npm does not work, second option is to uninstall and reinstall Node. Please install node version 8.x, since SharePoint Framework only supports Node version 8.x.


## Option 3: Edit npm configuration

The npm packages installed globally are available at "C:\Users\<user-name>\AppData\Roaming\npm\node_modules\". Manually edit the "lib/outdated.js" file located at the "npm/node_modules/npm" path. (E.g. C:\Users\nachanan\AppData\Roaming\npm\node_modules\npm\lib\outdated.js)

Update makePretty function as highlighted below.

![](/media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/03.png)

Run the ```npm outdated``` command again to see the outdated packages.

![](/media/2019-04-18-update-challenges-with-npm-packages-for-sharepoint-framework/04.png)


## Summary

It is important to update ourselves to newer npm package version to get the benefits of new enhancements, bug fixes available as part of newer packages. Listing the outdated npm packages command might not work sometimes. This can be resolved by following any of the options mentioned in this article.

This content was originally posted [here](https://www.c-sharpcorner.com/article/update-challenges-with-npm-packages-for-sharepoint-framework/).
