---
title: "Office 365 CLI: Upgrade SPFx Solutions"
date: "2019-07-11"
---

## Overview

SharePoint Framework is ever growing. With each of its version, it releases newer functionality. Upgrading the SPFx solutions each time is a challenge. The manual efforts to upgrade may or may not work. In some cases, we might have to rewrite the code in a newer version. However, Office 365 CLI handles this scenario gracefully and provides a flexible option for developers to upgrade to any new version of SPFx.

In this article, we will explore how Office 365 CLI helps to upgrade the SPFx solution to a newer version.

## Office 365 CLI Overview

Office 365 CLI is an open source project from PnP (Pattern and Practices). It helps to manage Office 365 tenant and SPFx projects on any platform (including Windows, Linux, macOS, etc.). Office 365 CLI is available as a distributable NPM package. Use below command to install Office 365 CLI.

npm i -g @pnp/office365-cli

Upgrade SPFx Solution to Newer Version

We will take an example of SPFx solution (React based Carousel) developed earlier with SPFx drop version of 1.7.1. The previous SPFx solution can be downloaded from here.

During this article, we will upgrade this solution to the latest SPFx version 1.8.2 (latest available at the time of writing this article).

Follow the below steps to upgrade the SPFx solution to the latest version.

1. Open a command prompt.
2. Execute the below command to update the Office 365 CLI to the latest version.

npm install -g @pnp/office365-cli@latest

1. On the command prompt, navigate to the SPFx solution directory and execute below command.

o365 spfx project upgrade --output md > report.md

This will generate a markdown file with the list of changes to be applied to SPFx solution to upgrade it to the latest version. Please note that Office 365 CLI does not apply any changes to SPFx solution. The changes need to be applied manually.

1. On the command prompt, type below command to open the SPFx solution in the code editor of your choice (e.g. Visual Studio Code).

code .

1. Open report.md file in Visual Studio Code.
2. Click “Open Preview to the Side” or shortcut (CTRL + K V) to open markdown file in a preview mode.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-538.png)

1. The markdown file will open in a preview mode with easy to follow instructions.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-539.png)

1. Follow the instructions as directed in the markdown file.

The markdown file contains a set of npm commands to run and explanation of need of each package upgrade. Scroll down to the “Summary” section in markdown file to see the consolidated npm commands to run and manual changes needed for an individual file.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-540.png)

## Upgrade SPFx Solution to Specific Version

In some scenarios, we might not want to upgrade to the latest available SPFx version but maybe to some intermediate version. Office 365 CLI supports this scenario. Use below command to upgrade to any specific version (e.g. 1.8.0 in this case).

o365 spfx project upgrade --toVersion 1.8.0 --output md > report.md

Downgrade SPFx Solution?

No, Office 365 CLI does not allow downgrading the SPFx solution. For example, if your SPFx solution is created using version 1.7.1 then running below command will not downgrade the SPFx solution to 1.5.0.

o365 spfx project upgrade --toVersion 1.5.0 --output md > report.md

The report will show the message “Error: You cannot downgrade a project”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-541.png)

## Summary

Office 365 CLI is an open source project from PnP (Pattern and Practices) which helps to manage Office 365 tenant and SPFx projects on any platform. Office 365 CLI does not apply any changes to SPFx solution but provides the guidance for the changes needs to be applied manually in order to upgrade the solution. Please note that you cannot downgrade SPFx project.

This content was originally posted [here](https://www.c-sharpcorner.com/article/upgrade-spfx-solutions-with-office-365-cli/).
