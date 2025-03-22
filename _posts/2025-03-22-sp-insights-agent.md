---
title: "Use case: SharePoint Insights Agent"
date: "2025-03-22"
share: true
header:
  image: media/2025-03-22-sp-insights-agent/01.png
  teaser: media/2025-03-22-sp-insights-agent/01.png
categories:
  - Microsoft 365
  - SharePoint
  - Copilot
tags:
  - "2025"
  - March 2025
last_modified_at: 2025-03-22T00:00:00-00:00
---
## Introduction

In today's dynamic digital workplaces, managing SharePoint Online environments efficiently has become a priority for IT admins, compliance officers, and even end users. As organizations continue to create, share, and collaborate using hundreds or even thousands of SharePoint sites, having centralized visibility into these environments is essential. 

This is where the SP Insights Agent comes in. An intelligent assistant designed to answer user queries about SharePoint sites by leveraging hidden but powerful data sources.

## What is SP Insights Agent?

SP Insights Agent is a conversational, intelligent assistant purpose-built to help users access critical information about SharePoint Online sites quickly and accurately. It pulls site metadata from a hidden SharePoint list named `DO_NOT_DELETE_SPLIST_TENANTADMIN_AGGREGATED_SITECOLLECTIONS`, which acts as an aggregated source of site collection data within the Microsoft 365 tenant.

The agent is designed to respond to user queries such as:

- Who is the owner of site XYZ?
- When was the site XYZ created?
- Tell me about the site XYZ.
- How much storage has site XYZ used?
- How many sites are externally shared?

By interpreting these questions and responding with precise data, SP Insights Agent becomes a valuable asset for improving visibility, governance, and operational efficiency.

## Understanding the Data Source

At the heart of the SP Insights Agent lies a special SharePoint hidden list with a long and specific name: `DO_NOT_DELETE_SPLIST_TENANTADMIN_AGGREGATED_SITECOLLECTIONS`. Despite its intimidating title, this list is incredibly powerful. It is automatically maintained by SharePoint Online and aggregates metadata about all site collections in the tenant.

The list contains key columns such as:

- Title (Site Title)
- Created By and TimeCreated
- Site Owner Name and Email
- StorageUsed and StorageQuota
- ExternalSharing
- LastActivityOn

This data can be exported to CSV and processed through Power Automate, Microsoft Graph, or other automation pipelines to feed the SP Insights Agent.

## How Does SP Insights Agent Work?

The architecture is simple yet effective:

- Data Source: The agent relies on the `DO_NOT_DELETE_SPLIST_TENANTADMIN_AGGREGATED_SITECOLLECTIONS` list.
- CSV Conversion: A Power Automate Scheduled Flow or a script periodically exports the list data into a structured CSV.
- Data Formatting: The CSV is transformed into text-based records, making each site's details easy to parse by the agent.
- Query Understanding: The agent uses natural language understanding (NLU) to identify user intent and extract the site identifier (e.g., "site XYZ").
- Response Generation: Based on the processed data, the agent constructs a factual, concise, and user-friendly response.

## The Architecture

Here is the architecture of the SP Insights Agent, showing how it leverages data from SharePoint to provide intelligent responses to user queries. 

![](/media/2025-03-22-sp-insights-agent/01.png)

### Step 1: Source of Truth – SharePoint Hidden List
- **List Name:** `DO_NOT_DELETE_SPLIST_TENANTADMIN_AGGREGATED_SITECOLLECTIONS`
- This hidden SharePoint list contains metadata about **all site collections** in the Microsoft 365 tenant.
- It includes:
  - Site Title
  - Created By
  - Site Owner Name & Email
  - Creation Date
  - Storage Used / Quota
  - External Sharing status
  - Last Activity Date

### Step 2: Automated Data Export – Power Automate Flow
- A **Scheduled Power Automate flow** runs periodically (e.g., daily).
- It reads the hidden list and **exports the data to a CSV file**.
- This keeps the agent’s data **fresh and up to date**.

### Step 3: CSV File Generation
- The exported CSV contains rows for each site and columns for each property.
- This structured file becomes the **primary dataset** for the agent to work with.

### Step 4: CSV to Text Transformation
- The CSV is uploaded to the assistant (SP Insights Agent).
- The agent **converts each row into a readable line of text**.

**Example Output:**
``Site: SPDemo, CreatedBy: Nanddeep Nachan, CreatedOn: 11/20/2019, Owner: Nanddeep Nachan, StorageUsed: 2.4 GB, ExternalSharing: On``

### Step 5: Knowledge Layer – SharePoint + Word/Text
- The processed text is saved as a **Word document** or **text file** in SharePoint.
- This file acts as the **knowledge base** the agent uses to answer queries.

### Step 6: SP Insights Agent Responds to Queries
- Users ask natural language questions like:
  - "Who owns site SPDemo?"
  - "How much storage is used by site XYZ"
- The agent parses the question, searches the text data, and provides a **factual and user-friendly response**.


## Challenges Solved by SP Insights Agent

- **Reduced Admin Center Dependency:** Users and helpdesk teams no longer need to dig through SharePoint Admin Center to find basic site information.
- **Time Efficiency:** What used to take minutes (or hours) to track down can now be answered instantly.
- **Governance and Compliance:** The agent highlights externally shared sites, inactive sites, or orphaned sites (with no owner), improving organizational control.
- **Improved Support Experience:** Helpdesk and IT support teams can handle SharePoint-related questions more effectively without escalating to senior admins.
- **Empowering Non-Technical Users:** Even business users can ask questions about sites they use and get quick insights in human-readable format.

## Sample Use Cases

- IT Admins use it to identify sites nearing their storage quota or with external sharing turned on.
- Support Teams rely on it to respond to "Who owns this site?" tickets.
- Security Teams use it to monitor externally shared or inactive sites.
- Site Owners themselves use it to validate site information without needing admin access.

## Agent Behavior and Personality

The SP Insights Agent has been intentionally designed with the following traits:

- **Factual:** It answers only based on available data.
- **Helpful:** If it cannot find something, it guides users on where else to look.
- **Friendly:** Communicates in a tone that is clear, approachable, and professional.
- **Reliable:** Always provides consistent and auditable responses.

## Limitations to Keep in Mind

- The agent cannot update or delete SharePoint data.
- It relies solely on the latest exported data; hence the frequency of data refresh impacts accuracy.
- Some advanced metadata (e.g., permissions, content types) is not included in the source list.
- Sites not yet aggregated in the list won't be visible to the agent.

## Summary

SP Insights Agent is a low-overhead, high-impact solution for organizations that want to improve visibility and reduce friction in managing SharePoint Online. It transforms a hidden and underutilized list into an intelligent, user-friendly interface for answering everyday site-related questions.

## Video Tutorial

Watch the video version of this article.

<iframe src="https://www.youtube.com/embed/BjvkEyDxFu8" frameborder="0" allowfullscreen></iframe>
