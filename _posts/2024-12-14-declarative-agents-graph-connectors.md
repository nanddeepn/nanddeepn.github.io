---
title: "Adding Graph Connector as a Data Source to a Microsoft Copilot Declarative Agent"
date: "2024-12-14"
share: true
header:
  image: media/2024-12-14-declarative-agents-graph-connectors/06.png
  teaser: media/2024-12-14-declarative-agents-graph-connectors/06.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - December 2024
last_modified_at: 2024-12-14T00:00:00-00:00
---
## Overview

Declarative agents in Microsoft Copilot are designed to simplify workflows and provide contextually relevant information to users. Integrating a Graph Connector as a data source into a Microsoft Copilot Declarative Agent enhances its capabilities by providing seamless access to external and enterprise data.

This article guides you through the process of adding a Graph Connector source to a Microsoft Copilot agent.

## Purpose of Graph Connector

[Microsoft Graph Connectors](/posts/2023-12-14-ms-graph-connector-overview/) extend the reach of Microsoft Search by allowing organizations to index content from various third-party data sources. This indexed data can then be surfaced within Microsoft 365 apps, Teams, and, most importantly, Copilot, providing a unified and efficient search and retrieval experience.

The purpose of a Graph Connector is to bridge the gap between Microsoft 365 and external data sources. Many organizations rely on external systems beyond Microsoft 365 ecosystem to perform their operations, For e.g., ServiceNow to track incidents and tasks, Workday for HR and Finance operations, cloud-based applications, and more. Without proper integration, this data remains siloed, leading to inefficiencies in retrieving critical information.

Graph Connectors enable organizations to:
- Index content from external sources and make it discoverable within Microsoft 365.
- Ensure that all relevant data is accessible through a single interface.
- Maintain security and compliance by respecting the permissions defined in the source systems.
- Provide consistent and unified search results, improving productivity and collaboration.

By connecting these external data sources to Microsoft Graph, Graph Connectors ensure that Copilot has access to a wider pool of information, enabling it to answer user queries more comprehensively and accurately.

## Use Cases

Below are few use cases for using Graph Connectors as data source for a declarative agent:

- **Customer Support:** A declarative agent integrated with a Graph Connector can access customer data stored in CRM systems, ticketing platforms, and knowledge bases. This enables support agents to quickly retrieve customer details, history, and resolutions to similar issues, reducing response times.

- **Employee Onboarding:** An HR declarative agent can access data from an employee management system, policy documents, and training platforms via Graph Connectors. New hires can receive personalized onboarding experiences by interacting with Copilot.

- **Sales Enablement:** A sales-focused declarative agent can retrieve account details, sales metrics, and deal histories from CRM platforms and document repositories. This equips sales teams with relevant insights during customer interactions.

- **Research and Development:** A declarative agent can pull technical documentation, research papers, and project data from file shares and knowledge management systems, enabling R&D teams to accelerate innovation.

- **Compliance and Auditing:** Compliance teams can use Copilot to query policies, audit records, and regulatory guidelines stored in disparate systems via Graph Connectors, ensuring thorough and accurate audits.


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

    ![](/media/2024-12-14-declarative-agents-graph-connectors/01.png)

5. Select to create a new **Declarative Agent**.

    ![](/media/2024-12-14-declarative-agents-graph-connectors/02.png)

6. Select the option **No plugin** to create a declarative Copilot only.

    ![](/media/2024-12-14-declarative-agents-graph-connectors/03.png)

7. Name the Copilot as **RESTCountries**.


## Declarative agent manifest

We will build an agent based on the Graph connector implementation of [Ingest REST Countries content using PowerShell](https://adoption.microsoft.com/en-us/sample-solution-gallery/sample/pnp-graph-connector-json-restcountries/)

**Get connector id**:

Follow the below steps to get the id of Graph connector:
1. Open [Microsoft 365 Admin portal](https://admin.microsoft.com/).
2. Click **Settings** > **Search & Intelligence**.
3. Click **Data sources**.
4. Click the connector.
5. Note down the connector id (For e.g., `restcountries`).

    ![](/media/2024-12-14-declarative-agents-graph-connectors/04.png)

**Update configuration**:

Declarative agents require schema definitions to determine their operational scope. Update the schema file to restrict the data source to specify the Graph connector id.

Update the file at `appPackage\declarativeAgent.json` to define your Copilot by adding add an `capabilities` element.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/copilot/declarative-agent/v1.2/schema.json",
    "version": "v1.2",
    "name": "RESTCountries",
    "description": "This is a declarative agent designed to assist with searching for country information.",
    "instructions": "$[file('instruction.txt')]",
    "capabilities": [
        {
            "name": "GraphConnectors",
            "connections": [
                {
                    "connection_id": "restcountries"
                }
            ]
        }
    ]
}
```

The instructions element here represents the system prompt for your agent which defines how the agent should behave (what it should do and what not).

Add below instructions to the file `appPackage\instruction.txt`:

```
You are a country information specialist named "Country Explorer" that helps users find details about countries.
You are knowledgeable and approachable.
You always greet users warmly and introduce yourself with enthusiasm.
You enjoy using emojis to make interactions engaging and fun.
```


## Test the Copilot

Follow the below steps to test the Copilot

1. In the VS Code, open Teams Toolkit extension.
2. Make sure you are logged-in to Microsoft 365 tenant.
3. Click **Provision**. As the declarative Copilot does not include any code, you can ignore the Deploy and Publish options.

    ![](/media/2024-12-14-declarative-agents-graph-connectors/05.png)

4. Open Microsoft Copilot, from right menu select the agent to start the conversation.

    ![](/media/2024-12-14-declarative-agents-graph-connectors/06.png)


## Summary

Integrating Graph Connectors as a data source for Microsoft Copilot Declarative Agents unlocks the full potential of enterprise data. By bridging silos and enabling unified access to information, Graph Connectors enhance Copilot's contextual understanding, improve productivity, and provide secure and personalized responses.

Organizations looking to implement this integration should evaluate their existing data repositories, identify key use cases, and leverage Microsoft Graph Connectors to ensure their Copilot Declarative Agents deliver maximum value to their users.


## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/RESTCountries](https://github.com/nanddeepn/code-samples/tree/master/Copilot/DeclarativeCopilots/RESTCountries)


## References

- [Declarative agents for Microsoft 365 Copilot overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent?WT.mc_id=M365-MVP-5003693)
- [Create declarative agents using Teams Toolkit](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-declarative-agents?WT.mc_id=M365-MVP-5003693)
