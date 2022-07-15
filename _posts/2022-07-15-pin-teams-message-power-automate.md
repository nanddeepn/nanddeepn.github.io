---
title: "Pin MS Teams messages with Power Automate"
date: "2022-07-15"
share: true
header:
  image: media/2022-07-15-pin-teams-message-power-automate/01.png
  teaser: media/2022-07-15-pin-teams-message-power-automate/01.png
categories:
  - Power Platform
  - MS Teams
tags:
  - "2022"
  - July 2022
last_modified_at: 2022-07-15T00:00:00-00:00
---
## Overview

Microsoft Teams offers Pin functionality to highlight an important message. In a typical chat conversation, there will be a huge number of messages. Pinning a specific reply or a message will help everyone visually identify it as an important one.

In this article, we will cover how to pin a message using Power Automate.

## Pin chat messages

Pinning a chat message provides quick access to the important content. Any participant can pin a chat message.

![](/media/2022-07-15-pin-teams-message-power-automate/01.png)

More than one message or message reply can be pinned at a time. We can use this functionality to highlight a potential valuable reply. This will help everyone to look at the pinned reply and save time to read through all replies.

**API support**

Unfortunately, we do not have a programmable way to Pin the chat message. As of today, we do not have any PowerShell or MS Graph API support for this functionality.

## The background processing

Let us analyze what happens in the background when you pin a message in MS Teams.

1. Open Microsoft Teams in a browser.
2. Open the Developer toolbar and navigate to the **Network** tab.
3. Navigate to a conversation with replies.
4. Pin a message reply and observe the background calls from the Network tab.

![](/media/2022-07-15-pin-teams-message-power-automate/02.png)

The highlighted call goes in the background. Let us analyze the elements of it.

![](/media/2022-07-15-pin-teams-message-power-automate/03.png)

1. This refers to the channel id of the General channel of the team.
2. This refers to the channel id of the Team channel where the message is pinned
3. This refers to the Id of the message.

## Power Automate to pin the message

I came across an interesting article - [Teams Status Update via Power Automate](https://www.damobird365.com/teams-status-update-via-power-automate/) by [Damien Bird](https://twitter.com/DamoBird365) mentioning that the undocumented Teams API can be called from Power Automate by leveraging `Send HTTP request to SharePoint` action.

The below action in Power Automate will do the trick to pin a message in MS Teams.

![](/media/2022-07-15-pin-teams-message-power-automate/04.png)

## Summary

Pin functionality in Microsoft Teams helps to highlight an important message. As of now, we do not have any PowerShell or MS Graph API support for this functionality. However, this approach should help you do the trick to pin the messages.

## Disclaimer

Please do not use this functionality in the Production, as it is using undocumented Teams API.

## References

- [Microsoft 365 roadmap: Microsoft Teams: Pin chat messages](https://www.microsoft.com/en-ww/microsoft-365/roadmap?filters=&searchterms=82584)