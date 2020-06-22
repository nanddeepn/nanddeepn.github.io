---
title: "SharePoint Framework - CRUD operations using No Framework"
date: "2018-08-09"
---

## Overview

In the article Develop First Client Side Web Part, we developed basic SharePoint client web part which can run independently without any interaction with SharePoint.

In this article, we will explore to interact with the SharePoint list for CRUD (Create, Read, Update, and Delete) operations using No framework option. The CRUS operations will be performed using REST APIs.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-crud-no-framework

1. Navigate to above created directory.

cd spfx-crud-no-framework

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-84.png)

- **Solution Name:** Hit enter to have default name (spfx-crud-no-framework in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Same folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: N (install on each site explicitly)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: NoFrameworkCRUD
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: CRUD operations with no framework
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: No JavaScript Framework

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, in the command prompt type below command to open the solution in code editor of your choice.

code .

Configure Property for List Name

SPFx solution by default have description property created. Let us change the property to list name. We will use this property to configure the list name on which the CRUD operation is to perform.

1. Open mystrings.d.ts under \\src\\webparts\\noFrameworkCrud\\loc\\ folder.
2. Rename DescriptionFieldLabel to ListNameFieldLabel.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-85.png)

declare interface INoFrameworkCrudWebPartStrings {  
  PropertyPaneDescription: string;  
  BasicGroupName: string;  
  ListNameFieldLabel: string;  
}  
  
declare module 'NoFrameworkCrudWebPartStrings' {  
  const strings: INoFrameworkCrudWebPartStrings;  
  export = strings;  
}

1. In en-us.js file under \\src\\webparts\\noFrameworkCrud\\loc\\ folder set the display name for listName property.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-86.png)

define(\[\], function() {  
  return {  
    "PropertyPaneDescription": "Description",  
    "BasicGroupName": "Group Name",  
    "ListNameFieldLabel": "List Name"  
  }  
});

1. Open main webpart file (NoFrameworkCrudWebPart.ts) under \\src\\webparts\\noFrameworkCrud folder.
2. Rename description property pane field to listName.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-87.png)

import { Version } from '@microsoft/sp-core-library';  
import {  
  BaseClientSideWebPart,  
  IPropertyPaneConfiguration,  
  PropertyPaneTextField  
} from '@microsoft/sp-webpart-base';  
import { escape } from '@microsoft/sp-lodash-subset';  
  
import { SPHttpClient, SPHttpClientResponse } from '@microsoft/sp-http';  
import { IListItem } from './IListItem';  
  
import styles from './NoFrameworkCrudWebPart.module.scss';  
import \* as strings from 'NoFrameworkCrudWebPartStrings';  
  
export interface INoFrameworkCrudWebPartProps {  
  listName: string;  
}  
  
export default class NoFrameworkCrudWebPart extends BaseClientSideWebPart<INoFrameworkCrudWebPartProps> {  
  private listItemEntityTypeName: string = undefined;  
  
