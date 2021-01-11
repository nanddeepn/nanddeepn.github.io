---
title: "Power Automate to create SharePoint List View"
date: "2021-01-11"
share: true
header:
  image: media/2021-01-11-power-automate-create-sp-list-view/07.png
  teaser: media/2021-01-11-power-automate-create-sp-list-view/07.png
categories:
  - SharePoint
  - Power Automate
tags:
  - "2021"
  - January 2020
last_modified_at: 2021-01-11T00:00:00-00:00
---

## Overview

SharePoint connector offers various flow triggers and actions in Power Automate. These help us to automate various business scenarios. The triggers help us to monitor changes to the SharePoint list or library. The actions help to perform various operations.

In this article, we will explore to build one of such missing action for creating a list view from Power Automate.

## Background

Power Automate offers various actions for SharePoint including create, update, delete, etc. on SharePoint lists and list items. The most commonly used actions are as follows:

- Check-in file
- Check out file
- Create file
- Create item
- Delete file
- Delete item
- Get all lists and libraries

> Recently, I came across a [User voice](https://powerusers.microsoft.com/t5/Power-Automate-Ideas/Create-a-List-View/idi-p/19635) to have a flow action to create a view in a SharePoint list, when a new item is added. For example, when adding a new company, I want to have a view that shows all items with that company selected in another list.

## SharePoint Lists

To support the scenario, let us start by creating 2 lists as follows:

**List# 1: Companies**

A list to store company information. For simplicity, we will use only one OOB column - Title.

![](/media/2021-01-11-power-automate-create-sp-list-view/01.png)

**List# 2: Vendors**

A list to store vendor information, which will have a lookup column (Company) to the above list.

![](/media/2021-01-11-power-automate-create-sp-list-view/02.png)

## REST API

Here, we can make use of SharePoint REST API to create a list view.

End Point: 
```
/_api/web/lists/getByTitle('Vendors')/views
```

Body: 
```json
{'__metadata':{'type': 'SP.View'},'ViewType': 'HTML','Title':'My View','PersonalView':false,'ViewQuery':'<where>…</where>'}
```

Headers:
```json
{
    "accept": "application/json;odata=verbose",
    "content-type": "application/json;odata=verbose"
}
```


## Power Automate

We will start by creating a Power automate flow as follows:

- Type: Automated cloud flow
- Name: Create SharePoint List View
- Trigger: When an item is created
- List: Companies

![](/media/2021-01-11-power-automate-create-sp-list-view/03.png)

Define a variable to form a CAML query for a list view.

![](/media/2021-01-11-power-automate-create-sp-list-view/04.png)

The query is as follows:

```xml
<Where><Eq><FieldRef Name="Company" /><Value Type="Text">@{triggerOutputs()?['body/Title']}</Value></Eq></Where>
```

Define a variable to form a body content for REST API.

![](/media/2021-01-11-power-automate-create-sp-list-view/05.png)

The body content is as follows:

```json
{'__metadata':{'type': 'SP.View'},'ViewType': 'HTML','Title':'@{triggerOutputs()?['body/Title']}','PersonalView':false,'ViewQuery':'@{variables('viewQuery')}'}
```

Add an action Send an HTTP request to SharePoint to call the REST API to create a view.

![](/media/2021-01-11-power-automate-create-sp-list-view/06.png)

## Outcome

When a new company is created in the Companies list, a flow will trigger and create a new view in the Vendors list with the company name (specified in Title field of Companies list).

![](/media/2021-01-11-power-automate-create-sp-list-view/07.png)

## Summary

Power Automate offers various actions for SharePoint. We can overcome any missing actions by using REST APIs provided by SharePoint.

## References
- [User voice](https://powerusers.microsoft.com/t5/Power-Automate-Ideas/Create-a-List-View/idi-p/19635)
- [Working with lists and list items with REST](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/working-with-lists-and-list-items-with-rest)
