---
title: "SPFx Commands Reference Sheet"
date: "2019-09-25"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/common/SPFx-2.png
  teaser: media/common/SPFx-2.png
tags:
  - "2019"
  - September 2019
last_modified_at: 2019-09-25T00:00:00-00:00
---

## Overview

SharePoint Framework supports modern toolchain. The modern toolchain includes a wide range of open source tools including Node.js, npm, Yeoman, Gulp, TypeScript, and more. So is the set of commands used across the lifetime of SPFx solution development right from solution creation to deployment and testing. We use the set of commands based on our implementation needs. The command list is huge, so I decided to try making them available in one place.

In this article, we will revise all the useful commands for SharePoint Framework.


## Node.js Commands

Node.js is an open source JavaScript runtime, used to build and run the applications.

**Command**|**Description**|**Example**
```-v, --version```|Display node's version.|```node -v```
```-h, --help```|Display node command line options|```node -h```


## NPM Commands

Node Package Manager installs modules and its dependencies.

**Install**

Install a package.

_Outline_

```
npm install (with no args, in package directory)
npm install [<@scope>/] <package-name>
```

_aliases: npm i, npm add_

**Command**|**Description**|**Example**
```npm i -g <package-name>```|Install a package globally|```npm i -g @microsoft/generator-sharepoint```
```npm install --global <package-name>```| |  
```npm i -g <package-name1> <package-name2>```|Install multiple packages at once|```npm i -g yo gulp```
```npm install --global <package-name1> <package-name2>```| |  
```npm install (with no args, in package directory)```|Install all modules listed as dependencies in package.json|```npm i```
```npm i -g npm```|Update npm itself|```npm i -g npm```
```npm i <package-name> --save```|Enable NPM to include the packages to dependencies section of the package.json file|```npm i jquery –-save```
```npm i <package-name> --save-exact```|Avoid caret or tilde dependencies only at first level| 
```npm i <package-name> -E```| |  
```npm i <package-name> --save-dev```|Package will appear in your devDependencies| 
```npm i <package-name> -D```| | 
```npm i <package-name> -- save-optional```|Package will appear in your optionalDependencies| 
```npm i <package-name> -O```| | 
```npm i tsd -g```|TSD is a package manager to search and install TypeScript definition files| 


**Update**

Update a package.

_Outline_

```npm update [-g] [<package-name>...]```

_aliases: up, upgrade_

**Description**|**Command**|**Example**
Report globally outdated packages|```npm outdated --global```| 
Report locally outdated packages|```npm outdated```| 
Update the package globally|```npm update -g <package-name>```|```npm update -g @microsoft/generator-sharepoint```
Update the package globally|```npm update <package-name>```|```npm update jquery```
Update all dependencies to the minimum required version|```npm update --save```| 


## Other helpful commands

**Command**|**Description**
```npm ls <package-name> -g --depth=0```|Check the version of installed package
```npm shrinkwrap```|Lockdown the package dependencies
```npm link```|symlink a package folder (library component)
```npm ls -g <library-name>```|Check the folder location of SPFx library
```npm unlink <library-name>```|Unlink an SPFx library that was symlinked during development in your SPFx project. Navigate to SPFx project root folder and run the command.
```npm unlink```|Remove local npm link to the library, navigate to the SPFx library root folder and run the command.

 
## Gulp

Automates SPFx development and deployment tasks.

```gulp <command> [optional pararms]```

**Command**|**Description**
```gulp bundle```|Creates a new build and writes manifest to the temp folder. This will minify the required assets to upload to CDN. The minified assets are located at "temp\deploy" folder.
```gulp bundle --ship```|The ship switch denotes distribution.
```gulp package```|Create the packages inside ./dist folder
```gulp package-solution```|Create the solution package (sppkg) in sharepoint\solution folder
```gulp package-solution --ship```|The ship switch denotes distribution.
```gulp deploy-azure-storage```|Deploy the assets (JavaScript, CSS files) to Azure CDN
```gulp --update```|Update config.json to the latest version
```gulp clean```|Removes all files from previous builds
```gulp clean-build```|Clean the build folder.
```gulp serve```|Serve code for testing in the browser
```gulp serve --nobrowser```|Will not automatically launch the SharePoint Workbench
```gulp build```|Build all of the packages
```gulp test```|Runs the tests specified in each package's tests folder

 
## Yeoman SharePoint Generator

Scaffolding tool for Modern web apps. Used as SPFx solution generator and builds the required project structure.

```yo @microsoft/sharepoint [optional pararms]```

**Optional Parameter**|**Description**
```--help```|See the list of command-line options available for the SharePoint generator.
```--skip-cache```|Do not remember prompt answers.
```--skip-install```|Do not automatically install dependencies.
```--component-type```|The type of component ("webpart", "extension", or "library")
```--component-name```|Name of the component. (Web part name)
```--component-description```|Description of the component. (Web part description)
```--framework```|Framework to use for the solution. ("none", "react", or "knockout")
```--plusbeta```|Use the beta packages
```--extension-type```|The type of extension (ApplicationCustomizer, FieldCustomizer, ListViewCommandSet)
```--solution-name```|SPFx solution name
```--environment```|Target environment for SPFx solution ("onprem", "onprem19" or "spo")
```--package-manager```|The package manager for the solution ("npm", "pnpm", or "yarn")
```--skip-feature-deployment```|Allow the tenant admin the choice of being able to deploy the components to all sites immediately without running any feature deployment or adding apps in sites.
```--is-domain-isolated```|The web part will be rendered in isolated domain using IFrame.




## Other Commands

**Command**|**Description**|**Example**
```tsd install <package-name> --save```|TSD is a package manager to search and install TypeScript definition files. Typings will help for auto complete while writing the code in the code editor.|```tsd install jquery --save```
```code .```|Open the solution in the code editor of your choice.| 



## Summary

SharePoint Framework supports a wide range of open source tools, so is the set of commands used across the lifetime of SPFx solution development right from solution creation to deployment and testing. I have tried to get together a commonly used.

Feel free to add to the list!


## References

- [Overview of the SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/sharepoint-framework-overview)
- [npm-install](https://docs.npmjs.com/cli/install)
- [npm-update](https://docs.npmjs.com/cli/update.html)
- [Scaffold projects by using Yeoman SharePoint generator](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/toolchain/scaffolding-projects-using-yeoman-sharepoint-generator)
- [Gulp.js](https://gulpjs.com/)
- [Node.js](https://nodejs.org)
 

This content was originally posted [here](https://www.c-sharpcorner.com/article/spfx-commands-cheat-sheet/).
