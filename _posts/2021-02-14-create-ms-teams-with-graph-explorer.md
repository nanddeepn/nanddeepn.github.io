---
title: "Create MS Teams Team with Graph Explorer"
date: "2021-02-14"
share: true
header:
  image: media/2021-02-14-create-ms-teams-with-graph-explorer/01.png
  teaser: media/2021-02-14-create-ms-teams-with-graph-explorer/01.png
categories:
  - MS Graph
  - MS Teams
tags:
  - "2021"
  - February 2021
last_modified_at: 2021-02-14T00:00:00-00:00
---

## Overview

Microsoft Graph provides APIs for Microsoft 365 and a unified programmability model. It offers various APIs to work with Microsoft Teams, a chat-based workspace in Microsoft 365.

In this article, we will create a Microsoft Teams team using Microsoft Graph Explorer.

## Microsoft Graph Explorer

It is a web-based tool to build and execute requests against Microsoft 365 resources using Microsoft Graph APIs. It helps us to test `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` operations with v1.0 and Beta endpoints for resources in Microsoft 365. The APIs under /beta version may change in the future, so should be avoided using in the production-ready environments.

Microsoft Graph Explorer can be accessed from [https://developer.microsoft.com/en-us/graph/graph-explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) or [https://aka.ms/ge](https://aka.ms/ge)

## Create an Office 365 Group

Microsoft Teams team is backed up by an Office 365 Group. In Microsoft Graph, the Microsoft Teams team is represented by a group resource. Such groups have a `resourceProvisioningOptions` property set to `Team`.

We will start by creating an Office 365 group from Microsoft Graph Explorer.

To get all the Office 365 groups, use the below API:

```
GET https://graph.microsoft.com/v1.0/groups
```

We can create a new group as Microsoft 365 group (unified group) or Security group using below API:

```
POST https://graph.microsoft.com/v1.0/groups
```

**Permissions requirement**

Any of the below permission is needed to call this API:

| **Permission type** | **Permissions** |
| --- | --- |
| Delegated (work or school account) | Group.ReadWrite.All, Directory.ReadWrite.All, Directory.AccessAsUser.All |
| Application | Group.Create, Group.ReadWrite.All, Directory.ReadWrite.All |

**Request body**

Use below request body:

```json
{
  "description": "Group for Graph Team",
  "displayName": "Graph Team",
  "groupTypes": [
    "Unified"
  ],
  "mailEnabled": true,
  "mailNickname": "graphteam",
  "securityEnabled": false,
  "owners@odata.bind": [
    "https://graph.microsoft.com/v1.0/users/admin@contoso.onmicrosoft.com"
  ]
}
```

The group type of Unified creates Microsoft 365 group (unified group). The users specified in `owners@odata.bind` gets added as group owners.

![](/media/2021-02-14-create-ms-teams-with-graph-explorer/01.png)

When the group is created, note down the group id which will be needed for the creation of the Microsoft Teams team.

## Create Microsoft Teams from the Group

The group should have at least one owner to create a team from it. You may face a 404 error due to replication delays if a group is created recently (less than 15 minutes ago).

To create a Team from the Group, use the below API:

```
PUT https://graph.microsoft.com/v1.0/groups/<group-id>
```

Replace the `<group-id>` with the GUID returned as a group id from the execution of the previous API.

**Permissions requirement**

Any of the below permission is needed to call this API:

| **Permission type** | **Permissions** |
| --- | --- |
| Delegated (work or school account) | Group.ReadWrite.All, Directory.ReadWrite.All |
| Application | Group.ReadWrite.All, Directory.ReadWrite.All |

**Request body**

```json
{  
  "memberSettings": {
    "allowCreatePrivateChannels": true,
    "allowCreateUpdateChannels": true
  },
  "messagingSettings": {
    "allowUserEditMessages": true,
    "allowUserDeleteMessages": true
  },
  "funSettings": {
    "allowGiphy": true,
    "giphyContentRating": "strict"
  }
}
```

The request body helps to set the configurations to the team for a member, messaging, and fun (for giphy).

The team will be created with a response code as `201 Created`.

![](/media/2021-02-14-create-ms-teams-with-graph-explorer/02.png)

## Summary

Microsoft Graph offers various APIs to work with Microsoft Teams. Microsoft Teams is backed up by an Office 365 Group. We can use the APIs to create Office 365 Group and then create a Team based on the group.

## References

- [Overview of Microsoft Graph](https://docs.microsoft.com/en-us/graph/overview?WT.mc_id=M365-MVP-5003693)
- [Microsoft Graph, Create group](https://docs.microsoft.com/en-us/graph/api/group-post-groups?WT.mc_id=M365-MVP-5003693)
- [Create team from group](https://docs.microsoft.com/en-us/graph/api/team-put-teams?WT.mc_id=M365-MVP-5003693)
