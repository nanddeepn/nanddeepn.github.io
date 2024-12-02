---
title: "Building Declarative agents for Microsoft 365 with Teams Toolkit"
date: "2024-09-10"
share: true
header:
  image: media/2024-09-10-declarative-copilots-m365/07.png
  teaser: media/2024-09-10-declarative-copilots-m365/07.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - September 2024
last_modified_at: 2024-12-02T00:00:00-00:00
---
## Overview

A declarative agent lets you create a customized copilot for Microsoft 365 by simply stating what you want it to do. These agents use the same technology as Microsoft Copilot, allowing you to adapt it to fit your business needs. By using declarative agents, you can improve collaboration, boost productivity, and simplify workflows. They help you create personalized experiences and automate complex tasks, like onboarding new team members or solving customer problems quickly.

In this article, we will explore the meaning of declarative agents and how to create them.

## What Are Declarative Agent?

Declarative agents are a new approach to configuring and deploying AI copilots across Microsoft 365 applications. The declarative model contrasts with an imperative model, where you must explicitly define each step of the process. Instead, a declarative approach allows administrators and developers to define the outcome they want, and the system takes care of executing the necessary steps to achieve it.

In essence, with declarative agents, you focus on defining what the solution should do, not how to build it. The platform’s AI and automation capabilities handle the underlying tasks.

**The Benefits of Declarative Agents:**

- **Ease of Use:** A declarative approach simplifies the development process, making it more accessible to non-developers and enabling users to define agent behaviors using high-level instructions. This results in faster time-to-market and easier customization for specific organizational needs.
- **Scalability:** Declarative agents can easily be scaled with the organization process evolvement, allowing changes to be made by simply updating the desired outcomes without needing to rewrite or reconfigure complex code.
- **Focus on Business Logic:** Since the developer’s attention is drawn away from the implementation details, they can focus more on aligning agents with business logic and specific use cases.

## Common use case with Declarative Agent

Here is a simple use case where declarative agent can be effective.

**Problem statement**

Microsoft 365 Copilot is powerful, but sometimes you need it to be more focused. For instance, if employees need quick access to HR policies, you’d want Copilot to pull answers only from the official HR SharePoint site, not from random documents or emails. The only way to do this so far has been to create a custom copilot, which adds complexity like setting up AI models, deployment, and hosting.

**Solution**

Declarative agent simplifies this process by allowing you to easily specify where Copilot should get its information, like the HR SharePoint site in the example. Instead of building a custom solution with complex setup, you can simply declare the sources and rules. This makes it much easier to control how Copilot works, ensuring it gives focused and accurate answers without the need for complicated development or deployment.

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
4. Select **Agent**.

    ![](/media/2024-09-10-declarative-copilots-m365/01.png)

5. Select to create a new **Declarative Agent**.

    ![](/media/2024-09-10-declarative-copilots-m365/02.png)

6. Select the option **No plugin** to create a declarative Copilot only.

    ![](/media/2024-09-10-declarative-copilots-m365/03.png)

7. Name the Copilot as **GeographyKnowledgeAgent**.

The solution will be created as follows:

![](/media/2024-09-10-declarative-copilots-m365/04.png)

Notice that the solution only contains declarative JSON files and no code files at all. Below are the important files from the solution:

- **App manifest (appPackage\manifest.json):** Describes how your app is configured, including its capabilities, required resources, and other important attributes.
- **App icons (appPackage\outline.png and appPackage\color.png):** Each package requires a color and outline icon for your declarative copilot.
- **Declarative copilot manifest (appPackage\declarativeCopilot.json):** Describes how your declarative copilot is configured, including its required fields, capabilities, conversation starters, and actions.

**App manifest**

Notice that the file `appPackage\manifest.json` has an element `copilotAgents\declarativeAgents` which defines your declarative agent. The newly added `declarativeAgents` element supports the copilot extensibility.

![](/media/2024-09-10-declarative-copilots-m365/05.png)

**Declarative agent manifest**

Update the file at `appPackage\declarativeAgent.json` to define your Copilot.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "Geography Knowledge Game (Declarative Agent)",
    "description": "This is a Geography Knowledge Game Declarative Copilot",
    "instructions": "$[file('instruction.txt')]"
}
```

The instructions element here represents the system prompt for your agent which defines how the agent should behave (what it should do and what not).

Add below instructions to the file `appPackage\instruction.txt`:

```
You are a Geography Knowledge Test declarative copilot. Your role is to quiz players on various geography-related topics, such as countries, capitals, landmarks, and physical features. Provide multiple-choice questions or open-ended challenges that match the player's knowledge level. Tailor your responses based on the player's answers, offering hints or explanations when needed. Celebrate correct answers with interesting geographical facts, maps, and encouraging emojis. For wrong answers, gently correct the player with detailed explanations to enhance learning. Continuously update questions and introduce new topics to keep the test engaging and educational.
```

## Test the Copilot

Follow the below steps to test the Copilot

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2024-09-10-declarative-copilots-m365/06.png)

4. Open Microsoft Copilot, from right menu select the agent to start the conversation.

    ![](/media/2024-09-10-declarative-copilots-m365/07.png)


## Summary

Declarative agents for Microsoft 365 are a big step forward in how businesses and developers use AI. Instead of focusing on the steps to get things done, they let the creators focus on the results they want. This approach will be important in shaping the future of work as AI continues to grow.

## References

- [Declarative agents for Microsoft 365 Copilot overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent?WT.mc_id=M365-MVP-5003693)
- [Create declarative agents using Teams Toolkit](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)
