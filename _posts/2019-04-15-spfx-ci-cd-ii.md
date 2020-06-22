---
title: "Implement CI / CD for SharePoint Framework - Part II"
date: "2019-04-15"
---

## Overview

CI and CD are common acronyms in modern software development practices. It helps developers to deliver the product faster with transparency. In the [previous article](https://nanddeepnachanblogs.com/2019/04/spfx-ci-cd-i/), we learned the CI, CD concepts and set up the build process to achieve Continuous Integration.

## Continuous Deployment

Continuous Deployment (CD) takes package from build and deploys it to designated environment. Successful and failed deployments can be tracked by developers.

Continuous Deployment with SharePoint Framework involves below steps

1. Create the Release Definition
2. Link the Build Artifact
3. Create the Environment
4. Install NodeJS
5. Install Office 365 CLI
6. Connect to App Catalog
7. Add Solution Package to App Catalog
8. Deploy the App
9. Set Environment Variables

## Create the Release Definition

1. From left navigation, click Pipelines > Releases.
2. Click “New pipeline”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-577.png)

1. Create a new release definition with empty template.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-578.png)

## Link the Build Artifact

1. Click “Add an artifact”
2. Under “Source (build pipeline)”, select the previously created build definition
3. Note down “Source alias” for future use.
4. Click Add.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-579.png)

## Create the Environment

Define an environment to deploy the build artifacts.

1. Under Stages, click “Stage 1”.
2. Name your environment.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-580.png)

## Install NodeJS

1. Under environment, click “1 job, 0 task”.
2. The task configuration window will appear, same as in build definition.
3. On the default agent, click + sign.
4. Search for “Node”.
5. Add Node.js tool installer.
6. Specify the version as 8.x, since SharePoint Framework supports Node version 8.x.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-581.png)

## Install Office 365 CLI

Office 365 Common Language Interface (CLI) is an open source project from OfficeDev PnP Community.

1. Add npm task
2. Under “Command”, select custom
3. In the “Command and Arguments”, type install -g @pnp/office365-cli

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-582.png)

## Connect to App Catalog

We need to authenticate against App Catalog of our tenant.

1. Add “Command Line” task.
2. In the “Script” field, type in below command

o365 spo login https://$(tenant).sharepoint.com/$(catalogsite) --authType password --userName $(username) --password $(password)

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-583.png)

## Add Solution Package to App Catalog

Now, we need to upload the solution package to app catalog.

1. Add “Command Line” task.
2. In the “Script” field, type in below command.

o365 spo app add -p $(System.DefaultWorkingDirectory)/SPFx-CI/drop/SharePoint/solution/spfx-ci.sppkg --overwrite

Where SPFx-CI is source alias setup during “Link the Build Artifact” step.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-584.png)

## Deploy the App

We need to deploy the app to App Catalog to make it available to all site collections within the tenant.

1. Add “Command Line” task.
2. In the “Script” field, type in below command.

o365 spo app deploy --name spfx-ci.sppkg --appCatalogUrl https://$(tenant).sharepoint.com/$(catalogsite)

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-585.png)

## Set Environment Variables

We need to define the process variables used in earlier steps.

1. Click Variables tab.
2. Under Pipeline variables, add below variables.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-586.png)

## Test Continuous Deployment

Follow below steps to test the continuous deployment.

1. Under Pipelines click Builds.
2. Click Queue.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-587.png)

1. Select the branch. Click Queue.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-588.png)

1. Once the build runs, see the logs and fix the issues if any.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-589.png)

1. Once the build is successful, auto deployment will trigger (if configured). Once the deployment runs, see the logs and fix the issues if any.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-590.png)

1. Verify the app (.sppkg) is deployed to SharePoint app catalog.

## Setup Trigger for Deployment

We can manually trigger the deployment. However the ideal scenario is to trigger the deployment on successful build completion. Follow below steps to setup trigger.

1. From the left menu, click Pipelines > Releases.
2. Open our release pipeline, we created earlier.
3. Click Edit.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-591.png)

1. Under Artifacts, click Continuous deployment trigger.
2. Enable the continuous deployment.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-592.png)

1. Click Save.

## Summary

CI/CD helps to automate the build and deployment process when a solution being worked on by the team is undergoing continuous changes. Azure DevOps helps to automate SPFx solution builds and deployment.

This content was originally posted [here](https://www.c-sharpcorner.com/article/implement-cicd-for-sharepoint-framework-part-two/).
