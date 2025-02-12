---
title: "Buidling Declarative Agents with Grapic Art Capabilities"
date: "2025-02-12"
share: true
header:
  image: media/2025-02-12-declarative-agents-graphic-art/04.png
  teaser: media/2025-02-12-declarative-agents-graphic-art/04.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2025"
  - February 2025
last_modified_at: 2025-02-12T00:00:00-00:00
---
## Overview

Declarative Agents are an innovative way of developing AI-driven solutions that interact with users naturally through conversations. Unlike traditional programming approaches, Declarative Agents rely on configurations and prompts to define behavior, making them more accessible to developers and business users alike. Declarative Agents with GraphicArt capabilities open a new world of possibilities for image generation for the agent.

This article guides you through the process of creating declarative agents for generating images.


## Use Cases for Agents with Image Generation Capability

Agents with image generation capabilities can be deployed in various industries and scenarios. Here are some prominent use cases:

**E-Learning and Training**
- Scenario: A student asks an AI tutor to explain the solar system.
- Output: The agent generates a diagram showing planets and their orbits, enhancing the learning experience.

**Marketing and Branding**
- Scenario: A marketer asks the agent to generate promotional material for a new product launch.
- Output: The agent produces visually appealing posters, banners, or social media graphics.

**Design and Creativity**
- Scenario: A graphic designer asks the agent for ideas for a logo or a banner.
- Output: The agent generates mock-ups or conceptual designs based on user inputs.


## Create declarative agent with Teams Toolkit

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
4. Select **Declarative Agent**.

    ![](/media/2025-02-12-declarative-agents-graphic-art/01.png)

5. Select **No Action** option to create a declarative agent only.

    ![](/media/2025-02-12-declarative-agents-graphic-art/02.png)

6.	Select the folder location.
7.	Name the agent as **PicArtAgent**.

## Declarative agent manifest

Update the file at `appPackage\declarativeAgent.json` to define your Copilot.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "PicArt Agent",
    "description": "This is a declarative agent to help transforms your ideas into dynamic visuals",
    "instructions": "$[file('instruction.txt')]"
}
```

The instructions element here represents the system prompt for your agent which defines how the agent should behave (what it should do and what not).

Add below instructions to the file `appPackage\instruction.txt`:

```
You are a graphic specialist, designed to generate visually engaging and dynamic graphics based on user inputs. 
Your role is to transform complex ideas or scenarios into clear, compelling visual representations, such as charts, diagrams, infographics, or customized images. 
Always ensure your outputs are accurate, visually appealing, and aligned with the user's requirements, enhancing their understanding and decision-making.
```

Open `appPackage\declarativeAgent.json` and add an element `capabilities`:

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "PicArt Agent",
    "description": "This is a declarative agent to help transforms your ideas into dynamic visuals",
    "instructions": "$[file('instruction.txt')]",
    "capabilities": [
        {
            "name": "GraphicArt"
        }
    ]
}
```


## Test the Copilot

Follow the below steps to test the Copilot:

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2025-02-12-declarative-agents-graphic-art/03.png)

4. Open Microsoft Copilot, from right menu select the agent to start the conversation.

    ![](/media/2025-02-12-declarative-agents-graphic-art/04.png)


## Summary

Agents with image generation capabilities go beyond traditional chatbots by adding visual intelligence to their functionality. They are versatile, impactful, and applicable across industries, ensuring that users not only receive information but also understand it better.


## References

- [Declarative agents for Microsoft 365 Copilot overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent?WT.mc_id=M365-MVP-5003693)
- [Create declarative agents using Teams Toolkit](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)
