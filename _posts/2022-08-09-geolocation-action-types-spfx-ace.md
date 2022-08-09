---
title: "Using geolocation Action Types in SPFx ACEs"
date: "2022-08-09"
share: true
header:
  image: media/2022-08-09-geolocation-action-types-spfx-ace/preview.gif
  teaser: media/2022-08-09-geolocation-action-types-spfx-ace/preview.gif
categories:
  - SharePoint Framework
tags:
  - "2022"
  - August 2022
last_modified_at: 2022-08-09T00:00:00-00:00
---

## Overview

SPFx v1.15 has introduced new action types for geolocation to use in Adaptive Card Extensions to get a location and show a location on the map.

In this article, we will get introduced to using these new geolocation action types.

## Scaffold SPFx ACE solution

Let us start by scaffolding the SPFx ACE solution to get familiar with new geolocation action types.

![](/media/2022-08-09-geolocation-action-types-spfx-ace/01.png)

Update the Quick View template JSON as follows:

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
    }
  ],
  "actions": [
    {
      "type": "VivaAction.GetLocation",
      "id": "Get Location",
      "title": "Get Location",
      "parameters": {
        "chooseLocationOnMap": true
      }
    },
    {
      "type": "VivaAction.ShowLocation",
      "id": "Show Location",
      "title": "Show Location",
      "parameters": {
        "locationCoordinates": {
          "latitude": 18.5204,
          "longitude": 73.8567
        }
      }
    }
  ]
}
```

The quick view is shown as follows:

![](/media/2022-08-09-geolocation-action-types-spfx-ace/02.png)

We will use the `onAction` method to get the selected location coordinates.

```typescript
public onAction(action: IActionArguments): void {
    if (action.type === 'VivaAction.GetLocation') {
        console.log(action.location.latitude);
        console.log(action.location.longitude);
    }
}
```

![](/media/2022-08-09-geolocation-action-types-spfx-ace/preview.gif)

With the `GetLocation` action, you can get your current location or choose a custom location on the Bing map. Whereas `ShowLocation` action helps you to show the location with latitude and longitude on the Bing map.

## Summary

The geolocation action type is useful in Adaptive Card Extensions to get a location and show a location on the map. You can use GetLocation and ShowLocation action to work with the location on the Bing map.

## Code Download

The SPFx code developed for this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/SPFx/ACEs/geolocation-action-types).

## References

- [SharePoint Framework v1.15 release notes](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/release-1.15?WT.mc_id=M365-MVP-5003693)
