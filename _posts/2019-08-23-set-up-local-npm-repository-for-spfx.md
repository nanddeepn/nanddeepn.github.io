---
title: "Set Up Local NPM Repository For SPFx"
date: "2019-08-23"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-08-23-set-up-local-npm-repository-for-spfx/01.png
  teaser: media/2019-08-23-set-up-local-npm-repository-for-spfx/01.png
tags:
  - "2019"
  - August 2019
last_modified_at: 2019-08-23T00:00:00-00:00
---

## Overview

SharePoint Framework relies on NPM as a package manager to find and install Node.js packages to the solution. It helps to manage package dependencies gracefully. NPM repository hosts various packages to download and use. This requires a working internet connection. Downloading same packages each time to create an SPFx solution is redundant and time consuming. It would have been better, if we have those npm packages readily available on our machine (without a need of working internet connection).

In this article, we will explore how we can set up a local npm repository.


## Need for Local NPM Repository

There may be several reasons to set up local offline npm repository, including below:

1. Restricted environment
2. Make same packages available to the group of people
3. No internet connectivity
4. Speed up the process


## local-npm NPM Package

It works as proxy between you and online npm repository. Once set, all **npm install** commands are sent through local offline npm server. More information [https://github.com/local-npm/local-npm](https://github.com/local-npm/local-npm)

**How it works?**

A very first **npm install** command for any module is served from online npm repository. The npm module and its dependencies are stored to local database. All of the subsequent modules are served from local database.

**Will I get stale packages over the time?**

No. local-npm always listen for changes to online npm repository and updates the modules when they change.

**Install local-npm**

On the command prompt, run below command.

```
npm install -g local-npm
```

Type below command to start the npm packges replication process.

```
local-npm
```

![](/media/2019-08-23-set-up-local-npm-repository-for-spfx/01.png)

The local packages are downloaded to the folder location, where the above command is run.

The entire replication process may take few hours. However, we do not have to wait until the replication completes. Since local-npm is offline first, it will try to get the package from local first. If not found, it will download and use from npm online repository.

With the **local-npm** running on the command prompt, open another command prompt instance and instruct npm to prefer local repository, by running below command.

```
npm set registry http://127.0.0.1:5080
```


## Test the npm local repository

It is time to test that packages are being referred from npm local repository.

**Download any missing packages to local**

1. Make sure your internet connection is working.
2. Run ```npm install``` to download modules or chain of dependencies specified in the package.json
3. local-npm should download and cache the modules, if missing from local repository.

**Clear npm cache**

1. On the command prompt, run ```npm cache clean```.
2. Delete **node_modules** folder from the solution.

**Test the local repository**

1. Turn off the internet connection.
2. On the command prompt, run ```npm install```
3. All npm packages should be installed from local repository


## Browsing Local npm Repository

local-npm provides UI based experience to browse local modules. It can be accessed from [http://localhost:5080/\_browse](http://localhost:5080/_browse)

![](/media/2019-08-23-set-up-local-npm-repository-for-spfx/02.png)


## Test local-npm with Yeoman

Create a SPFx solution with Yeoman generator (yo @microsoft/sharepoint). Everything including dependency installation works fine.

![](/media/2019-08-23-set-up-local-npm-repository-for-spfx/03.png)


## Switch back to online repository

To switch back to npm online repository, run below command.

```
npm set registry http://registry.npmjs.org
```


## Summary

For several reasons, you might want to have the npm packages available offline. local-npm works as proxy between you and online npm repository. local-npm is offline first, it will try to get the package from local first. If not found, it will download and use from npm online repository. local-npm is faster than online repository.

NPM team does not have any directions to head towards offline repository. However, this is something cool to try out once.

This content was originally posted [here](https://www.c-sharpcorner.com/article/set-up-local-npm-repository-for-spfx/).
