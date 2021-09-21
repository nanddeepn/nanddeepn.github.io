---
title: "Build Adaptive Card Extension with SPFx"
date: "2021-09-21"
share: true
header:
  image: media/2021-09-21-build-ace-spfx/03.png
  teaser: media/2021-09-21-build-ace-spfx/03.png
categories:
  - SharePoint Framework
  - Adaptive Card Extension
  - Microsoft Viva
tags:
  - "2021"
  - September 2021
last_modified_at: 2021-09-21T00:00:00-00:00
---
## Overview

SPFx v1.13 supports Adaptive Card Extensions (ACEs) component type which enables developers to build a rich experience for Viva Connections' Dashboards and SharePoint Pages.

In this article, we will explore the ACE component types and build our first SPFx experience with ACE.

## Adaptive Card Extensions Overview

Adaptive Card Extensions (ACE) uses Adaptive Card Framework with declarative JSON schema to generate UI, so the developer can focus on the business logic rather than thinking of complex JSON building on their own.

ACEs support below list of templates:
- Basic Card Template
- Image Card Template
- Primary Text Template

Install the latest beta release by running the below command:

```
npm install @microsoft/generator-sharepoint@next --global
```

## Scaffold Adaptive Card Extensions (ACE) solution

Start the scaffolding process by running the below command:

```
yo @microsoft/sharepoint
```

![](/media/2021-09-21-build-ace-spfx/01.png)

## Test Basic Card Template

Follow the below steps to test the basic card template:

1. Execute the below command to run the solution:

    ```
    gulp serve -l --nobrowser
    ```

2. On the browser, navigate to SharePoint hosted workbench.

3. Add ACE to the page.

    ![](/media/2021-09-21-build-ace-spfx/02.png)

4. Click on **Quick View** to see the basic card popping out.

    ![](/media/2021-09-21-build-ace-spfx/03.png)

5. Click the **Edit** icon to see the property pane.

    ![](/media/2021-09-21-build-ace-spfx/04.png)

> Note:
> ACE interactions are only supported when the Workbench or Page is in Preview or Read mode.

## Code Walkthrough

On the command prompt, type `code .` to open the solution in the code editor (e.g. Visual Studio Code).

### ACE Class

The file `src\adaptiveCardExtensions\basicCard\BasicCardAdaptiveCardExtension.ts` contains the definition of an ACE, which extends from the `BaseAdaptiveCardExtension` class.

```typescript
export default class BasicCardAdaptiveCardExtension extends BaseAdaptiveCardExtension<
  IBasicCardAdaptiveCardExtensionProps,
  IBasicCardAdaptiveCardExtensionState
> {

    // ...

}
```

Here we can optionally implement two generics:
1. TProperties: Set of persisted properties of the component (a property bag)
2. TState: State of ACE


**Render ACE**

The virtual `renderCard()` method returns the string identifier to the registered view. This method is invoked during the initial render of the Card view.

```typescript
protected renderCard(): string | undefined {
    return CARD_VIEW_REGISTRY_ID;
}
```

If this method is commented, quick view will not render. Unlike with the Card view, there is no default Quick view.


**Register View for ACE**

View must be registered within class' constructor or `onInit()` method before we use it.

```typescript
public onInit(): Promise<void> {
    ...

    this.cardNavigator.register(CARD\_VIEW\_REGISTRY\_ID, () => new CardView());
    this.quickViewNavigator.register(QUICK\_VIEW\_REGISTRY\_ID, () => new QuickView());

    return Promise.resolve();
}
```

### Card View

Card view can be found at `src\adaptiveCardExtensions\basicCard\cardView\CardView.ts`

It must extend from any of below base classes:
- BaseBasicCardView
- BaseImageCardView
- BasePrimaryTextCardView

Two generics for the properties and state objects are shared between the view and the ACE.

**data**
The `data` getter is the only method that must be implemented by a Card view.

```typescript
public get data(): IBasicCardParameters {
    return {
      primaryText: strings.PrimaryText
    };
}
```

> Note: 
> The return type is unique to the parent class of the View.

**cardButtons**

The `cardButtons` property determines the number of buttons that appear on the Card and actions to perform when clicked.

```typescript
public get cardButtons(): [ICardButton] | [ICardButton, ICardButton] | undefined {
    return [
      {
        title: strings.QuickViewButton,
        action: {
          type: 'QuickView',
          parameters: {
            view: QUICK_VIEW_REGISTRY_ID
          }
        }
      },
      {
        title: 'Microsoft',
        action: {
          type: 'ExternalLink',
          parameters: {
            target: 'https://www.microsoft.com'
          }
        }
      }
    ];
}
```

**onCardSelection**
This method determines what will happen when the Card is clicked.

```typescript
public get onCardSelection(): IQuickViewCardAction | IExternalLinkCardAction | undefined {
    return {
        type: 'ExternalLink',
        parameters: {
            target: 'https://www.bing.com'
        }
    };
}
```

### Quick View
Quick view can be found at `src\adaptiveCardExtensions\basicCard\quickView\QuickView.ts`.

Quick views extend the `BaseAdaptiveCardView` base class with below three optional generics:
1. TProperties: Interface used by persisted properties of the ACE (a property bag).
2. TState: Set of stateful data the View needs to render.
3. TData: The type returned from the data() getter method.


**template()**

The `template()` getter returns valid Adaptive Card template JSON.

```typescript
public get template(): ISPFxAdaptiveCard {
    return require('./template/QuickViewTemplate.json');
}
```

The properties on the object returned from the data getter will automatically be mapped to the bound template slot.

```typescript
public get data(): IQuickViewData {
    return {
        subTitle: strings.SubTitle,
        title: strings.Title,
        description: this.properties.description
    };
}
```

The template slot can be found at `src\adaptiveCardExtensions\basicCard\quickView\template\QuickViewTemplate.json`.

```json
{
    "type": "TextBlock",
    "text": "${description}",
    "wrap": true
}
```

## Summary

SPFx v1.13 supports Adaptive Card Extensions (ACEs) component type which enables developers to build a rich experience for Viva Connections' Dashboards and SharePoint Pages.

## References

- [Build your first SharePoint Adaptive Card Extension](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/build-first-sharepoint-adaptive-card-extension?WT.mc_id=M365-MVP-5003693)
- [Advanced Card View Functionality](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/advanced-card-view-functionality?WT.mc_id=M365-MVP-5003693)
- [Advanced Quick View Functionality](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/advanced-quick-view-functionality?WT.mc_id=M365-MVP-5003693)
- [Adaptive Card Extensions and Teams Apps](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/adaptive-card-extensions-and-teams?WT.mc_id=M365-MVP-5003693)
