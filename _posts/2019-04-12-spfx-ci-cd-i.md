---
title: "Implement CI / CD for SharePoint Framework - Part I"
date: "2019-04-12"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - DevOps
header:
  image: media/2019-04-12-spfx-ci-cd-i/11.png
  teaser: media/2019-04-12-spfx-ci-cd-i/11.png
tags:
  - "2019"
  - April 2019
last_modified_at: 2019-04-12T00:00:00-00:00
---

## Overview

Implementing SharePoint Framework solutions is easy. However, maintaining the SPFx solutions for their integrity as they grow is always challenging. When an SPFx solution is being worked on by a team of considerable size and it undergoes continuous development efforts, testing and deployment of the latest released package is a huge task. Setting up CI (Continuous Integration) and CD (Continuous Deployment) helps to automate the process.


## Continuous Integration

Continuous Integration (CI) is process of automating build and testing of code when a developer commits changes to source control. Commit to source control triggers an automated build which grabs the latest code from version control, builds it and runs tests on it (if configured). Below are few benefits of implementing CI:

- Improved productivity of dev team
- Reduced number of bugs
- Early detection of bugs
- Early to market release


**Build pipeline:**

Build pipeline includes below major steps:

1. Create build definition
2. Install Node JS
3. Restore npm packages
4. Build the solution
5. Package the solution
6. Prepare the Artifacts
7. Publish the Artifacts


## Create Build Definition

Build definition contains definition and configuration for build. Follow below steps to create a new build definition.

1. Login to Visual Studio Online (Azure DevOps) at [https://visualstudio.microsoft.com/vso/](https://visualstudio.microsoft.com/vso/)
2. Select your project to setup build definition.
3. From left navigation, click **Pipelines** > **Builds**.
4. Click **New pipeline**.

    ![](/media/2019-04-12-spfx-ci-cd-i/01.png)

5. Select the source to connect to your source repository. Click **continue**.

    ![](/media/2019-04-12-spfx-ci-cd-i/02.png)

6. Under **Select a template**, select **Empty Pipeline**.

    ![](/media/2019-04-12-spfx-ci-cd-i/03.png)

7. The build definition has default agent. We can add multiple tasks to agent to define our build.

    ![](/media/2019-04-12-spfx-ci-cd-i/04.png)


## Install Node JS

1. On the default agent, click + sign.
2. Search for **Node**.
3. Add Node.js tool installer.

    ![](/media/2019-04-12-spfx-ci-cd-i/05.png)

4. Specify the version as 8.x, since SharePoint Framework supports Node version 8.x.

    ![](/media/2019-04-12-spfx-ci-cd-i/06.png)


## Restore npm Packages

SharePoint Framework solution uses third party npm packages. We need to restore those before starting the build process.

1. Add **npm** task.
2. Verify command is set to install.

![](/media/2019-04-12-spfx-ci-cd-i/07.png)


## Build the Solution

Build the SPFx solution to minify the required assets to upload to CDN.

1. Add **gulp** task.
2. Set Gulp file path to gulpfile.js
3. Set Gulp task as ```bundle```.
4. Set Gulp arguments to ```--ship```.

![](/media/2019-04-12-spfx-ci-cd-i/08.png)


## Package the Solution

The next step is to combine the assets into a package.

1. Add gulp task.
2. Set Gulp file path to gulpfile.js
3. Set Gulp task as ```package-solution```.
4. Set Gulp arguments to ```--ship```.

![](/media/2019-04-12-spfx-ci-cd-i/09.png)


## Prepare the Artifacts

Azure DevOps build does not retain any files. The .sppkg file created from above step needs to be copied to staging directory to be published to release pipeline.

1. Add **Copy Files** task
2. Set **Source Folder** to ```$(Build.Repository.LocalPath)/sharepoint/solution```
3. Set **Contents** to ```*.sppkg```
4. Set target folder to ```$(Build.ArtifactStagingDirectory)/drop````

![](/media/2019-04-12-spfx-ci-cd-i/10.png)


## Publish the Artifacts

Instruct Azure DevOps to keep the files after build execution.

1. Add **Publish Build Artifacts** task.
2. Set **Path to publish** to ```$(Build.ArtifactStagingDirectory)/drop```
3. Set **Artifact name** to **drop**.

![](/media/2019-04-12-spfx-ci-cd-i/11.png)


## Summary

Continuous Integration helps to automate the build process when a solution being worked on by a team is undergoing continuous changes. Azure DevOps helps to automate SPFx solution builds.

This content was originally posted [here](https://www.c-sharpcorner.com/article/implement-ci-cd-for-sharepoint-framework-part-1/).
