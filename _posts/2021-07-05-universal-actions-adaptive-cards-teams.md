---
title: "Universal Actions for Adaptive Cards on Microsoft Teams"
date: "2021-07-05"
share: true
header:
  image: media/2021-07-05-universal-actions-adaptive-cards-teams/preview.gif
  teaser: media/2021-07-05-universal-actions-adaptive-cards-teams/preview.gif
categories:
  - Adaptive Cards
  - MS Teams
tags:
  - "2021"
  - July 2021
last_modified_at: 2021-07-05T00:00:00-00:00
---

## Overview

Adaptive Cards are familiar to developers for building platform agnostic UI snippets written in JSON. Adaptive Cards offers lightweight UI which can be used across various apps including Microsoft Teams and Outlook. More about it can be read at [https://adaptivecards.io/](https://adaptivecards.io/)

In this article, we will explore the newer capabilities of `Adaptive Cards v1.4` including universal actions, user specific views, sequential workflows, and up to date cards..

## Why Universal Actions?

Adaptive Cards when used in Microsoft Teams via Bot, we used `Action.Submit`.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            ...
        }
    ],
    "actions": [
        {
            "type": "Action.Submit",
            "title": "OK"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

While Adaptive Cards when used in Microsoft Outlook via Actionable messages, we used `Action.Http`.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            ...
        }
    ],
    "actions": [
        {
            "type": "Action.Http",
            "title": "OK"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

Although for the end user point of view Adaptive Cards provided a unified experience across the platform, however from the developers' point of view action handling was not universal. The universal action model has solved this gracefully.

Adaptive Cards v1.4 have below major offerings:

### Action.Execute

With Universal Actions, we can use `Action.Execute` which works across both Microsoft Teams and Outlook.

### Refresh model

Universal Actions offers refresh clause to automatically refresh the Adaptive Card.

```json
{
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "type": "AdaptiveCard",
    "version": "1.4",
    "refresh": {
        "action": {
            "type": "Action.Execute",
            "title": "OK",
            "verb": "cardRefresh"
        },
        "userIds": []
    },
    "body": [
        ...
        {
            "type": "ActionSet",
            "actions": [
                {
                    "type": "Action.Execute",
                    "title": "Submit",
                    "verb": "formSubmit",
                    "fallback": "Action.Submit"
                }
            ]
        }
    ]
}
```

Specify the `fallback` property with value as `Action.Submit` to ensure compatibility with pervious versions of the Microsoft Teams client.

The `userIds` is an array of MRIs of users for whom the adaptive cards will be refreshed automatically at the time when it is displayed. This refresh mechanism will ensure that the users always see the up to date information. If the array is left empty, refresh card button will be displayed for all the users to manually refresh the card.

User MRIs in Microsoft Teams can be retrieved using below code:

```cs
var members = new List<TeamsChannelAccount>();
string continuationToken = null;

do
{
    var currentPage = await TeamsInfo.GetPagedMembersAsync(turnContext, 100, continuationToken, cancellationToken);
    continuationToken = currentPage.ContinuationToken;
    members.AddRange(currentPage.Members);
}
while (continuationToken != null);

foreach (var member in members)
{
    if (member.AadObjectId != turnContext.Activity.From.AadObjectId)
    {
        // AadObjectId is the User MRI
        var newMemberInfo = new MemberDetails { value = member.AadObjectId, title = member.Name };
        memberDetails.Add(newMemberInfo);
    }
}
```

Limitations with userIds:

- Maximum 60 user IDs can be specified.
- This property will be ignored in Outlook.

## Get started with Universal Actions for Adaptive Cards in Microsoft Teams

To start using Universal Actions, make below changes to your code.

1. Replace all instances of `Action.Submit` with `Action.Execute`.
2. Add a `refresh` model to Adaptive Card (if required)
3. Handle `adaptiveCard/action` invoke requests in your bot.

    ```cs
    protected override async Task<InvokeResponse> OnInvokeActivityAsync(ITurnContext<IInvokeActivity> turnContext, CancellationToken cancellationToken)
    {
        if (turnContext.Activity.Name == "adaptiveCard/action")
        {
            ...
        }
        ...
    }
    ```

4. Respond back with the card using `CreateInvokeResponse`.

## Use Case - Incident Management

Here is an use case for handling incidents in Microsoft Teams using Adaptive Cards. Below are the actors on this:

- Nanddeep Nachan (Left window): Incident creator
- Debra Berger (middle window): Incident resolver
- Smita Nachan (right window): Member of Microsoft Teams channel

![](/media/2021-07-05-universal-actions-adaptive-cards-teams/preview.gif)

The functionality works as follows:

1. Nanddeep Nachan (incident creator) raises an incident by calling bot with @ mention
2. With Refresh model, only incident creator sees the option to create an incident while other users see the message of incident being created.
3. After incident creation, bot sends an Adaptive card with refresh Ids for incident creator and incident resolver.
4. Now incident creator sees an option to cancel the incident, while incident resolver sees an option to resolve. For other users (members) they just see incident information, but they cannot take any action.
5. When the incident is cancelled or resolved, the card is refreshed for all users to see the up to date information.

## Code Download

The code developed during this article can be found at: [https://github.com/nanddeepn/code-samples/tree/master/MSTeams/bot-teams-incidentmanagement](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/bot-teams-incidentmanagement)

## Summary

Adaptive Cards v1.4 offers unified experience for the developers. The developers can now take advantages of universal actions, user specific views, sequential workflows, and up to date cards.

## References

- [Adaptive Cards](https://adaptivecards.io/)
- [Universal Actions for Adaptive Cards](https://docs.microsoft.com/en-us/microsoftteams/platform/task-modules-and-cards/cards/universal-actions-for-adaptive-cards/overview?WT.mc_id=M365-MVP-5003693)
- [User Specific Views](https://docs.microsoft.com/en-us/microsoftteams/platform/task-modules-and-cards/cards/universal-actions-for-adaptive-cards/user-specific-views/?WT.mc_id=M365-MVP-5003693)
- [Sequential Workflows](https://docs.microsoft.com/en-us/microsoftteams/platform/task-modules-and-cards/cards/universal-actions-for-adaptive-cards/sequential-workflows/?WT.mc_id=M365-MVP-5003693)
- [Up to date cards](https://docs.microsoft.com/en-us/microsoftteams/platform/task-modules-and-cards/cards/universal-actions-for-adaptive-cards/up-to-date-views/?WT.mc_id=M365-MVP-5003693)
- [Universal Action Model](https://docs.microsoft.com/en-us/adaptive-cards/authoring-cards/universal-action-model/?WT.mc_id=M365-MVP-5003693)
- [sequential-workflow-adaptive-card](https://github.com/OfficeDev/Microsoft-Teams-Samples/tree/main/samples/bot-sequential-flow-adaptive-cards/csharp)

## Video Refences

- [Deep dive into Universal Actions for Microsoft Teams and Outlook](https://www.youtube.com/watch?v=mwWAFw8df50)) by [Shiladitya Saha](https://www.linkedin.com/in/shiladityasaha/) (Microsoft - Teams) and [Karan Thapar](https://www.linkedin.com/in/karanthapar91/) (Microsoft - Outlook)

## Thank You!

Special thanks to [Shiladitya Saha](https://www.linkedin.com/in/shiladityasaha/), [Wictor Wilen](https://www.linkedin.com/in/wictorw/), and [Rabia Williams](https://www.linkedin.com/in/rabiawilliams/) (Microsoft) for their valuable discussions and guidance on this interesting topic.
