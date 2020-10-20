---
title: "Read Large Lists in SharePoint Framework"
date: "2020-02-29"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/common/SPFx-1.jpg
  teaser: media/common/SPFx-1.jpg
tags:
  - "2020"
  - February 2020
last_modified_at: 2020-02-29T00:00:00-00:00
---

## Overview

Reading large lists always has been a challenge for SharePoint developers. This is one of those instances where the on-premises environment is appreciated by SharePoint developers, as SharePoint administrators can come to their rescue by adjusting the query threshold limit in Central Administration. The other commonly used approaches from the developer are to limit the items in view, index the columns, etc. This workaround reduces the pain to a certain extent but not fully. Unfortunately, we do not have much administration liberty in SharePoint Online to play around with threshold limits.

In this article, we will revise the threshold concept and various approaches to read the large lists programmatically in SharePoint Framework.


## Threshold

Let's quickly revise the term Threshold. To build information architecture for SharePoint, it is important to understand the SharePoint online limitations and boundaries. Which says, the list items limit is 30 Million. That's very very huge.

Every time you access a list or document library, a search query happens behind the scenes that return all the results. For example, if you have 50,000 items in a list, when you try to show them in a single view or programmatically retrieve those in a single query, you won't be able to get more than 5,000 items. We can say the list/library is facing the 5,000 item limit threshold. The threshold limit with SharePoint Online is still 5,000.

Do not confuse the threshold of 5,000 items with the list capacity which is 30 Million.


## Get around with Threshold

PnP JS is the popular library amongst SPFx developers for SharePoint operations. It provides some cool methods to get around the threshold issues.

**Example# 1: Basic usage**

```typescript
pnp.sp.web.lists.getByTitle("BigList").items.getAll().then((allItems: any[]) => {
    // how many did we get
    console.log(allItems.length);
});
```


**Example# 2: Set page size**

```typescript
pnp.sp.web.lists.getByTitle("BigList").items.getAll(4000).then((allItems: any[]) => {
    // how many did we get
    console.log(allItems.length);
});
```


**Example# 3:** Use select and top. top will set page size and override the any value passed to getAll

```typescript
pnp.sp.web.lists.getByTitle("BigList").items.select("Title").top(4000).getAll().then((allItems: any[]) => {
    // how many did we get
    console.log(allItems.length);
});
```


**Example# 4: U**se filter as a supported odata operation, but this will likely fail on large lists.

```typescript
pnp.sp.web.lists.getByTitle("BigList").items.select("Title").filter("Title eq 'Test'").getAll().then((allItems: any[]) => {
    // how many did we get
    console.log(allItems.length);
});
```


## Custom Implementation

Here is another approach to get read the large list with paging implementation.

Consider we have a SharePoint list named "LargeList" with below schema:

**Field Name**|**Type**
Title|Single line of text
Description|Multiple lines of text
Category|Single line of text
Quantity|Number


The model to represent above list will be as below:

```typescript
export interface ILargeListItem {  
     Title: string;  
     Description: string;  
     Category: string;  
     Quantity: number;  
}
```

Let us define our approach to handle large list:

- We will define the **page size** as 5000 for example (maximum threshold limit). Please note, SharePoint internally fetches the items in the batch of 100 items.
- The number of requests we will have to read entire large list will be: Number of list items / page size.
- We will make asynchronous requests to read the list items in batch.
- Wait for all asynchronous requests to finish

Implement a genetic method getPageListItems which returns items with paging, starting with index passed as an argument.

```
$skiptoken=Paged=TRUE%26p_ID=` + (index * Constants.Page_Size + 1)
```

The getPageListItems method implementation will look as follows:

```typescript
private getPageListItems(listTitle: string, index: number): Promise<ILargeListItem[]> {  
    return new Promise<ILargeListItem[]>((resolve, reject): void => {  
    let requestUrl = this.context.pageContext.web.absoluteUrl  
        + `/_api/web/Lists/GetByTitle('` + listTitle + `')/items`  
        + `?$skiptoken=Paged=TRUE%26p_ID=` + (index * Constants.Page_Size + 1)  
        + `&$top=` + Constants.Page_Size  
        + `&$select=ID,Title,Description,Category,Quantity`;  
  
    this.context.spHttpClient.get(requestUrl, SPHttpClient.configurations.v1)  
    .then((response: SPHttpClientResponse) => {  
        response.json().then((responseJSON: any) => {    
            resolve(responseJSON.value);    
          });    
        });  
    });   
}
```


Implement a method to get the latest item id. This will be our maximum limit to read the items.

```typescript
public getLatestItemId(listTitle: string): Promise<number> {  
    return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
        sp.web.lists.getByTitle(listTitle)  
            .items.orderBy('Id', false).top(1).select('Id').get()  
            .then((items: { Id: number }[]): void => {  
                if (items.length === 0) {  
                    resolve(-1);  
                }  
                else {  
                    resolve(items[0].Id);  
                }  
            }, (error: any): void => {  
                reject(error);  
            });  
    });  
}
```


The next step is to implement method the main method to make asynchronous requests to read the list items in batch.

```typescript
public async getLargeListItems(listTitle: string): Promise<ILargeListItem[]> {  
    var largeListItems: ILargeListItem[] = [];  
  
    return new Promise<ILargeListItem[]>(async (resolve, reject) => {  
        // Array to hold async calls  
        const asyncFunctions = [];  
  
        this.getLatestItemId(listTitle).then(async (itemCount: number) => {  
            for (let i = 0; i < Math.ceil(itemCount / Constants.Page_Size); i++) {  
                // Make multiple async calls  
                let resolvePagedListItems = () => {  
                    return new Promise(async (resolve) => {  
                        let pagedItems:ILargeListItem[] = await this.getPageListItems(listTitle, i);  
                        resolve(pagedItems);  
                    })  
                };  
                asyncFunctions.push(resolvePagedListItems());  
            }  
  
            // Wait for all async calls to finish  
            const results: any = await Promise.all(asyncFunctions);  
            for (let i = 0; i < results.length; i++) {  
                largeListItems = largeListItems.concat(results[i]);  
            }  
  
            resolve(largeListItems);  
        });  
    });  
}
```


## Summary

Reading large lists always has been a challenge for SharePoint developers. There are various approaches to read the large lists programmatically in SharePoint Framework. We explored the options to read large lists with PnP JS as well a custom implementation with paging approach.

This content was originally posted [here](https://www.c-sharpcorner.com/article/read-large-lists-in-sharepoint-framework/).
