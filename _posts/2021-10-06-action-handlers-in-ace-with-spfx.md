---
title: "Action Handlers in Adaptive Card Extension with SPFx"
date: "2021-10-06"
share: true
header:
  image: media/2021-10-06-action-handlers-in-ace-with-spfx/01.png
  teaser: media/2021-10-06-action-handlers-in-ace-with-spfx/01.png
categories:
  - SharePoint Framework
  - Adaptive Card Extension
  - Microsoft Viva
tags:
  - "2021"
  - October 2021
last_modified_at: 2021-10-06T00:00:00-00:00
---
## Overview

In the previous article [Build Adaptive Card Extension with SPFx](https://nanddeepnachanblogs.com/posts/2021-09-21-build-ace-spfx/) we explored the ACE component types and build our first SPFx experience with ACE.

In this article, we will extend it further and write our action handlers.

## Add Actions

We will start by adding actions to Quick View template at `src\adaptiveCardExtensions\basicCard\quickView\template\QuickViewTemplate.json`

```json
{
  "schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.2",
  "body": [
    {
      "type": "TextBlock",
      "weight": "Bolder",
      "text": "${title}"
    },
    {
      "type": "ColumnSet",
      "columns": [
        {
          "type": "Column",
          "items": [
            {
              "type": "TextBlock",
              "weight": "Bolder",
              "text": "${subTitle}",
              "wrap": true
            }
          ]
        }
      ]
    },
    {
      "type": "TextBlock",
      "text": "${description}",
      "wrap": true
    }
  ]
}
```

![](/media/2021-10-06-action-handlers-in-ace-with-spfx/01.png)

We now have 2 buttons added to our Quick View. However, they do not respond to a button click.

## Action handlers

Actions are handled by the views where they're defined. The button click initiates the Action.Submit action when clicked which can be handled inside `onAction()` method.

Open `QuickView.ts` file at `src\adaptiveCardExtensions\basicCard\quickView\QuickView.ts` to handle button click events.

```typescript
import { ISPFxAdaptiveCard, BaseAdaptiveCardView } from '@microsoft/sp-adaptive-card-extension-base';

public onAction(action: IActionArguments): void {
  if (action.type === 'Submit') {
    const { id, message } = action.data;

    switch (id) {
      case 'button1':
      case 'button2':
        this.setState({
          description: message
        });
        break;
    }
  }
}
```

Update `data` getter method to set the description from the state.

```typescript
public get data(): IQuickViewData {
  return {
    subTitle: strings.SubTitle,
    title: strings.Title,
    description: this.state.description
  };
}
```

![](/media/2021-10-06-action-handlers-in-ace-with-spfx/02.png)


## Summary

Actions are handled by the views where they are defined. The button click initiates the `Action.Submit` action when clicked which can be handled inside `onAction()` method.


## References

- [Build your first SharePoint Adaptive Card Extension](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/build-first-sharepoint-adaptive-card-extension?WT.mc_id=M365-MVP-5003693)
- [Advanced Card View Functionality](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/advanced-card-view-functionality?WT.mc_id=M365-MVP-5003693)
- [Advanced Quick View Functionality](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/advanced-quick-view-functionality?WT.mc_id=M365-MVP-5003693)
- [Adaptive Card Extensions and Teams Apps](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/adaptive-card-extensions-and-teams?WT.mc_id=M365-MVP-5003693)
