---
title: "SharePoint Framework Development Guidelines"
date: "2019-05-13"
---

## Overview

SharePoint developers were very much used to server-side code development with .Net. With the introduction of JSOM, the client-side development started getting popular amongst the SharePoint developers. But, still most were happy writing managed .Net code using C#. It may be server side or CSOM.

SharePoint Framework supports open source toolchain (including Node.js, TypeScript, npm, etc.). The development paradigm has shifted from server-side development to client side. However good thing is, SharePoint developers adopted to this change. Microsoft is giving more capabilities with SharePoint framework. The SPFx is now entering maturity stage.

There is a sufficient information available on internet about SPFx for best practices, coding guidelines, performance optimization, and related areas.

In this article, we will stick to the basics and revise few guidelines for SPFx development that will help to build better SPFx solutions.

## Upgrade to latest SPFx version

SharePoint Framework is evolving continuously based on user feedbacks, fixes and improvements. As a result, Microsoft releases newer versions of SPFx regularly. It is important to stay updated with latest SPFx version. It is always not an easy task as it involves upgrade of each web part, extension, libraries, and components to newer version and test out the entire solution for unit and integration part.

Use below command to view the version of SPFx Yeoman being used.

npm -g ls @microsoft/generator-spfx --depth=0

Upgrade the outdated packages

While creating a new SPFx solution, Yeoman generator fetches latest version of npm packages to create client-side project. The existing npm packages could be outdated in the future as there will be a new versions of npm packages are available.

Use below command to view the list of outdated npm packages.

npm outdated

This command will list down all the packages that needs upgrade along with information of current version installed in your project and latest available version.

For each package you want to update, run below command.

npm install package-name@newversion --save

Test Cases

As we try to be in sync with latest npm packages, this also silently says that nothing should break after the updates. This situation demands for having unit and integration test cases in place. The test cases can be implemented using Jest.

## npm shrinkwrap

Locking down the package dependency is very important to ensure the same code works for all. The SPFx code is distributed without node\_modules folder, because it can be built later using “npm install” command. If the npm packages are not locked down then “npm install” downloads the latest version of npm packages. This may break any dependent packages.

To lock down the npm packages, use below command.

npm shrinkwrap

## Use Office UI Fabric Controls

SharePoint and UI developers have utilized bootstrap for developing nice user interfaces in the past. However, Microsoft has provided Office UI Fabric for SharePoint Framework. Office UI Fabric is an official front-end framework to build experiences in Office 365. It helps to develop robust and consistent design across SharePoint modern experiences.

## App Package Deployment

SPFx app packages can be deployed at tenant level, which makes the app available all across the tenant. Which is a good idea. However, if for some reason you are using a single Office 365 tenant for all development, UAT and production purpose (e.g. different site collections for each environment) then this will not be a viable option. In such cases consider using app catalog at site collection level.

## Performance Optimization

Optimize the performance of SPFx solution by following best practices. Below few points might help:

- Always use release builds in production.
- Do not include third-party libraries in the bundle. Consider loading them from CDN or a hosting location owned by organization.
- Reference (or import) only the necessary components in code.
- Allow files to be cached by proxies.
- Minimize HTTPS calls and data size transferred.

## PnP Samples

SharePoint PnP (Patterns and Practices) has various samples created around common business scenarios. Before building web parts, extensions or property pane controls first verify if they are readily available in PnP samples.

- PnP client side web part samples ([https://github.com/SharePoint/sp-dev-fx-webparts](https://github.com/SharePoint/sp-dev-fx-webparts))
- PnP SPFx extensions samples ([https://github.com/SharePoint/sp-dev-fx-extensions](https://github.com/SharePoint/sp-dev-fx-extensions))
- PnP reusable property pane controls ([https://sharepoint.github.io/sp-dev-fx-property-controls/](https://sharepoint.github.io/sp-dev-fx-property-controls/))

## DevOps

Implement CI (Continuous integration) / CD (Continuous deployment) to verify every change rigorously. Continuous Integration (CI) is the process of automating the build and testing of code when a developer commits changes to source control. Continuous Deployment (CD) takes a package from the build and deploys it to a designated environment.

## Summary

SharePoint framework is widely accepted by SharePoint developers. While implementing SPFx solutions, it is important to follow certain guidelines and build your own governance to accelerate the development, avoid common mistakes, and optimize the performance.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-development-guidelines/).
