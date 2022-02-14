---
title: "Paging for SPFx solutions with PnP Pagination Control"
date: "2022-02-14"
share: true
header:
  image: media/2022-02-14-paging-spfx-pnp-navigation/demo.gif
  teaser: media/2022-02-14-paging-spfx-pnp-navigation/demo.gif
categories:
  - SharePoint
  - SharePoint Framework
tags:
  - "2022"
  - February 2022
last_modified_at: 2022-02-14T00:00:00-00:00
---
## Overview

Paging is a nice way to get faster access to data as well as give a nice user experience to browse through huge data. While working with SharePoint lists and libraries having a huge number of items or documents paging makes the navigation through data easy.

In this article, we will explore using the PnP Control for paging (Pagination Control) to navigate from a large list.

## Develop SPFx Solution

We will develop the SPFx web part as shown below:

![](/media/2022-02-14-paging-spfx-pnp-navigation/01.png)

In the solution, create a model at `src\webparts\pnPPagination\models\ISPItem.ts` to represent a SharePoint list item.

```typescript
export interface ISPItem {
    title: string;
    description: string;
}
```

In the web part, implement a method to read SharePoint list items. For simplicity, I am using a mock method to return dummy list items. I used the website [Generate Random CSV](https://onlinerandomtools.com/generate-random-csv) to generate some random text for our list data.

```typescript
public getSPListItems(): ISPItem[] {
    const spItems: ISPItem[] = [
        { title: "stove", description: "completely" },
        { title: "rich", description: "know" },
        { title: "composed", description: "explain" },
        { title: "said", description: "simply" },
        { title: "sum", description: "bear" },
        { title: "bowl", description: "exclaimed" },
        { title: "help", description: "drive" },
        { title: "pie", description: "wore" },
        { title: "grade", description: "saw" },
        { title: "butter", description: "personal" },
        { title: "family", description: "being" },
        { title: "occur", description: "gather" },
        { title: "push", description: "industry" },
        { title: "earth", description: "wooden" },
        { title: "went", description: "able" },
        { title: "milk", description: "divide" },
        { title: "art", description: "truck" },
        { title: "arrive", description: "step" },
        { title: "taught", description: "throat" },
        { title: "connected", description: "went" },
        { title: "charge", description: "meet" },
        { title: "having", description: "attached" },
        { title: "expression", description: "sit" },
        { title: "dear", description: "cattle" },
        { title: "closely", description: "immediately" },
        { title: "those", description: "skin" },
        { title: "shore", description: "lake" },
        { title: "meant", description: "answer" },
        { title: "down", description: "correctly" },
        { title: "pair", description: "equipment" },
        { title: "deal", description: "blanket" },
        { title: "garage", description: "hay" },
        { title: "cattle", description: "view" },
        { title: "heavy", description: "moving" },
        { title: "throat", description: "locate" },
        { title: "motor", description: "serve" },
        { title: "gun", description: "alone" },
        { title: "situation", description: "far" },
        { title: "worse", description: "general" },
        { title: "until", description: "globe" },
        { title: "tent", description: "pile" },
        { title: "dot", description: "naturally" },
        { title: "theory", description: "score" },
        { title: "dinner", description: "underline" },
        { title: "solid", description: "gain" },
        { title: "allow", description: "region" },
        { title: "joined", description: "vowel" },
        { title: "form", description: "as" },
        { title: "any", description: "swung" },
        { title: "excited", description: "shine" },
        { title: "enter", description: "dirty" },
        { title: "pile", description: "supply" },
        { title: "piano", description: "help" }
    ];

    return spItems;
}
```

## Define a State

Let us define a state to store all items.

```typescript
import { ISPItem } from '../models/ISPItem';

export interface IPnPPaginationState {
    allItems: ISPItem[];
    paginatedItems: ISPItem[]; 
}
```

Update the `render` method to display the list items.

```typescript
public render(): React.ReactElement<IPnPPaginationProps> {
    return (
      <div className={styles.pnPPagination}>
        <div className={styles.container}>
          <div className={styles.row}>
            {
              this.state.allItems.map((item) =>
                <div>{item.title}</div>
              )
            }
          </div>
        </div>
      </div>
    );
}
```

The result will be displayed as follows:

![](/media/2022-02-14-paging-spfx-pnp-navigation/02.png)

## Paging with PnP Pagination Control

Let us start by installing the dependency to our solution.

**Install @pnp/spfx-controls-react**

We will implement the paging with [PnP pagination control](https://pnp.github.io/sp-dev-fx-controls-react/controls/Pagination/). Run the below command to install PnP Controls in the solution:

```powershell
npm install @pnp/spfx-controls-react --save --save-exact
```


**Update state**

Update the state to include paginated items.

```typescript
import { ISPItem } from '../models/ISPItem';

exportinterfaceIPnPPaginationState {
    allItems: ISPItem[];
    paginatedItems: ISPItem[];
}
```

**Use PnP pagination control**

Import the pagination control to React component as follows:

```typescript
import { Pagination } from '@pnp/spfx-controls-react/lib/pagination';
```

Add the pagination control in the `render` method as follows:

```typescript
public render(): React.ReactElement<IPnPPaginationProps> {
    return (
      <div className={styles.pnPPagination}>
        <div className={styles.container}>
          <div className={styles.row}>
            {
              this.state.paginatedItems.map((item) =>
                <div>{item.title}</div>
              )
            }
            <Pagination
              currentPage={1}
              totalPages={(this.state.allItems.length / pageSize) - 1}
              onChange={(page) => this._getPage(page)}
              limiter={3}
            />
          </div>
        </div>
      </div>
    );
}
```

The selected page in the Pagination control can be retrieved with below method:

```typescript
private _getPage(page: number) {
    // round a number up to the next largest integer.
    const roundupPage = Math.ceil(page);

    this.setState({
      paginatedItems: this.state.allItems.slice(roundupPage * pageSize, (roundupPage * pageSize) + pageSize)
    });
}
```

Update the `componentDidMount` method to show first set of paginated items with pageSize as 5.

```typescript
public componentDidMount(): void {
    const items: ISPItem[] = this.getSPListItems();
    this.setState({ allItems: items, paginatedItems: items.slice(0, pageSize) });
}
```

## The End Result

When deployed, the web part works as below:
![](/media/2022-02-14-paging-spfx-pnp-navigation/demo.gif)


## Summary

Paging is a nicer way to present the large data. PnP Control for paging (Pagination Control) is a flexible control to implement paging in an easy way.

## Code Download
The SPFx code developed for this article can be found [here](github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-pnp-pagination).

## References
- [PnP Pagination Control](https://pnp.github.io/sp-dev-fx-controls-react/controls/Pagination/)
- [Getting started with SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant?WT.mc_id=M365-MVP-5003693)
