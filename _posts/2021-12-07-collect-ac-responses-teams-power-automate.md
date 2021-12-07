---
title: "Collect responses from Adaptive Cards on MS Teams using Power Automate"
date: "2021-12-07"
share: true
header:
  image: media/2021-12-07-collect-ac-responses-teams-power-automate/06.png
  teaser: media/2021-12-07-collect-ac-responses-teams-power-automate/06.png
categories:
  - Adaptive Cards
  - MS Teams
  - Power Automate
tags:
  - "2021"
  - December 2021
last_modified_at: 2021-12-07T00:00:00-00:00
---
## Overview

Adaptive cards are platform agnostic snippets of UI authored in json format. Adaptive cards can be sent from Power Automate to MS Teams to collect the information and the response can be recorded from the users.

In this article, we will explore to send Adaptive cards to MS Teams and collect the responses to Microsoft Excel stored in SharePoint or OneDrive.

## Send Adaptive Card to MS Teams

To send an Adaptive Card on MS Teams using Power Automate, follow below steps:
1. Open [Power Automate](https://flow.microsoft.com).
2. Create a flow based on your trigger preference. To keep it simple, let us create **Instant cloud flow**.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/01.png)

3. Add an action **Post adaptive card and wait for a response**.
4. Specify the **Post in** as **Chat with Flow bot** and **Recipient** details to post an adaptive card to. Please note, you can specify a single name or email address. To send the adaptive card to multiple recipients, loop through all recipients.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/02.png)

5. Add below Adaptive card json as Message on the above added action.

    ```json
    {
        "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
        "type": "AdaptiveCard",
        "version": "1.0",
        "body": [
            {
                "type": "TextBlock",
                "text": "Poll Request",
                "id": "Title",
                "spacing": "Medium",
                "horizontalAlignment": "Center",
                "size": "ExtraLarge",
                "weight": "Bolder",
                "color": "Accent"
            },
            {
                "type": "TextBlock",
                "text": "Power Automate Poll",
                "id": "acHeaderTagLine",
                "separator": true
            },
            {
                "type": "TextBlock",
                "text": "Preferred Meal Option",
                "weight": "Bolder",
                "size": "ExtraLarge",
                "spacing": "None",
                "id": "acHeader"
            },
            {
                "type": "TextBlock",
                "text": "We are arranging a year end part and polling our employees in order to share their food preference.",
                "id": "acInstructions",
                "wrap": true
            },
            {
                "type": "TextBlock",
                "text": "Please vote for your food preference from the choices listed here.",
                "id": "acPollQuestion"
            },
            {
                "type": "Input.ChoiceSet",
                "placeholder": "Select from these choices",
                "choices": [
                    {
                        "title": "Veg",
                        "value": "Veg"
                    },
                    {
                        "title": "Vegan",
                        "value": "Vegan"
                    },
                    {
                        "title": "Non-Veg",
                        "value": "Non-Veg"
                    }
                ],
                "id": "acPollChoices",
                "style": "expanded"
            }
        ],
        "actions": [
            {
                "type": "Action.Submit",
                "title": "Submit",
                "id": "btnSubmit"
            }
        ]
    }
    ```

    You can design your Adaptive card with [designer](https://adaptivecards.io/designer/).
    The update message will be shown to the user when they will submit their choice on the adaptive card. This functionality eases the developer life, because now they do not need to send the update card.


## Record user responses to Excel file

Once the Poll is sent to the recipients as an adaptive card, the team members will respond to it. We need to collect their responses to process further and take the actions.

Follow the below steps to create and configure an Excel file in OneDrive:
1. Create an Excel file named **Poll Responses.xlsx** in your **OneDrive**.
2. In this file, insert a table with two columns (use the Insert > Table function of Excel).

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/03.png)

3. Optionally, name your table from **Table Design** menu. 
4. Optionally, rename the columns.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/04.png)

5. Save and close the Excel file.

Follow the below steps to record the responses in an Excel file stored in OneDrive:
1. Navigate to previously created Power Automate flow.
2. Add an action **Add a row into a table** under **Excel Online**.
3. Specify the location to Excel file stored in OneDrive.
4. Select the Table and columns from Excel file.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/05.png)

5. **Save** the flow.

## Test the functionality

To test the functionality, follow below steps:
1. Execute the Power Automate flow manually.
2. An adaptive card will be sent to the recipients.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/06.png)

3. Make your selection in Adaptive card, click Submit.
4. The configured update message will be shown to the user.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/07.png)

5. The Excel file in OneDrive should have the responses recorded.

    ![](/media/2021-12-07-collect-ac-responses-teams-power-automate/08.png)


## Summary

Adaptive cards can be sent from Power Automate to MS Teams to collect the information and the response can be recorded from the users.

## References

- [Create your first adaptive card](https://docs.microsoft.com/en-us/power-automate/create-adaptive-cards?WT.mc_id=M365-MVP-5003693)
- [Overview of Adaptive Cards for Microsoft Teams](https://docs.microsoft.com/en-us/power-automate/overview-adaptive-cards?WT.mc_id=M365-MVP-5003693)
- [Use adaptive cards in Microsoft Teams](https://docs.microsoft.com/en-us/power-automate/create-adaptive-cards-teams?WT.mc_id=M365-MVP-5003693)
- [Power Automate and Adaptive Cards in Microsoft Teams](https://docs.microsoft.com/en-us/power-platform-release-plan/2019wave2/power-automate/contextual-flows-power-automation-teams?WT.mc_id=M365-MVP-5003693)