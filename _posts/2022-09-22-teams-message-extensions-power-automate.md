---
title: "Building messaging extensions for MS Teams with Power Automate"
date: "2022-09-22"
share: true
header:
  image: media/2022-09-22-teams-message-extensions-power-automate/04.png
  teaser: media/2022-09-22-teams-message-extensions-power-automate/04.png
categories:
  - MS Teams
  - Power Automate
tags:
  - "2022"
  - September 2022
last_modified_at: 2022-09-22T00:00:00-00:00
---
## Overview

Messaging extensions are a great way to define action on the selected message in Microsoft Teams. In the past, it was only possible to create messaging extensions with custom coding and deployment efforts. But now even citizen developers can build messaging extensions with few clicks. You can use messages to trigger processes in Teams.

In this article, we will explore how to create messaging extensions for MS Teams with Power Automate and add some AI factors to it to create a profanity reporting use case.

## Get started on messaging extensions with Power Automate

For creating messaging extensions in Teams using Power Automate, the first thing you should do is to add the Power Automate app.

![](/media/2022-09-22-teams-message-extensions-power-automate/01.png)

The default view shows the basic information on the created flows. The column "Team and Channel" is tricky to read. It lists out the Teams and Channels your flow reads/writes to/from.

The page provides you to get started by using popular Teams templates or even you can start from blank. One thing to note, only instant flows qualify for messaging extensions.

## Build a messaging extension to save the message to OneNote

Let us start with a pre-built template, **Save a message to OneNote**.

![](/media/2022-09-22-teams-message-extensions-power-automate/02.png)

1. The first thing that we need to do here is to name our flow.
2. Second thing, we need to verify all the connections. Once it is done, click Next.

    ![](/media/2022-09-22-teams-message-extensions-power-automate/03.png)

3. Then we need to select Notebook as well as the section within that Notebook where we need to store the message and then just click on create flow and That's it.

That's it. Your first messaging extension is ready.

**Test the messaging extension**

Let us head to any existing message in Teams. Under **More actions** , you will see all the messaging extensions listed.

![](/media/2022-09-22-teams-message-extensions-power-automate/04.png)

Each messaging extension created with Power Automate will have a special logo next to it.

As you select the **Save a message to OneNote** messaging extension, add any optional note along with the message and it will be saved to OneNote. Clicking View note will take you to the saved conversation in OneNote.

One limitation to note here is that it only saves the top message and not the replies.

## Messaging extension to report inappropriate content

Now let us have look at the implementation of one of the custom messaging extensions built with some AI capabilities.

MS Teams is for collaboration and while conversing with colleagues, we should always consider the well-being factor, and no one gets hurt by any profanity.

As a pre-requisite, you first need to create a Content Moderator service in Azure. Let us start by building a Power automate flow.

![](/media/2022-09-22-teams-message-extensions-power-automate/05.png)

**For a selected message**

This starting action gives us an adaptive card that can be shown to the user, as they select the message.

The below adaptive card allows users to select a category and add optional notes while reporting profanity.

```json
{
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "type": "AdaptiveCard",
    "version": "1.3",
    "body": [
        {
            "type": "TextBlock",
            "text": "Why do you think this is inappropriate content?",
            "weight": "Bolder",
            "size": "Medium"
        },
        {
            "type": "TextBlock",
            "text": "Category:",
            "wrap": true,
            "size": "Small"
        },
        {
            "type": "Input.ChoiceSet",
            "choices": [
                {
                    "title": "Dangerous or exploitative content",
                    "value": "Dangerous or exploitative content"
                },
                {
                    "title": "Demeaning or inflammatory content",
                    "value": "Demeaning or inflammatory content"
                },
                {
                    "title": "Personal attacks",
                    "value": "Personal attacks"
                },
                {
                    "title": "Misrepresentative content",
                    "value": "Misrepresentative content"
                },
                {
                    "title": "Misleading synthetic or manipulated content",
                    "value": "Misleading synthetic or manipulated content"
                },
                {
                    "title": "Harmful activity",
                    "value": "Harmful activity"
                },
                {
                    "title": "Distasteful content",
                    "value": "Distasteful content"
                },
                {
                    "title": "Profanity and vulgarity",
                    "value": "Profanity and vulgarity"
                },
                {
                    "title": "Violence",
                    "value": "Violence"
                }
            ],
            "placeholder": "Select category",
            "id": "ICCategory"
        },
        {
            "type": "TextBlock",
            "text": "Optional Notes:",
            "size": "Small"
        },
        {
            "placeholder": "Message to the reviewer",
            "type": "Input.Text",
            "spacing": "None",
            "id": "messageToSend",
            "isMultiline": true
        }
    ]
}
```

**Detect profanity and match against custom and shared block lists**

The next action from the Content Moderator connector helps to detect profanity, by passing in the selected message content.

![](/media/2022-09-22-teams-message-extensions-power-automate/06.png)

**Variable to store profanity terms**

Next, we are initializing a variable (named varProfanityTerms) to store the list of reported profanity terms.

