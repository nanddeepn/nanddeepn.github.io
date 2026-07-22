---
title: "Empowering Enterprise AI Agents with IQ and Microsoft Foundry"
date: "2026-07-05"
share: true
header:
  image: media/2026-07-05-ai-agents-microsoft-iq/01.png
  teaser: media/2026-07-05-ai-agents-microsoft-iq/01.png
categories:
  - Microsoft Foundry
  - AI
tags:
  - "2026"
  - July 2026
  - "AI 2026-27"
last_modified_at: 2026-07-05T00:00:00-00:00
---
## Introduction

As organizations move from AI assistants to autonomous AI agents, one challenge becomes increasingly important: **how can agents access the right information, at the right time, from trusted enterprise sources?**

Large Language Models (LLMs) are excellent at reasoning and generating responses, but they have no inherent understanding of an organization's people, documents, business data, or internal processes. Without enterprise context, even the most advanced models can produce incomplete or inaccurate answers.

Microsoft addresses this challenge with the **Microsoft IQ Platform**-a unified intelligence layer that brings together knowledge from Microsoft 365, business systems, enterprise documents, and the public web. Instead of forcing developers to integrate dozens of different data sources individually, Microsoft IQ provides a consistent way for AI agents to discover, retrieve, and reason over enterprise knowledge.

When combined with **Microsoft Foundry**, organizations can build AI agents that understand:

- Employees and collaboration
- Business operations and structured data
- Enterprise knowledge and documents
- Current information from the web

This article explains the Microsoft IQ Platform, its four intelligence pillars, and how it empowers enterprise AI agents built with Microsoft Foundry.

## Why Enterprise AI Needs Intelligence

Traditional AI applications mainly answer questions using prompts and the knowledge embedded inside the model.

Enterprise AI is different.

An enterprise agent must understand:

- Who is asking?
- What department do they belong to?
- Which documents can they access?
- What business systems contain the required information?
- Which knowledge source is authoritative?
- Is there newer information available on the web?

Without this context, an AI agent becomes another chatbot.

With enterprise intelligence, an AI agent becomes a trusted digital coworker capable of making informed decisions.

## What is Microsoft IQ Platform?

Microsoft IQ Platform is Microsoft's unified enterprise intelligence platform designed to provide AI agents with contextual knowledge from across the organization.

Rather than treating enterprise information as isolated silos, Microsoft IQ combines multiple intelligence sources into a single platform that AI agents can consume.

The platform consists of four major intelligence pillars:

- Work IQ
- Fabric IQ
- Foundry IQ
- Web IQ

Together they provide a complete understanding of an organization.

The image below summarizes the Microsoft IQ Platform.

| **Intelligence** | **Focus**                | **Primary Context**           |
| ---------------- | ------------------------ | ----------------------------- |
| Work IQ          | People and collaboration | Microsoft 365 knowledge       |
| Fabric IQ        | Business operations      | Business data and systems     |
| Foundry IQ       | Enterprise knowledge     | Documents and knowledge bases |
| Web IQ           | Internet intelligence    | Web, news, images, videos     |

## Microsoft IQ Platform Architecture

The Microsoft IQ Platform combines multiple intelligence layers.

### 1. Work IQ

**"How your employees work"**

Work IQ provides AI agents with organizational context.

It understands:

- Employees
- Teams
- Meetings
- Emails
- Chats
- Files
- Calendars
- Work relationships
- Collaboration patterns
- Business workflows

Instead of simply knowing documents, an AI agent understands **how people work together**.

**Typical information available**

- Organization hierarchy
- Team membership
- Recent meetings
- Shared documents
- Email conversations
- Collaboration history
- Microsoft Graph relationships
- Microsoft 365 workflows

**Example**

Instead of asking:

Show me the latest project documents.

The agent can understand:

Show documents recently discussed in meetings by my product engineering team.

This level of understanding comes from Work IQ.

### 2. Fabric IQ

