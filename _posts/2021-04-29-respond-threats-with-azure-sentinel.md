---
title: "Respond to Office 365 Threats with Azure Sentinel"
date: "2021-04-29"
share: true
header:
  image: media/2021-04-29-respond-threats-with-azure-sentinel/01.png
  teaser: media/2021-04-29-respond-threats-with-azure-sentinel/01.png
categories:
  - Office 365
  - MS Azure
tags:
  - "2021"
  - April 2021
last_modified_at: 2021-04-29T00:00:00-00:00
---

## Overview

Detecting the threats and respond to it in a timely manner is very crucial in the Office 365 environments. In the previous article, we explored [Threat detection with Azure Sentinel analytics](https://nanddeepnachanblogs.com/posts/2021-04-15-threat-detection-with-azure-sentinel-analytics/).

In this article, we will take this further to respond to the threats.

## Responding to threats

We can use playbooks together with automation rules to automate incident response and remediate security threats detected by Azure Sentinel.

**Automation rules**

Automation rules are helpful to triage incidents in Azure Sentinel. With automation rules, we can assign incidents to an individual, change the severity, add tags, and also can run playbooks in response to incidents.

**Playbooks**

Playbooks are helpful to automate and orchestrate (using Azure Logic Apps) the response to an alert or incident.


## Create a playbook and logic app

Follow below steps to create a playbook in Azure Sentinel:

1. In the Azure Sentinel, under **Configuration**, click **Automation**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/01.png)

2. Click **Create** > **Add new playbook**.
3. You will be navigated to **Create a logic app** page.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/02.png)

4. In the **Logic Apps Designer**, select **Blank Logic App**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/03.png)

5. In the search box, type **Sentinel**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/04.png)

6. Select the appropriate trigger to build the logic app.
7. We will create a simple logic app, which will post a message in MS Teams channel.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/05.png)


## Automate threat response with Automation rule

Follow below steps to create automation rule to automate threat response:

1. In Azure Sentinel, under **Configuration**, click **Automation**.
2. Click **Create** > **Add new rule**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/06.png)

3. Under **Conditions**, select certain analytics rules.
4. Under **Actions**, select **Run playbook**.


## Run a playbook on demand

Follow below steps to run a playbook on demand:

1. In Azure Sentinel, under **Threat Management**, click **Incidents**.
2. Select the Incident, click **View full details**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/07.png)

3. Click **View playbooks**.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/08.png)

4. Select a playbook to run.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/09.png)

5. The message will be posted to MS Teams channel.

    ![](/media/2021-04-29-respond-threats-with-azure-sentinel/10.png)


## Summary

Detecting the threats and respond to it in a timely manner is very crucial in the Office 365 environments. We can use playbooks together with automation rules to automate incident response and remediate security threats detected by Azure Sentinel.

## References

- [Use playbooks with automation rules in Azure Sentinel](https://docs.microsoft.com/en-us/azure/sentinel/tutorial-respond-threats-playbook?WT.mc_id=ES-MVP-5003693)