---
title: "Using Office UI Fabric Icons in SPFx solution for MS Teams App"
date: "2022-04-25"
share: true
header:
  image: media/2022-04-25-office-icons-spfx-for-ms-teams/04.png
  teaser: media/2022-04-25-office-icons-spfx-for-ms-teams/04.png
categories:
  - SharePoint
  - SharePoint Framework
  - MS Teams
tags:
  - "2022"
  - April 2022
last_modified_at: 2022-04-25T00:00:00-00:00
---
## Overview

The web parts built with SharePoint Framework can be easily used in Microsoft Teams as an app. Building the web parts which can work in SharePoint and MS Teams is a seamless experience. However few things might not work as expected.

In this article, we will look into one such experience with the Fluent UI icons. The icons work perfectly fine in SharePoint but need some tweaking to make them work in MS Teams.

## The icon issue

Based on the version you are using, call it a Fluent UI or Office UI Fabric throughout the article. The nice little icons help build engaging UI.

Let us start by importing the icon to the React component of our web part.

```typescript
import { IconButton, IIconProps} from 'office-ui-fabric-react';

const emoji2Icon: IIconProps = { iconName: 'Emoji2' };
```

Add the icon button to the render method as follows:

```html
<IconButton iconProps={emoji2Icon} title="I am happy!" />
```

**Build and deploy the package**

Let us build the package with the below magic commands:

```powershell
gulp bundle --ship

gulp package-solution --ship
```

Upload the solution package (.sppkg) to SharePoint tenant app catalog. Click **Enable app**.

![](/media/2022-04-25-office-icons-spfx-for-ms-teams/01.png)

Once the app is enabled, click **Add to Teams** to get the SPFx web part added as the Teams app.

![](/media/2022-04-25-office-icons-spfx-for-ms-teams/02.png)

Add the web part as Teams personal app or Teams tab. One thing, you will notice is an absence of icon. In the browser console, you will observe the warning about the icon.

![](/media/2022-04-25-office-icons-spfx-for-ms-teams/03.png)

## The Solution

As per the resolution mentioned in the reference URL (i.e., [https://github.com/microsoft/fluentui/wiki/Using-icons](https://github.com/microsoft/fluentui/wiki/Using-icons)), 
*By default, the font-based Fluent UI icons are not added to your bundle or loaded on the page, in order to save bytes for scenarios where you don't care about icons, or you only care about a subset.*

If you have upgraded to use Fluent UI, add below import to the web part.

```typescript
import { initializeIcons } from '@fluentui/react/lib/Icons';
```

If you are using Office UI Fabric icons, add the below import to the web part.

```typescript
import { initializeIcons } from 'office-ui-fabric-react';
```

In the `onInit` method, call `initializeIcons()`.

```typescript
protectedonInit(): Promise<void> {
    this._environmentMessage = this._getEnvironmentMessage();

    if (this.context.sdks.microsoftTeams) {
      initializeIcons();
    }

    return super.onInit();
  }
```

Package and deploy the solution again to the SharePoint tenant app catalog. The icon is now visible in the MS Teams app.

![](/media/2022-04-25-office-icons-spfx-for-ms-teams/04.png)

## Summary

SharePoint Framework solutions work best for both SharePoint and MS Teams. Fluent UI icons need some extra configuration to make them work in MS Teams app.

## References
- [Getting started with SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant?WT.mc_id=M365-MVP-5003693)
- [Building for Microsoft teams](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/build-for-teams-overview?WT.mc_id=M365-MVP-5003693)
- [Fluent UI icons](https://developer.microsoft.com/en-us/fluentui#/styles/web/icons?WT.mc_id=M365-MVP-5003693)