**"How your business operates"**

Work IQ focuses on people.

Fabric IQ focuses on business operations.

It gives AI agents access to structured business information such as:

- Customers
- Products
- Orders
- Inventory
- Finance
- ERP
- CRM
- Manufacturing
- Sales
- Business KPIs

Fabric IQ understands business entities instead of collaboration.

**Business systems supported**

Fabric IQ can provide intelligence from:

- Microsoft Fabric
- Data warehouses
- Lakehouses
- SQL databases
- ERP systems
- CRM systems
- Operational databases

**Example**

A supply chain agent could answer:

- Current inventory levels
- Warehouse status
- Product availability
- Delayed shipments
- Regional demand

instead of only searching documents.

### 3. Foundry IQ

**"How your agents unlock knowledge"**

Foundry IQ is the enterprise knowledge intelligence layer for Microsoft Foundry.

Its primary objective is to help AI agents retrieve trusted organizational knowledge.

Unlike Work IQ and Fabric IQ, Foundry IQ focuses on:

- Policies
- Procedures
- Technical documentation
- Product manuals
- Knowledge bases
- Standard operating procedures
- Compliance documents
- Research documents
- Enterprise content

Instead of directly embedding documents into prompts, Foundry IQ indexes, organizes, and retrieves the most relevant information before the model generates a response.

This dramatically improves:

- Accuracy
- Grounding
- Citation quality
- Trustworthiness

**Key capabilities**

Foundry IQ supports:

- Retrieval Augmented Generation (RAG)
- Hybrid Search
- Semantic Search
- Vector Search
- Metadata filtering
- Knowledge connectors
- Multi-step retrieval
- Citation generation

**Example**

An HR agent may answer:

What is our maternity leave policy?

Rather than relying on model memory, Foundry IQ retrieves the latest HR policy from the organization's knowledge base before generating the answer.

### 4. Web IQ

**"How you connect to web intelligence"**

Enterprise decisions sometimes require current information that does not exist inside the organization.

Web IQ provides AI agents with trusted information from the public web.

Examples include:

- News
- Financial information
- Market trends
- Public documentation
- Images
- Videos
- Industry updates

This allows enterprise agents to combine internal knowledge with external intelligence.

**Example**

A sales agent can answer:

- Latest competitor announcements
- Market trends
- Industry news
- Product launches

while also considering internal pricing and customer information.

## How Microsoft Foundry Uses IQ

Microsoft Foundry provides the platform for building enterprise AI agents.

The IQ Platform supplies the intelligence.

Think of the relationship like this:

| **Microsoft Foundry**  | **Microsoft IQ**              |
| ---------------------- | ----------------------------- |
| Builds AI agents       | Provides enterprise knowledge |
| Orchestrates workflows | Supplies context              |
| Connects models        | Retrieves trusted information |
| Executes tools         | Understands enterprise data   |
| Runs reasoning         | Grounds responses             |

Together they create enterprise-grade AI applications.

## End-to-End Flow

A typical request follows these steps:

- A user asks an AI agent a question.
- Microsoft Foundry analyzes the request.
- The agent determines which intelligence source is needed.
- IQ retrieves the appropriate context.
- The LLM reasons using the retrieved information.
- Foundry orchestrates tools if actions are required.
- The response is generated with grounded enterprise knowledge.

Instead of asking the model to "guess," the model answers using verified organizational information.

## Combining Multiple IQ Sources

One of the biggest strengths of Microsoft IQ is that agents are not limited to a single knowledge source.

Consider this question:

Which customers might be affected by today's manufacturing delay?

The agent could use:

- **Work IQ** to identify the operations team responsible.
- **Fabric IQ** to determine affected products and customers.
- **Foundry IQ** to retrieve manufacturing procedures and escalation policies.
- **Web IQ** to check whether weather or transportation disruptions contributed to the delay.

The final response combines intelligence from all four sources into a single grounded answer.