  public render(): void {  
    this.domElement.innerHTML = \`  
      <div class="${ styles.noFrameworkCrud }">  
        <div class="${ styles.container }">  
          <div class="${ styles.row }">  
            <div class="${ styles.column }">  
              <span class="${ styles.title }">Welcome to SharePoint!</span>  
              <p class="${ styles.subTitle }">Customize SharePoint experiences using Web Parts.</p>  
              <p class="${ styles.description }">${escape(this.properties.listName)}</p>  
          <a href="https://aka.ms/spfx" class="${ styles.button }">  
                <span class="${ styles.label }">Learn more</span>  
              </a>  
            </div>  
          </div>  
        </div>  
      </div>\`;  
  }  
  
  protected get dataVersion(): Version {  
    return Version.parse('1.0');  
  }  
  
  protected getPropertyPaneConfiguration(): IPropertyPaneConfiguration {  
    return {  
      pages: \[  
        {  
          header: {  
            description: strings.PropertyPaneDescription  
          },  
          groups: \[  
            {  
              groupName: strings.BasicGroupName,  
              groupFields: \[  
                PropertyPaneTextField('listName', {  
                  label: strings.ListNameFieldLabel  
                })  
              \]  
            }  
          \]  
        }  
      \]  
    };  
  }  
} 

1. On the command prompt, type “gulp serve”.
2. In the SharePoint local workbench page, add the web part.
3. Edit the web part to ensure the listName property pane field is getting reflected.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-88.png)

## Model for List Item

Let us add a class (IListItem.ts) representing the list item.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-89.png)

export interface IListItem {  
    Title?: string;  
    Id: number;  
}

## Add Controls to WebPart

1. Open main webpart file (NoFrameworkCrudWebPart.ts) under \\src\\webparts\\noFrameworkCrud folder.
2. Modify Render method to include buttons for CRUD operations and add event handlers to each of the button.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-90.png)

export default class NoFrameworkCrudWebPart extends BaseClientSideWebPart<INoFrameworkCrudWebPartProps> {  
  private listItemEntityTypeName: string = undefined;  
  
  public render(): void {  
    this.domElement.innerHTML = \`  
      <div class="${ styles.noFrameworkCrud }">  
        <div class="${ styles.container }">  
          <div class="${ styles.row }">  
            <div class="${ styles.column }">  
              <span class="${ styles.title }">CRUD operations</span>  
              <p class="${ styles.subTitle }">No Framework</p>  
              <p class="${ styles.description }">Name: ${escape(this.properties.listName)}</p>  
  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <button class="${styles.button} create-Button">  
                    <span class="${styles.label}">Create item</span>  
                  </button>  
                  <button class="${styles.button} read-Button">  
                    <span class="${styles.label}">Read item</span>  
                  </button>  
                </div>  
              </div>  
  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <button class="${styles.button} update-Button">  
                    <span class="${styles.label}">Update item</span>  
                  </button>  
                  <button class="${styles.button} delete-Button">  
                    <span class="${styles.label}">Delete item</span>  
                  </button>  
                </div>  
              </div>  
  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <div class="status"></div>  
                  <ul class="items"><ul>  
                </div>  
              </div>  
  
            </div>  
          </div>  
        </div>  
      </div>\`;  
  
      this.setButtonsEventHandlers();  
  }  
  
  private setButtonsEventHandlers(): void {  
    const webPart: NoFrameworkCrudWebPart = this;  
    this.domElement.querySelector('button.create-Button').addEventListener('click', () => { webPart.createItem(); });  
    this.domElement.querySelector('button.read-Button').addEventListener('click', () => { webPart.readItem(); });  
    this.domElement.querySelector('button.update-Button').addEventListener('click', () => { webPart.updateItem(); });  
    this.domElement.querySelector('button.delete-Button').addEventListener('click', () => { webPart.deleteItem(); });  
  }  
  
  private createItem(): void {  
  }  
  
  private readItem(): void {  
  }  
  
  private updateItem(): void {  
  }  
  
  private deleteItem(): void {  
  }  
}

1. In the command prompt type “gulp serve” to see the buttons on webpart.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-91.png)

1. We will perform read, update, and delete operations on latest item in SharePoint list. Let us implement generic method which will return the id of latest item from given list. We will use the REST API to query the list.

private getLatestItemId(): Promise<number> {  
  return new Promise<number>((resolve: (itemId: number) => void, reject: (error: any) => void): void => {  
    this.context.spHttpClient.get(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items?$orderby=Id desc&$top=1&$select=id\`,  
      SPHttpClient.configurations.v1,  
      {  
        headers: {  
          'Accept': 'application/json;odata=nometadata',  
          'odata-version': ''  
        }  
      })  
      .then((response: SPHttpClientResponse): Promise<{ value: { Id: number }\[\] }> => {  
        return response.json();  
      }, (error: any): void => {  
        reject(error);  
      })  
      .then((response: { value: { Id: number }\[\] }): void => {  
        if (response.value.length === 0) {  
          resolve(-1);  
        }  
        else {  
          resolve(response.value\[0\].Id);  
        }  
      });  
  });  
}

## Implement Create Operation

First start by implementing create method, which will add an item to SharePoint list.

private createItem(): void {  
  const body: string = JSON.stringify({  
    'Title': \`Item ${new Date()}\`  
  });  
  
  this.context.spHttpClient.post(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items\`,  
  SPHttpClient.configurations.v1,  
  {  
    headers: {  
      'Accept': 'application/json;odata=nometadata',  
      'Content-type': 'application/json;odata=nometadata',  
      'odata-version': ''  
    },  
    body: body  
  })  
  .then((response: SPHttpClientResponse): Promise<IListItem> => {  
    return response.json();  
  })  
  .then((item: IListItem): void => {  
    this.updateStatus(\`Item '${item.Title}' (ID: ${item.Id}) successfully created\`);  
  }, (error: any): void => {  
    this.updateStatus('Error while creating the item: ' + error);  
  });  
}  
  
private updateStatus(status: string, items: IListItem\[\] = \[\]): void {  
  this.domElement.querySelector('.status').innerHTML = status;  
  this.updateItemsHtml(items);  
}  
  
private updateItemsHtml(items: IListItem\[\]): void {  
  this.domElement.querySelector('.items').innerHTML = items.map(item => \`<li>${item.Title} (${item.Id})</li>\`).join("");  
}

## Implement Read Operation

We will use REST API to read the latest item

private readItem(): void {  
  this.getLatestItemId()  
    .then((itemId: number): Promise<SPHttpClientResponse> => {  
      if (itemId === -1) {  
        throw new Error('No items found in the list');  
      }  
  
      this.updateStatus(\`Loading information about item ID: ${itemId}...\`);  
        
      return this.context.spHttpClient.get(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items(${itemId})?$select=Title,Id\`,  
        SPHttpClient.configurations.v1,  
        {  
          headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'odata-version': ''  
          }  
        });  
    })  
    .then((response: SPHttpClientResponse): Promise<IListItem> => {  
      return response.json();  
    })  
    .then((item: IListItem): void => {  
      this.updateStatus(\`Item ID: ${item.Id}, Title: ${item.Title}\`);  
    }, (error: any): void => {  
      this.updateStatus('Loading latest item failed with error: ' + error);  
    });  
}  

## Implement Update Operation

Firstly we will get the latest item and update it.

private updateItem(): void {  
  let latestItemId: number = undefined;  
  this.updateStatus('Loading latest item...');  
  
  this.getLatestItemId()  
    .then((itemId: number): Promise<SPHttpClientResponse> => {  
      if (itemId === -1) {  
        throw new Error('No items found in the list');  
      }  
  
      latestItemId = itemId;  
      this.updateStatus(\`Loading information about item ID: ${itemId}...\`);  
        
      return this.context.spHttpClient.get(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items(${latestItemId})?$select=Title,Id\`,  
        SPHttpClient.configurations.v1,  
        {  
          headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'odata-version': ''  
          }  
        });  
    })  
    .then((response: SPHttpClientResponse): Promise<IListItem> => {  
      return response.json();  
    })  
    .then((item: IListItem): void => {  
      this.updateStatus(\`Item ID1: ${item.Id}, Title: ${item.Title}\`);  
  
      const body: string = JSON.stringify({  
        'Title': \`Updated Item ${new Date()}\`  
      });  
  
      this.context.spHttpClient.post(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items(${item.Id})\`,  
        SPHttpClient.configurations.v1,  
        {  
          headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'Content-type': 'application/json;odata=nometadata',  
            'odata-version': '',  
            'IF-MATCH': '\*',  
            'X-HTTP-Method': 'MERGE'  
          },  
          body: body  
        })  
        .then((response: SPHttpClientResponse): void => {  
          this.updateStatus(\`Item with ID: ${latestItemId} successfully updated\`);  
        }, (error: any): void => {  
          this.updateStatus(\`Error updating item: ${error}\`);  
        });  
    });  
}

## Implement Delete Operation

REST APIs are used to find and delete the latest item.

private deleteItem(): void {  
  if (!window.confirm('Are you sure you want to delete the latest item?')) {  
    return;  
  }  
  
  this.updateStatus('Loading latest items...');  
  let latestItemId: number = undefined;  
  let etag: string = undefined;  
  this.getLatestItemId()  
    .then((itemId: number): Promise<SPHttpClientResponse> => {  
      if (itemId === -1) {  
        throw new Error('No items found in the list');  
      }  
  
      latestItemId = itemId;  
      this.updateStatus(\`Loading information about item ID: ${latestItemId}...\`);  
      return this.context.spHttpClient.get(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items(${latestItemId})?$select=Id\`,  
        SPHttpClient.configurations.v1,  
        {  
          headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'odata-version': ''  
          }  
        });  
    })  
    .then((response: SPHttpClientResponse): Promise<IListItem> => {  
      etag = response.headers.get('ETag');  
      return response.json();  
    })  
    .then((item: IListItem): Promise<SPHttpClientResponse> => {  
      this.updateStatus(\`Deleting item with ID: ${latestItemId}...\`);  
      return this.context.spHttpClient.post(\`${this.context.pageContext.web.absoluteUrl}/\_api/web/lists/getbytitle('${this.properties.listName}')/items(${item.Id})\`,  
        SPHttpClient.configurations.v1,  
        {  
          headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'Content-type': 'application/json;odata=verbose',  
            'odata-version': '',  
            'IF-MATCH': etag,  
            'X-HTTP-Method': 'DELETE'  
          }  
        });  
    })  
    .then((response: SPHttpClientResponse): void => {  
      this.updateStatus(\`Item with ID: ${latestItemId} successfully deleted\`);  
    }, (error: any): void => {  
      this.updateStatus(\`Error deleting item: ${error}\`);  
    });  
}

# Test the WebPart

1. On the command prompt, type “gulp serve”.
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Edit webpart, in the properties pane type the list name.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-97.png)

1. Click the buttons (Create Item, Read Item, Update Item, and Delete Item) one by one to test the webpart.
2. Verify the operations are taking place in SharePoint list.

**Create Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-98.png)

**Read Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-99.png)

**Update Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-100.png)

**Delete Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-101.png)

# Troubleshooting

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-102.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

# Summary

SharePoint framework client web parts can be developed with No JavaScript options. REST APIs can be used to perform the CRUD operations on SharePoint list.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-crud-operations-using-no-framework/).
