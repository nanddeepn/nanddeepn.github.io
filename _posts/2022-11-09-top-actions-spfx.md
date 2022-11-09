---
title: "Using Web part Top Actions in SPFx"
date: "2022-11-09"
share: true
header:
  image: media/2022-11-09-top-actions-spfx/02.png
  teaser: media/2022-11-09-top-actions-spfx/02.png
categories:
  - SharePoint Framework
tags:
  - "2022"
  - November 2022
last_modified_at: 2022-11-09T00:00:00-00:00
---
## Overview

SPFx v1.16 has introduced top actions for web parts which allow developers to add custom actions to the command bar of a web part.

In this article, we will get introduced to using these top actions for web parts.

## Scaffold SPFx ACE solution

Let us start by scaffolding the SPFx web part solution to get familiar with the top actions.

![](/media/2022-11-09-top-actions-spfx/01.png)

## Add top actions

This feature is similar to property pane controls. Implement getTopActionsConfiguration to start adding top actions.

The implementation expects below:

1. `topActions` as a list of PropertyPane top actions.
2. `onExecute` method, which gets triggered when the top action configuration state has changed for the specified property path.

```typescript
public getTopActionsConfiguration(): ITopActions {
    return {
      topActions: [
        {
          type: PropertyPaneFieldType.Button,
          targetProperty: 'sayHello',
          properties: {
            text: 'Say Hello!',
            icon: 'GreetingCard'
          }
        },
        {
          ...PropertyPaneChoiceGroup('selectColor', {
            label: 'Select color',
            options: [
              {
                key: 'red',
                text: 'Red'
              },
              {
                key: 'yellow',
                text: 'Yellow'
              },
              {
                key: 'green',
                text: 'Green'
              }
            ]
          }),
          title: 'My Top Bar'
        }
      ],
      onExecute(actionName, newValue) {
        switch(actionName) {
          case 'sayHello':
            alert('Hello');
            break;
          case 'selectColor':
            alert(`Selected color: ${newValue}`);
            break;
        }
      },
    }
}
```


## Top Actions in the action

The top actions for the web part will be only visible in the edit page experience to configure the web part.

![](/media/2022-11-09-top-actions-spfx/02.png)


## Summary

Top actions for web parts allow developers to add custom actions to the command bar of a web part. They can be configured in the edit page mode.


## Code Download

The SPFx code developed for this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-top-actions).


## References

- [SharePoint Framework v1.16 release notes](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/release-1.16?WT.mc_id=M365-MVP-5003693)
- [New features in SharePoint Framework 1.16 preview](https://www.youtube.com/watch?v=b2gDvgpv1e8)
