---
title: "Building Declarative copilots for Microsoft 365 with Teams Toolkit"
date: "2024-09-10"
share: true
header:
  image: media/2024-09-10-declarative-copilots-m365/01.png
  teaser: media/2024-09-10-declarative-copilots-m365/01.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - September 2024
last_modified_at: 2024-09-10T00:00:00-00:00
---
## Overview

A declarative copilot lets you create a customized copilot for Microsoft 365 by simply stating what you want it to do. These copilots use the same technology as Microsoft Copilot, allowing you to adapt it to fit your business needs. By using declarative copilots, you can improve collaboration, boost productivity, and simplify workflows. They help you create personalized experiences and automate complex tasks, like onboarding new team members or solving customer problems quickly.

In this article, we will explore the meaning of declarative copilots and how to create them.

## What Are Declarative Copilots?

Declarative Copilots are a new approach to configuring and deploying AI copilots across Microsoft 365 applications. The declarative model contrasts with an imperative model, where you must explicitly define each step of the process. Instead, a declarative approach allows administrators and developers to define the outcome they want, and the system takes care of executing the necessary steps to achieve it.

In essence, with declarative copilots, you focus on defining what the solution should do, not how to build it. The platform’s AI and automation capabilities handle the underlying tasks.

**The Benefits of Declarative Copilots:**

- **Ease of Use:** A declarative approach simplifies the development process, making it more accessible to non-developers and enabling users to define copilot behaviors using high-level instructions. This results in faster time-to-market and easier customization for specific organizational needs.
- **Scalability:** Declarative copilots can easily be scaled with the organization process evolvement, allowing changes to be made by simply updating the desired outcomes without needing to rewrite or reconfigure complex code.
- **Focus on Business Logic:** Since the developer’s attention is drawn away from the implementation details, they can focus more on aligning copilots with business logic and specific use cases.

## Common use case with Declarative Copilot

Here is a simple use case where declarative copilot can be effective.

**Problem statement**

Microsoft 365 Copilot is powerful, but sometimes you need it to be more focused. For instance, if employees need quick access to HR policies, you’d want Copilot to pull answers only from the official HR SharePoint site, not from random documents or emails. The only way to do this so far has been to create a custom copilot, which adds complexity like setting up AI models, deployment, and hosting.

**Solution**

Declarative copilot simplifies this process by allowing you to easily specify where Copilot should get its information, like the HR SharePoint site in the example. Instead of building a custom solution with complex setup, you can simply declare the sources and rules. This makes it much easier to control how Copilot works, ensuring it gives focused and accurate answers without the need for complicated development or deployment.

## Create declarative copilot with Teams Toolkit

Declarative copilot does not have any code as they use the power of Copilot for Microsoft 365 behind the scenes. The configuration happens through JSON based files.

**Pre-requisites:**

You need to have below things installed:

1. Visual Studio Code
2. Teams Toolkit extension (Pre-release version)
3. Follow the instructions mentioned [here](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/ttk-declarative-copilot-getting-started?WT.mc_id=M365-MVP-5003693)

Follow below steps to create declarative copilot with Teams Toolkit:

1. Open Visual Studio Code.
2. From the left pane, click **Teams Toolkit**.
3. Click **Create a New App**.
4. Select **Copilot Extension**.

    ![](/media/2024-09-10-declarative-copilots-m365/01.png)

5. Select to create a new **Declarative Copilot**.

    ![](/media/2024-09-10-declarative-copilots-m365/02.png)

6. Select the option **No plugin** to create a declarative Copilot only.

    ![](/media/2024-09-10-declarative-copilots-m365/03.png)

7. Name the Copilot as **GeographyKnowledgeCopilot**.

The solution will be created as follows:

![](/media/2024-09-10-declarative-copilots-m365/04.png)

Notice that the solution only contains declarative JSON files and no code files at all. Below are the important files from the solution:

- **App manifest (appPackage\\manifest.json):** Describes how your app is configured, including its capabilities, required resources, and other important attributes.
- **App icons (appPackage\\outline.png and appPackage\\color.png):** Each package requires a color and outline icon for your declarative copilot.
- **Declarative copilot manifest (appPackage\\declarativeCopilot.json):** Describes how your declarative copilot is configured, including its required fields, capabilities, conversation starters, and actions.

**App manifest**

Notice that the file appPackage\\manifest.json has an element copilotExtensions\\ declarativeCopilots which defines your declarative copilot. The newly added copilotExtensions element supports the copilot extensibility.

![](/media/2024-09-10-declarative-copilots-m365/05.png)

**Declarative copilot manifest**

Update the file at `appPackage\declarativeCopilot.json` to define your Copilot.

```json
{
    "$schema": "https://aka.ms/json-schemas/copilot-extensions/v1.0/declarative-copilot.schema.json",
    "name": "Geography Knowledge Game (Declarative Copilot)",
    "description": "This is a Geography Knowledge Game Declarative Copilot",
    "instructions": "You are a Geography Knowledge Test declarative copilot. Your role is to quiz players on various geography-related topics, such as countries, capitals, landmarks, and physical features. Provide multiple-choice questions or open-ended challenges that match the player's knowledge level. Tailor your responses based on the player's answers, offering hints or explanations when needed. Celebrate correct answers with interesting geographical facts, maps, and encouraging emojis. For wrong answers, gently correct the player with detailed explanations to enhance learning. Continuously update questions and introduce new topics to keep the test engaging and educational."
}
```

The instructions element here represents the system prompt for your Copilot which defines how the Copilot should behave (what it should do and what not).

## Test the Copilot

Follow the below steps to test the Copilot

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2024-09-10-declarative-copilots-m365/06.png)

At this point, you might receive below error, as the Declarative Copilot is in private preview:

```
Unable to execute action teamsApp/extendToM365. Error message: undefined
Failed to Execute lifecycle provision due to failed action: teamsApp/extendToM365.
```

## Summary

Declarative copilots for Microsoft 365 are a big step forward in how businesses and developers use AI. Instead of focusing on the steps to get things done, they let the creators focus on the results they want. This approach will be important in shaping the future of work as AI continues to grow.

## References

- [Declarative copilots for Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-copilot?WT.mc_id=M365-MVP-5003693)
- [Build a declarative copilot for Copilot for Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/ttk-declarative-copilot-getting-started?WT.mc_id=M365-MVP-5003693)