## Real-World Use Cases

**Employee Copilot**

Uses:

- Work IQ
- Foundry IQ

Provides:

- HR policies
- Leave guidance
- Organizational information
- Employee onboarding

**Customer Support Agent**

Uses:

- Foundry IQ
- Fabric IQ

Provides:

- Product documentation
- Customer history
- Warranty information
- Troubleshooting guides

**Finance Agent**

Uses:

- Fabric IQ

Provides:

- Revenue analysis
- Financial forecasting
- Budget reports
- Business KPIs

**Executive Assistant**

Uses:

- Work IQ
- Fabric IQ
- Web IQ

Provides:

- Meeting summaries
- Business performance
- Market updates
- Competitive intelligence

**Compliance Agent**

Uses:

- Foundry IQ

Provides:

- Regulatory guidance
- Company policies
- Risk assessments
- Audit support

## Benefits of Microsoft IQ Platform

Organizations adopting Microsoft IQ gain several advantages:

- Unified intelligence across multiple enterprise data sources
- Better grounded AI responses
- Reduced hallucinations through trusted retrieval
- Rich understanding of people, business processes, and knowledge
- Easier integration with Microsoft Foundry AI agents
- Improved accuracy for enterprise decision-making
- Support for both structured and unstructured data
- Ability to combine internal and external intelligence
- Consistent context across multiple AI agents

## Microsoft IQ and the Future of Enterprise AI

As organizations deploy multiple AI agents across departments, the challenge shifts from simply choosing the right language model to providing those agents with accurate, trusted, and contextual information.

The Microsoft IQ Platform addresses this by separating **reasoning** from **enterprise intelligence**. Large language models continue to evolve, but the real value comes from grounding those models in organizational knowledge. By combining Work IQ, Fabric IQ, Foundry IQ, and Web IQ, Microsoft enables AI agents to understand people, business operations, enterprise content, and the outside world through a unified intelligence layer.

When integrated with Microsoft Foundry, this creates a powerful foundation for building enterprise-grade AI agents that are not only intelligent, but also reliable, secure, and capable of delivering meaningful business outcomes.

## Summary

Building effective enterprise AI agents requires more than powerful language models. Agents must understand employees, business data, organizational knowledge, and external events to provide accurate and trustworthy responses.

The Microsoft IQ Platform delivers this intelligence through four complementary pillars:

- **Work IQ** provides context about people, collaboration, and workflows.
- **Fabric IQ** connects agents to business entities, systems of record, and operational data.
- **Foundry IQ** grounds responses using trusted enterprise documents, policies, and knowledge bases.
- **Web IQ** enriches agents with current information from the public web.

Microsoft Foundry orchestrates these intelligence sources, enabling AI agents to retrieve the right context, reason over it, and take meaningful actions. Together, Microsoft IQ and Microsoft Foundry provide a comprehensive platform for developing enterprise AI solutions that are context-aware, grounded in trusted information, and ready to support real-world business scenarios.

## References

- [Microsoft Build 2026 - BRK240 Build context aware agents from data to decisions](https://github.com/microsoft/Build26-BRK240-build-context-aware-agents-from-data-to-decisions)
- [Microsoft IQ - Unified Enterprise Intelligence for AI](https://www.microsoft.com/en-us/ai/microsoft-iq?WT.mc_id=M365-MVP-5003693)
- [Microsoft IQ documentation](https://learn.microsoft.com/en-us/microsoft-iq/?WT.mc_id=M365-MVP-5003693)
- [Fabric IQ](https://learn.microsoft.com/en-us/fabric/iq/overview?WT.mc_id=M365-MVP-5003693)
- [Foundry IQ](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq/?WT.mc_id=M365-MVP-5003693)
- [Web IQ](https://www.microsoft.com/en-us/webiq/?WT.mc_id=M365-MVP-5003693)
- [Work IQ](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/work-iq/?WT.mc_id=M365-MVP-5003693)
