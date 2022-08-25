---
title: "Tips to Send Adaptive Card to users in MS Teams with Power Automate"
date: "2022-08-25"
share: true
header:
  image: media/2022-08-25-tips-send-ac-teams-power-automate/04.png
  teaser: media/2022-08-25-tips-send-ac-teams-power-automate/04.png
categories:
  - MS Teams
  - Power Automate
tags:
  - "2022"
  - August 2022
last_modified_at: 2022-08-25T00:00:00-00:00
---

## Overview

Power Automate provides out-of-the-box action to send actionable adaptive cards on MS Teams. This action is very handy for better representing the information to users.

In this article, we will explore the practical scenarios to address while using this functionality.

## The Basics

In the simplest form, the Power Automate action named **Post adaptive card in a chat or channel** can be used to send an adaptive card on MS Teams to Channel or an existing Group chat.

![](/media/2022-08-25-tips-send-ac-teams-power-automate/01.png)

## How to send an Adaptive Card to a user on 1:1 chat?

The **Post adaptive card in a chat or channel** action does not provide an option to send the adaptive card to a specific user. At the same time, you cannot send an adaptive card to the user, if you have not had any chat with them before.

These limitations can be overcome by another action in Power Automate called **Create a chat**.

![](/media/2022-08-25-tips-send-ac-teams-power-automate/02.png)

In the **Create a chat** action, specify the members to start the conversation. Pass the `Conversation Id` from this action to the **Post adaptive card in a chat or channel** action to send the adaptive card to a specific user.

The adaptive card lands in a 1:1 chat with the specified user as follows:

![](/media/2022-08-25-tips-send-ac-teams-power-automate/03.png)

The next thing that might be worrying to the end users is the Power Automate icon and the text `someone via Power Automate`, as highlighted in the above figure. Because of these two points, someone might think of this one as fishy.

## Add Trustworthy feeling

To make appear the message coming from a trusted sender, you may consider using a service account with the organization logo and an email.

![](/media/2022-08-25-tips-send-ac-teams-power-automate/04.png)

Hopefully, these simple tips should help you design messages from Power Automate that look coming from trusted sources within the organization.

## Summary

Power Automate's **Post adaptive card in a chat or channel** action is very handy for presenting information better to the users with Adaptive cards. It lacks the functionality to send the Adaptive cards as a 1:1 message. The combination of actions demonstrated in this article should help you overcome the limitation and make the message appear from a trusted source within the organization.

## References

- [Create flows that post adaptive cards to Microsoft Teams](https://docs.microsoft.com/en-us/power-automate/create-adaptive-cards?WT.mc_id=M365-MVP-5003693)
- [Send a message in Teams using Power Automate](https://docs.microsoft.com/en-us/power-automate/teams/send-a-message-in-teams?WT.mc_id=M365-MVP-5003693)
