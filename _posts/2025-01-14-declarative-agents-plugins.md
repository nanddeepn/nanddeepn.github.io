---
title: "Building Declarative Agents with Plugins"
date: "2025-01-14"
share: true
header:
  image: media/2025-01-14-declarative-agents-plugins/08.png
  teaser: media/2025-01-14-declarative-agents-plugins/08.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2025"
  - January 2025
last_modified_at: 2025-01-14T00:00:00-00:00
---
## Overview

Declarative agents enable developers to build intelligent, context-aware applications by defining their intent and behavior declaratively rather than imperatively. If you already have a web API, you can leverage it to create declarative agents by defining intents and actions that interact with your API’s endpoints. Declarative agents built on web APIs can dynamically process user requests, fetch or update data, and provide real-time responses by integrating with existing systems. This approach not only saves development time but also enhances the capabilities of your API by making it accessible through conversational interfaces.

This article guides you through the process of creating declarative agents on existing APIs.

## Declarative agents with Plugins

The Teams Toolkit had introduced a simplified way to create declarative agents using a new extensibility feature called API plugins. Previously, creating plugins required a pro-code approach through Message Extensions plugins. While these plugins are highly flexible, they involve writing code to handle tasks such as receiving a search query from Copilot, calling a third-party platform to retrieve data, and generating an Adaptive Card to display the results.

API plugins, on the other hand, simplify this process by focusing on direct API integration. Instead of writing code, API plugins leverage OpenAPI, a standard format for describing REST APIs using JSON or YAML. OpenAPI outlines the API's endpoints, input and output parameters, authentication methods, and other details in a technology-agnostic manner. This means that whether the API is hosted on Azure using .NET or on AWS with Node.js, the implementation details are irrelevant to the plugin.

Additionally, building and testing APIs is straightforward with tools like .NET Web APIs or Azure Functions, which come with built-in OpenAPI support. By simply adding specific attributes to your ASP.NET Web API or Azure Functions, you can automatically generate an OpenAPI definition, making integration with the Teams Toolkit seamless and efficient.

## API Plugin

For the demonstration purpose of this article, I am going to use the Petstore Swagger available at [https://petstore.swagger.io/](https://petstore.swagger.io/)

It is an example API and documentation provided by Swagger, a set of tools for working with OpenAPI specifications. Swagger Petstore serves as a demo for understanding how APIs work, and it showcases how OpenAPI specifications can be used to describe RESTful APIs.

## Create declarative agent with Teams Toolkit

Declarative agent does not have any code as they use the power of Copilot for Microsoft 365 behind the scenes. The configuration happens through JSON based files.
Declarative agent does not have any code as they use the power of Copilot for Microsoft 365 behind the scenes. The configuration happens through JSON based files.

**Pre-requisites:**

You need to have below things installed:

1. Visual Studio Code
2. Teams Toolkit extension (Pre-release version)
3. Follow the instructions mentioned [here](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)

Follow below steps to create declarative agent with Teams Toolkit:

1. Open Visual Studio Code.
2. From the left pane, click **Teams Toolkit**.
3. Click **Create a New App**.
4. Select **Agent**.

    ![](/media/2025-01-14-declarative-agents-plugins/01.png)

5. Select to create a new **Declarative Agent**.

    ![](/media/2025-01-14-declarative-agents-plugins/02.png)

6. Select the option **Add an action** to create a declarative Copilot with an action.

    ![](/media/2025-01-14-declarative-agents-plugins/03.png)

7. Select the option **Start with an OpenAPI Description Document** to create an action from an existing API. 

    ![](/media/2025-01-14-declarative-agents-plugins/04.png)

8. Select the option **Enter OpenAPI Description Document Location**.

    ![](/media/2025-01-14-declarative-agents-plugins/05.png)

9. Enter OpenAPI Description Document URL as [https://petstore.swagger.io/v2/swagger.json](https://petstore.swagger.io/v2/swagger.json).

10. Select the operations Copilot can interact with.

    ![](/media/2025-01-14-declarative-agents-plugins/06.png)

11. Name the application as `PetstorePluginAgent`.

## Understand the solution

The solution mainly has below files:

1. `appPackage\apiSpecificationFile\openapi.json`: Local copy of the OpenAPI definition API.
2. `appPackage\ai-plugin.json`: It is like an upgraded version of the OpenAPI definition, tailored specifically for Copilot. It includes Copilot-specific details, like names and descriptions (used by the model to decide if the plugin should be used), and even adaptive cards to show as part of the response.
3. `appPackage\declarativeAgent.json`: Defines the actions and conversation starters.

## Test the Copilot

Follow the below steps to test the Copilot:

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. When asked, provide the key as `special-key`. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2025-01-14-declarative-agents-plugins/07.png)

4. Open Microsoft Copilot, from right menu select the agent to start the conversation.

    ![](/media/2025-01-14-declarative-agents-plugins/08.png)


## Summary

Building declarative agents with plugins using Teams Toolkit allows developers to create intelligent, context-aware solutions tailored to their organization's needs. By leveraging the power of Microsoft Teams Toolkit and OpenAI APIs, you can deliver impactful experiences that enhance collaboration and productivity.

## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/PetstorePluginAgent](https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/PetstorePluginAgent)


## References

- [Declarative agents for Microsoft 365 Copilot overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent?WT.mc_id=M365-MVP-5003693)
- [Create declarative agents using Teams Toolkit](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)
