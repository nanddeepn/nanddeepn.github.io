---
title: "Calling Graph API from Power Automate Flow"
date: "2020-01-02"
share: true
categories:
  - MS Graph
  - Power Automate
  - Power Platform
header:
  image: media/2020-01-02-calling-graph-api-from-power-automate-flow/06.png
  teaser: media/2020-01-02-calling-graph-api-from-power-automate-flow/06.png
tags:
  - "2020"
  - January 2020
last_modified_at: 2020-01-02T00:00:00-00:00
---

## Overview

Microsoft Graph API offers a single endpoint to connect to data from various services. Power Automate (earlier MS Flow) enables to design powerful workflows. Calling the Graph API from Power Automate Flow opens a wide range of possibilities. One scenario could be to get the things done with application permissions, which otherwise cannot work under user delegated permissions.

In the article, we will explore a scenario of calling Graph API from Power Automate Flow.


## Prerequisites

Before we proceed, make sure you have below permissions:

- Administrative access to Azure AD of Office 365 tenant
- Permissions to create flow in Power Automate (prior MS Flow)


## Create an application in Azure AD

We will start by registering an application in Azure AD. Follow the below steps to create an application in Azure AD:

1. Login to Microsoft 365 Portal ([https://portal.office.com](https://portal.office.com))
2. Open Microsoft 365 admin center ([https://admin.microsoft.com](https://admin.microsoft.com))
3. From the left menu, under **Admin centers**, click **Azure Active Directory**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/01.png)

4. From the left menu, click **Azure Active Directory**.
5. Click **App registrations**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/02.png)

6. Click **New registration**.
7. Provide the application name, supported account type and leave the Redirect URI blank.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/03.png)

8. Click **Register**.
9. Note down Application and Directory IDs to use later in the flow.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/04.png)

10. From the left menu, click **API permissions** to grant some permissions to the application.
11. Click **+ Add a permission**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/05.png)

12. Select **Microsoft Graph**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/06.png)

13. Select **Application permissions**.
14. Grant the needed permissions.
15. Click **Add permissions**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/07.png)

Below are examples of the needed application permissions to perform the operations.

**Action**|**Application permissions**
List owners|Group.Read.All, User.Read.All, Group.Read.All, and User.ReadWrite.All
Add member|GroupMember.ReadWrite.All, Group.ReadWrite.All, and Directory.ReadWrite.All


## Grant required permissions to the application

After creating the permissions, we need to grant consent to the application to allow the application to access Graph API without a consent screen.

1. Click **Grant admin consent for …**

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/08.png)

2. From the left menu, click **Certificates & secrets**.
3. Under **Client secrets**, click **+ New client secret**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/09.png)

4. Provide a description and select an expiry time for the secret.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/10.png)

5. Click **Add**.
6. Make a note of the secret value.

![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/11.png)


## Build Power Automate Flow

We will start by building a flow.

1. Navigate to Power Automate in Office 365.
2. From the left menu, click Create.
3. Select **Instant flow**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/12.png)

4. In the modal dialog, specify the flow name (e.g. Join Private Group).
5. Select the trigger as **When an HTTP request is received**.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/13.png)

6. Click **Create**.
7. The flow should get created as below.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/14.png)

8. Expand the activity **When a HTTP request is received**.
9. Click **Generate from sample**.
10. In the popup, add the above request body JSON schema.

      ```
      {  
         "groupId":"07c00c4d-6a21-4c51-9545-f2921b4109b0"  
      }
      ```

11. Click **Done**.
12. The request body json schema will be generated.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/15.png)

13. Initialize the variables client id, directory (tenant) id and secret generated from the previous step.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/16.png)

      These variables then can be used to make an MS Graph API call.

14. Add the **HTTP** activity.

![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/17.png)


## Test Flow with Soap UI

Download the SoapUI from [https://www.soapui.org/](https://www.soapui.org/)

1. Select Method as **Post**.
2. Specify the endpoint as HTTP POST URL.
3. Specify the header as **Content-type** with value **application/json**.
4. In the Body, specify the JSON.

      ![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/18.png)

5. Click **Send**.
6. Observe the run history of flow.

![](/media/2020-01-02-calling-graph-api-from-power-automate-flow/19.png)


## Summary

Calling the Graph API from Power Automate Flow opens a wide range of possibilities. One scenario could be to get the things done with application permissions, which otherwise cannot work under user delegated permissions. Power automate flow then can be called from an SPFx solution to build more advanced scenarios.

Please refer to my previous article - [Execute Power Automate Workflow from SPFx](/posts/2019-12-03-execute-power-automate-workflow-from-spfx) for more details.
 

This content was originally posted [here](https://www.c-sharpcorner.com/article/calling-graph-api-from-power-automate-flow/).
