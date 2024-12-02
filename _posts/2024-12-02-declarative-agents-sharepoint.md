---
title: "Adding a SharePoint or OneDrive Source to a Microsoft Copilot Declarative Agent"
date: "2024-12-02"
share: true
header:
  image: media/2024-12-02-declarative-agents-sharepoint/05.png
  teaser: media/2024-12-02-declarative-agents-sharepoint/05.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - December 2024
last_modified_at: 2024-12-02T00:00:00-00:00
---
## Overview

When building a Microsoft Copilot agent, one of the most frequent and essential requirements is scoping the data source. For effective and precise outcomes, organizations often prefer that their Copilot accesses a specific repository of knowledge instead of having unrestricted access to every piece of content in their Microsoft 365 tenant. For instance, an enterprise might want the Copilot to only answer queries based on content stored in a designated SharePoint site or OneDrive location. 

This article guides you through the process of adding a SharePoint or OneDrive source to a Microsoft Copilot agent.


## Why Scope the Data Source?

By default, Microsoft Copilot can utilize a vast range of content stored across Microsoft 365, such as emails, Teams messages, SharePoint sites, and OneDrive files. While this flexibility is powerful, there are scenarios where limiting the knowledge base to a specific source ensures:

- Relevance: The agent provides answers tailored to a focused context, avoiding irrelevant information.
- Compliance: Sensitive or unrelated information outside the specified scope is not accessed.
- Efficiency: Reduced search space leads to faster and more accurate responses.
- Governance: IT administrators and stakeholders maintain control over what data the Copilot interacts with.


## Business Scenario

The HR team at Contoso regularly reviews a high volume of resumes stored in a SharePoint document library. These resumes, submitted by global candidates, are written in various languages, making it challenging for recruiters to efficiently find and evaluate candidates who meet specific job criteria.

I am using the fictitious resumes, available as part of [Build your own copilot using Teams AI library and .NET](https://github.com/microsoft/community-content/blob/main/SeasonOfAI-S2-Copilots/build-your-own-copilot-teams-ai-library.md). These resumes are uploaded to a document library "Resumes" under a SharePoint site "https://contoso.sharepoint.com/sites/CopilotDemo".

**Challenge:**

- Manually searching resumes by keywords, qualifications, and experience is time-consuming.
- Language barriers require additional effort to translate and understand resumes in multiple languages.
- Traditional search lacks contextual understanding, often resulting in irrelevant matches.

**Solution: Copilot Agent Integration:** With Microsoft Copilot, the HR team can perform natural language searches directly within their Microsoft Teams environment, accessing and analyzing resumes stored in SharePoint.

**Use Case:**
The recruiter types a query: "Find resumes of candidates with 5+ years of experience in data analysis who speak Spanish or French."


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

    ![](/media/2024-12-02-declarative-agents-sharepoint/01.png)

5. Select to create a new **Declarative Agent**.

    ![](/media/2024-12-02-declarative-agents-sharepoint/02.png)

6. Select the option **No plugin** to create a declarative Copilot only.

    ![](/media/2024-12-02-declarative-agents-sharepoint/03.png)

7. Name the Copilot as **SPOResumeAgent**.


## Declarative agent manifest

Update the file at `appPackage\declarativeAgent.json` to define your Copilot.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "Resume Agent",
    "description": "This is a declarative agent to help search matching resumes",
    "instructions": "$[file('instruction.txt')]"
}
```

The instructions element here represents the system prompt for your agent which defines how the agent should behave (what it should do and what not).

Add below instructions to the file `appPackage\instruction.txt`:

```
You are a career specialist named "Career Genie" that helps Human Resources team for writing job posts.
You are friendly and professional.
You always greet users with excitement and introduce yourself first.
You like using emojis where appropriate.
```

## Scoping data source to SharePoint or OneDrive

Declarative agents require schema definitions to determine their operational scope. Update the schema file to restrict the data source to specify the SharePoint site or OneDrive folder URL.

Open `appPackage\declarativeAgent.json` and add an element `capabilities`:

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "Resume Agent",
    "description": "This is a declarative agent to help search matching resumes",
    "instructions": "$[file('instruction.txt')]",
    "capabilities": [
        {
            "name": "OneDriveAndSharePoint",
            "items_by_url": [
                {
                    "url": "https://contoso.sharepoint.com/sites/CopilotDemo/Resumes/"
                }
            ]
        }
    ]
}
```


## Test the Copilot

Follow the below steps to test the Copilot

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2024-12-02-declarative-agents-sharepoint/04.png)

4. Open Microsoft Copilot, from right menu select the agent to start the conversation.

    ![](/media/2024-12-02-declarative-agents-sharepoint/05.png)


## Summary

Modern HR teams face challenges in efficiently searching and analyzing large volumes of multi-lingual resumes stored in SharePoint document libraries. Microsoft Copilot provides a powerful solution by enabling natural language queries, AI-driven multilingual analysis, and contextual resume matching.

With Copilot, recruiters can quickly find candidates that meet specific job requirements, overcoming language barriers and saving significant time. The tool delivers structured insights, such as ranked candidate lists with key details, directly within Microsoft Teams, enhancing decision-making and accelerating the hiring process.


## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/SPOResumeAgent](https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/SPOResumeAgent)


## References

- [Declarative agents for Microsoft 365 Copilot overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent?WT.mc_id=M365-MVP-5003693)
- [Create declarative agents using Teams Toolkit](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)
