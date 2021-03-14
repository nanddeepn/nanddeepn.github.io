---
title: "Monitor Office 365 Logs from Azure Sentinel"
date: "2021-03-14"
share: true
header:
  image: media/2021-03-14-monitor-o365-logs-azure-sentinel/09.png
  teaser: media/2021-03-14-monitor-o365-logs-azure-sentinel/09.png
categories:
  - Office 365
  - MS Azure
tags:
  - "2021"
  - March 2021
last_modified_at: 2021-03-14T00:00:00-00:00
---

## Overview

Office 365 usage is increasing hugely. With an increase in usage day by day, it increases the challenges to monitoring the Office 365 environment for its usage. There are various products, tools, and ways available to monitor the user and admin activities in Office 365.

In this article, we will explore Azure Sentinel to monitor the Office 365 environment for its usage and detect possible risks.

## Challenges with Office 365 environment monitoring

As a large number of users are using various Office 365 apps and services (including Exchange, SharePoint, MS Teams). It is essential to keep track of user activities.

Office 365 offers various fine-grained roles or administrators who can perform various activities to certain apps and services by modifying tenant or application settings (e.g. enable external sharing, set authentication mode, network settings, etc.) which may impose a security risk.

Office 365 monitors most of these activities and logs those under the security and compliance center. However, these logs are available for 90 days only. For auditing and historic purpose, we should store these logs somewhere and Azure Sentinel comes in handy here.

## Azure Sentinel overview

Microsoft Azure Sentinel is a scalable, cloud-native, security information event management (SIEM) and security orchestration automated response (SOAR) solution. It delivers intelligent security analytics and threat intelligence across the enterprise for alert detection, threat visibility, proactive hunting, and threat response.

Azure Sentinel provides connectors to connect to your data in various systems including Microsoft 365 Defender (formerly Microsoft Threat Protection) solutions, and Microsoft 365 sources, including Office 365, Azure AD, Microsoft Defender for Identity (formerly Azure ATP), and Microsoft Cloud App Security.

## Set up Azure Sentinel

We will start by setting up the Azure Sentinel. Follow the below steps:

1. Open Azure portal ([https://portal.azure.com](https://portal.azure.com/)).
2. From the left menu, click **All services**.
3. Under **Security**, click **Azure Sentinel**.

    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/01.png)

## Set up log analytics workspace

In the Azure Sentinel, we first need to create a workspace. Follow the below steps:

1. On the Azure Sentinel page, click **Create Azure Sentinel**.
    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/02.png)

2. Click **Create a new workspace**.
    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/03.png)

3. Specify the name and region.
    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/04.png)

4. Add Azure Sentinel to a workspace by clicking **Add**.

    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/05.png)

5. Once Azure Sentinel is connected to Log Analytics Workspace, it looks like below:

    ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/06.png)

## Enable unified audit logging on Office 365 deployment

For the connector to be able to access data through the Office 365 Management Activity API, we must have unified audit logging enabled on Office 365 deployment.

Follow below steps to turn on auditing:

1. Go to the Compliance center audit log search ([https://compliance.microsoft.com/auditlogsearch](https://compliance.microsoft.com/auditlogsearch)).
2. If the audit log search is not turned on for your organization, a banner is displayed to turn it on.
3. Click **Turn on auditing**.

## Connect Office 365 logs to Azure Sentinel

Office 365 log connector helps to bring user and admin activities from Exchange, SharePoint, OneDrive, and MS Teams into Azure Sentinel. This information includes details of actions such as file downloads, access requests sent, changes to group events, mailbox operations, Teams events (such as chat, team, member, and channel events), as well as the details of the user who performed the actions.

Prerequisites:
- Read and write permissions on your Azure Sentinel workspace.
- Global administrator or security administrator rights on Office 365 tenant.
- Office 365 deployment must be on the same tenant as your Azure Sentinel workspace.

Follow the below steps to connect the Office 365 logs to Azure Sentinel:

1. Under **Configuration** , click **Data connectors**.
2. Search **Office 365**.
3. Select Office 365 connector. The Office 365 activity log connector provides insight into ongoing user activities (e.g. file downloads, access requests sent, changes to group events, etc.)

  ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/07.png)

4. Click **Open connector page**.
5. Azure Sentinel now enables Office 365 single-tenant connection. Under Configuration, select the record types you want to collect from your tenant.

  ![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/08.png)

## Azure Sentinel Dashboard

In the Azure Sentinel under the **Overview** section, the events and alerts will start to show over time.

![](/media/2021-03-14-monitor-o365-logs-azure-sentinel/09.png)

## Summary

Connecting Office 365 logs to Azure Sentinel enables you to view and analyze user and admin activities data in your workbooks and provides more insight into your Office 365 security.

## References

- [What is Azure Sentinel?](https://docs.microsoft.com/azure/sentinel/overview?WT.mc_id=ES-MVP-5003693)
- [Azure Sentinel Product Overview](https://azure.microsoft.com/services/azure-sentinel/?WT.mc_id=ES-MVP-5003693)
- [Azure Sentinel Pricing](https://azure.microsoft.com/pricing/details/azure-sentinel/?WT.mc_id=ES-MVP-5003693)
- [Connect Office 365 Logs to Azure Sentinel](https://docs.microsoft.com/en-us/azure/sentinel/connect-office-365?WT.mc_id=ES-MVP-5003693)
- [Turn audit log search on or off](https://docs.microsoft.com/en-us/microsoft-365/compliance/turn-audit-log-search-on-or-off?WT.mc_id=ES-MVP-5003693)
