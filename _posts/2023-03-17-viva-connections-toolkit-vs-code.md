---
title: "Exploring Viva Connections Toolkit for Visual Studio Code"
date: "2023-03-17"
share: true
header:
  image: media/2023-03-17-viva-connections-toolkit-vs-code/01.png
  teaser: media/2023-03-17-viva-connections-toolkit-vs-code/01.png
categories:
  - Microsoft Viva
  - SharePoint Framework
tags:
  - "2023"
  - March 2023
last_modified_at: 2023-03-17T00:00:00-00:00
---
## Introduction

Viva Connections Toolkit extension is now available in preview in Visual Studio Code to make developers' life easy to manage the SharePoint Framework (SPFx) solutions.

In this article, I will share my experience exploring Viva Connections Toolkit.

## Viva Connections Toolkit

It is a Visual Studio Code extension that can help developers to create and manage Viva Connections solutions. However just not Viva Connections solution but technically as a developer, you can manage all types of SPFx solutions.

Below are a few of the key points (out of documentation available):

- It is an abstraction layer on top of the SPFx Yeoman generator and CLI for Microsoft 365.
- Validate the environment.
- Install dependencies.
- Create new solutions.
- Perform gulp tasks.
- Upgrade the solution using CLI for Microsoft 365

## Install Viva Connections Toolkit

It is an extension available in Visual Studio code. Follow the below steps to install it.

1. Open Visual Studio Code.
2. From the left menu, click Extensions.
3. Search for "viva connections toolkit".
4. Click install.

    ![](/media/2023-03-17-viva-connections-toolkit-vs-code/01.png)

After successful installation, you should see the extension appearing in the left menu.

![](/media/2023-03-17-viva-connections-toolkit-vs-code/02.png)


## Creating a new solution

Viva Connections Toolkit is smart enough to detect the folder location where the Visual Studio code is opened. When it is opened from an empty folder, it will show options to create an SPFx project.

![](/media/2023-03-17-viva-connections-toolkit-vs-code/03.png)

**Create a Viva Connections project**

This option will help the developer to create an ACE (Adaptive Card Extension) project by following a wizard.

![](/media/2023-03-17-viva-connections-toolkit-vs-code/04.png)

**Create from a scenario**

This option will provide a list of pre-defined scenarios already built. The wizard will replace the placeholders in the solution with your project name.

![](/media/2023-03-17-viva-connections-toolkit-vs-code/05.png)

**View samples**

This option will help you to browse through brilliant community samples submitted by the community contributors to get inspiration from.

**Open folder**

This option helps the developer to open an existing SPFx solution.

**Check dependencies**

This option will help the developer to check the readiness of your system to start developing SPFx solutions. The check includes the installation of node js, yeoman generator, gulp, npm modules, and other dependencies.

**Install dependencies**

This option will prepare your machine to start developing SPFx solutions by installing all the dependencies.


## Tasks and Actions support for managing your project better

Once you create a project or open an existing project, the Viva Connections Toolkit extension provides you with various tasks and actions to ease the job of the developer to bundle, package, and serve the solution.

The actions like the **upgrade solution** should help you upgrade your SPFx solution to a newer version, a functionality powered by CLI for Microsoft 365.

![](/media/2023-03-17-viva-connections-toolkit-vs-code/06.png)


## Summary

Viva Connections Toolkit extension is now available in preview in Visual Studio Code to make developers' life easy to manage the SharePoint Framework (SPFx) solutions.


## References

- [Viva Connections Toolkit for Visual Studio Code now in preview](https://devblogs.microsoft.com/microsoft365dev/preview-of-viva-connections-toolkit-for-vs-code/)
- [Viva Connections Toolkit](https://marketplace.visualstudio.com/items?itemName=m365pnp.viva-connections-toolkit)
- [vscode-viva](https://github.com/pnp/vscode-viva)
