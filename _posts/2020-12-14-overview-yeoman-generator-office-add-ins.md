---
title: "Overview of Yeoman Generator for Office Add-ins"
date: "2020-12-14"
share: true
header:
  image: media/2020-12-14-overview-yeoman-generator-office-add-ins/addins-overview.png
  teaser: media/2020-12-14-overview-yeoman-generator-office-add-ins/addins-overview.png
categories:
  - Office Add-ins
tags:
  - "2020"
  - December 2020
last_modified_at: 2020-12-14T00:00:00-00:00
---

## Overview

Office add-ins are helpful to extend Office applications, deal with Office document contents. Visual Studio has been the popular developer IDE to build the Office add-ins. On the other hand, web technologies like HTML, CSS, and JavaScript can be used to develop Office add-ins platform independently, compared to Visual Studio which supports developers to write their applications on the Windows platform.

In this article, we will explore the Yeoman Generator for Office Add-ins and set up a developer machine for add-ins development.

## Yeoman Generator for building Office Add-ins

Yeoman is an open-source scaffolding tool to generate projects based on available templates. It also supports creating projects for Office. Yeoman runs through a wizard of questions to scaffold the project. It can be thought of as a replacement for File > New Project experience in Visual Studio. It can be used to create Office add-ins for:

- Word
- Excel
- Excel Custom Functions
- PowerPoint
- Outlook
- OneNote
- Project

## Development with Yeoman generator and Visual Studio

We have multiple options to start building our Office Add-ins. Yeoman generator offers more options compared to Visual Studio for development.

**Yeoman generator**

- Supports development with HTML, CSS, and JavaScript. We can develop add-ins using famous JavaScript based frameworks and libraries including Angular, React
- Supports TypeScript for writing our code
- Support for creating add-ins for Word, Excel, Excel Custom Functions, PowerPoint, Outlook, OneNote, Project

**Visual Studio**

- Supports development only with HTML, CSS, and JavaScript
- Only supports JavaScript based development option
- Support for creating add-ins for Word, Excel, PowerPoint, and Outlook

## Setup Developer Environment

Follow the below set of commands to get your developer environment ready.

**Install Node JS**

Install latest LTS version from https://nodejs.org

If you already have NodeJS installed, check the version

```
node -v
```

**Install Code Editor**

Install any of the below code editors

- Visual Studio Code (https://code.visualstudio.com)
- Atom (https://atom.io)
- Webstorm (https://www.jetbrains.com/webstorm)

**Install Yeoman and bower**

Run the below command (to install globally)

```
npm install -g yo bower
```

**Install Yeoman Generator for Office Add-ins**

Run the below command

```
npm install -g generator-office
```

## NPM packages Update

Yeoman, Bower, Yeoman Office add-ins Generator gets installed as NPM packages. Use the below commands to check and update them.

To update NPM, use below command:

```
npm i -g npm
```

To check outdated packages, use below command:

```
npm outdated --global
```

This command will report packages that need updates. Use the below command to update the reported packages.

```
npm update -g <package-name>
```

## Summary

Office add-ins offer to extend Office applications. Yeoman Generator for Office Add-ins is useful to develop Office add-ins platform independently as compared to Visual Studio which is Windows platform dependent.

## References

- [Office Add-ins documentation](https://docs.microsoft.com/en-us/office/dev/add-ins/)
- [Creating Office Add-ins with any editor – Introducing YO OFFICE](https://developer.microsoft.com/en-us/office/blogs/creating-office-add-ins-with-any-editor-introducing-yo-office/)
