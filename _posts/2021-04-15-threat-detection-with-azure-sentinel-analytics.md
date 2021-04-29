---
title: "Threat detection with Azure Sentinel analytics"
date: "2021-04-15"
share: true
header:
  image: media/2021-04-15-threat-detection-with-azure-sentinel-analytics/07.png
  teaser: media/2021-04-15-threat-detection-with-azure-sentinel-analytics/07.png
categories:
  - Office 365
  - MS Azure
tags:
  - "2021"
  - April 2021
last_modified_at: 2021-04-15T00:00:00-00:00
---

## Overview

Microsoft Azure Sentinel Analytics provides an intelligent solution to help detect potential threats and vulnerabilities in the organizations.

In this article, we will explore how Azure Sentinel Analytics can help identify and stop cyber attacks.

## Azure Sentinel Analytics Overview

Azure Sentinel Analytics helps to detect, investigate, and remediate cyber security threats. It analyzes data from various sources to identify correlations and anomalies. Using analytics rules, we can trigger alerts based on the attack techniques that are used by known malicious actors. With the analytics rule, we can get insights of where an attack originated from, what resources were compromised, potential data lost, along with the timeline for the incident.

The analytics rules can be managed from Azure Sentinel Analysis home page.

![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/01.png)

## Create Analytics Rule

Alerts triggered in Microsoft security solutions like Microsoft Defender for Identity that are connected to Azure Sentinel do not automatically create incidents. However, we can configure Azure Sentinel to automatically create incidents every time an alert is triggered.


**Create rule from Rule templates**
Follow below steps to create rule from existing built-in templates:

1. Select any of existing **Rule template**.
2. Click **Create Rule**.

    ![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/02.png)


**Microsoft incident creation rule**
We can create a new Microsoft security rule that filters alerts from different Microsoft security services by following below steps:
1. Click **Create** > **Microsoft incident creation rule**.

    ![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/03.png)

2. Specify `Name` and `Description`.
3. Select the `Microsoft security service` (e.g. Microsoft Defender for Office 365)

    ![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/04.png)


**Custom analytics rule with a scheduled query**
1. Specify unique `Name` and `Description`.
2. In the `Tactics` field, choose categories of attacks to classify the rule.
3. Set the alert `Severity`.

    ![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/05.png)

4. Specify `Rule query` as follows:

    ```
    OfficeActivity
    | where OfficeWorkload in ("SharePoint") and Operation in ("ListItemUpdated")
    ```

    ![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/06.png)

When there will be more than 5 edits to SharePoint list item in span of 5 hours, Azure Sentinel will create an incident for it for bulk editing, as shown below:

![](/media/2021-04-15-threat-detection-with-azure-sentinel-analytics/07.png)

## Summary

Azure Sentinel Analytics helps to detect, investigate, and remediate cyber security threats by analyzing data from various sources to identify correlations and anomalies.

## References

- [Threat detection with Azure Sentinel analytics](https://docs.microsoft.com/en-us/learn/modules/analyze-data-in-sentinel/?WT.mc_id=ES-MVP-5003693)
- [Automate incident handling in Azure Sentinel with automation rules](https://docs.microsoft.com/en-us/azure/sentinel/automate-incident-handling-with-automation-rules?WT.mc_id=ES-MVP-5003693)
- [Detect threats out-of-the-box](https://docs.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats-built-in?WT.mc_id=ES-MVP-5003693)
- [Create custom analytics rules to detect threats](https://docs.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats-custom?WT.mc_id=ES-MVP-5003693)