If review is recommended by the Content Moderator service, we populate the variable with reported profanity terms.

![](/media/2022-09-22-teams-message-extensions-power-automate/07.png)

The last part of the flow is to send an adaptive card to the reviewer. We are initializing a 1:1 chat with the reviewer before sending the adaptive card. The card is now populated with the required information.

![](/media/2022-09-22-teams-message-extensions-power-automate/08.png)

Below is the sample adaptive card sent to the reviewer.

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.4",
    "body": [
        {
            "type": "TextBlock",
            "text": "Reporting of inappropriate content",
            "id": "Title",
            "spacing": "Medium",
            "horizontalAlignment": "Center",
            "size": "ExtraLarge",
            "weight": "Bolder",
            "color": "Accent"
        },
        {
            "type": "TextBlock",
            "wrap": true,
            "text": "I would like to bring your attention to below inappropriate content.",
            "spacing": "Medium"
        },
        {
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "Content reported by",
                            "wrap": true,
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "text": "Content created by",
                            "wrap": true,
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "text": "Content",
                            "wrap": true,
                            "weight": "Bolder"
                        }
                    ]
                },
                {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "@{triggerBody()?['entity']?['teamsFlowRunContext']?['from']?['name']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{triggerBody()?['entity']?['teamsFlowRunContext']?['messagePayload']?['from']?['user']?['displayName']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{triggerBody()?['entity']?['teamsFlowRunContext']?['messagePayload']?['body']?['plainText']}",
                            "wrap": true
                        }
                    ]
                }
            ]
        },
        {
            "type": "TextBlock",
            "text": "Content Moderator Ratings",
            "wrap": true,
            "spacing": "Large",
            "size": "Medium",
            "weight": "Bolder",
            "color": "Accent"
        },
        {
            "type": "TextBlock",
            "text": "Below are the ratings reported by the Content moderator.",
            "wrap": true
        },
        {
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                        {
                            "type": "TextBlock",
                            "wrap": true,
                            "text": "Category1 Score",
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "wrap": true,
                            "text": "Category2 Score",
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "wrap": true,
                            "text": "Category3 Score",
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "wrap": true,
                            "text": "Review Recommended?",
                            "weight": "Bolder"
                        },
                        {
                            "type": "TextBlock",
                            "wrap": true,
                            "text": "Detected profanity terms",
                            "weight": "Bolder"
                        }
                    ]
                },
                {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "@{outputs('Detect_profanity_and_match_against_custom_and_shared_block_lists')?['body/Classification/Category1/score']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{outputs('Detect_profanity_and_match_against_custom_and_shared_block_lists')?['body/Classification/Category2/score']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{outputs('Detect_profanity_and_match_against_custom_and_shared_block_lists')?['body/Classification/Category3/score']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{outputs('Detect_profanity_and_match_against_custom_and_shared_block_lists')?['body/Classification/ReviewRecommended']}",
                            "wrap": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "@{variables('varProfanityTerms')}",
                            "wrap": true
                        }
                    ]
                }
            ]
        },
        {
            "type": "TextBlock",
            "text": "Let us make our workplace better every day!",
            "wrap": true,
            "spacing": "Large",
            "weight": "Lighter"
        }
    ],
    "actions": [
        {
            "type": "Action.OpenUrl",
            "title": "Open message",
            "url": "@{triggerBody()?['entity']?['teamsFlowRunContext']?['messagePayload']?['linkToMessage']}"
        }
    ]
}
```

**Test the messaging extension**

On a message, select the **Report inappropriate content** messaging extension if you want to report any profanity.

![](/media/2022-09-22-teams-message-extensions-power-automate/09.png)

Select the appropriate category under which you want to report this and add optional notes to the reviewer.

The suspected message information will be sent to a person or group configured in the power automate for review and action.

**The reviewer's view**

This reviewer view from Amanda shows how the information is sent as an adaptive card to the reviewer. To make it easier for the reviewer to take the best decision on the reported content, we are using the Content Moderator connector which reports the score in 3 categories and Detected profanity terms.

![](/media/2022-09-22-teams-message-extensions-power-automate/10.png)

**Points to note:**

- You must create these flows within the default environment for them to appear in Teams.
- Power Automate Actions app must be enabled in the Teams admin center.

**Best Practices:**

- Be sure to include a form of confirmation to the user after the flow is completed.
- Consider notifying users by sending Bot messages as an adaptive card.

**Limitations:**

- Only the flow author can trigger the flow.
- The flow will only be available to other members of the channel/chat if the author explicitly shares it with them.

## Summary

Building messaging extensions for MS Teams with Power Automate opens up new opportunities for citizen developers to build their use cases. With the power of Power Automate, you can connect to various services to build powerful scenarios.

## References

- [Trigger flows from Teams messages](https://learn.microsoft.com/en-us/power-automate/trigger-flow-teams-message?WT.mc_id=M365-MVP-5003693)
- [Messaging extensions in Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/messaging-extensions/what-are-messaging-extensions?WT.mc_id=M365-MVP-5003693)
