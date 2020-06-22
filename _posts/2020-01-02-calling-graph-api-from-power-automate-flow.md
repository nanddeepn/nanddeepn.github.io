---
title: "Calling Graph API from Power Automate Flow"
date: "2020-01-02"
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
3. From the left menu, under “Admin centers”, click “Azure Active Directory”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-431.png)

1. From the left menu, click “Azure Active Directory”.
2. Click “App registrations”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-432.png)

1. Click “New registration”.
2. Provide the application name, supported account type and leave the Redirect URI blank.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-433.png)

1. Click Register.
2. Note down Application and Directory IDs to use later in the flow.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-434.png)

1. From the left menu, click “API permissions” to grant some permissions to the application.
2. Click “+ Add a permission”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-435.png)

1. Select “Microsoft Graph”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-436.png)

1. Select Application permissions.
2. Grant the needed permissions.
3. Click “Add permissions”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-437.png)

Below are examples of the needed application permissions to perform the operations.

**Action**

**Application permissions**

List owners

Group.Read.All, User.Read.All, Group.Read.All, and User.ReadWrite.All

Add member

GroupMember.ReadWrite.All, Group.ReadWrite.All, and Directory.ReadWrite.All

## Grant required permissions to the application

After creating the permissions, we need to grant consent to the application to allow the application to access Graph API without a consent screen.

1. Click “Grant admin consent for …”

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-438.png)

1. From the left menu, click “Certificates & secrets”.
2. Under “Client secrets”, click “+ New client secret”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-439.png)

1. Provide a description and select an expiry time for the secret.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-440.png)

1. Click “Add”.
2. Make a note of the secret value.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-441.png)

## Build Power Automate Flow

We will start by building flow.

1. Navigate to Power Automate in Office 365.
2. From the left menu, click Create.
3. Select “Instant flow”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-442.png)

1. In the modal dialog, specify the flow name (e.g. Join Private Group).
2. Select the trigger as “When an HTTP request is received”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-443.png)

1. Click Create.
2. The flow should get created as below.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-444.png)

1. Expand the activity “When a HTTP request is received”.
2. Click “Generate from sample”.
3. In the popup, add the above request body JSON schema.

{  
   "groupId":"07c00c4d-6a21-4c51-9545-f2921b4109b0"  
}

1. Click Done.
2. The request body json schema will be generated.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-445.png)

1. Initialize the variables client id, directory (tenant) id and secret generated from the previous step.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-446.png)

These variables then can be used to make an MS Graph API call.

1. Add the “HTTP” activity.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-447.png)

## Test Flow with Soap UI

Download the SoapUI from [https://www.soapui.org/](https://www.soapui.org/)

1. Select Method as Post.
2. Specify the endpoint as HTTP POST URL.
3. Specify the header as Content-type with value application/json
4. In the Body, specify the JSON.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-448.png)

1. Click Send.
2. Observe the run history of flow.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-449.png)

## Summary

Calling the Graph API from Power Automate Flow opens a wide range of possibilities. One scenario could be to get the things done with application permissions, which otherwise cannot work under user delegated permissions. Power automate flow then can be called from an SPFx solution to build more advanced scenarios.

Please refer to my previous article - [Execute Power Automate Workflow from SPFx](https://nanddeepnachanblogs.com/2019/12/execute-power-automate-workflow-from-spfx/) for more details.

 

This content was originally posted [here](https://www.c-sharpcorner.com/article/calling-graph-api-from-power-automate-flow/).
