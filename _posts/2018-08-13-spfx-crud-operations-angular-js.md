---
title: "SharePoint Framework - CRUD operations using Angular JS"
date: "2018-08-13"
---

## Overview

In the article Develop First Client Side Web Part, we developed basic SharePoint client web part which can run independently without any interaction with SharePoint.

In this article, we will explore to interact with the SharePoint list for CRUD (Create, Read, Update, and Delete) operations using Angular JS. React JS is not natively supported by SharePoint Framework.

## Brief about Angular JS

AngularJS is a JavaScript framework which extends HTML with new attributes. It is popular for creating SPA (Single Page Application). Read more about Angular JS at [https://angularjs.org/](https://angularjs.org/)

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-crud-angularjs

1. Navigate to above created directory.

cd spfx-crud-angularjs

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-145.png)

- **Solution Name:** Hit enter to have default name (spfx-crud-angularjs in this case) or type in any other name for your solution.
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
    - Selected choice: AngularCRUD
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: CRUD operations with Angular JS
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

Configure Angular JS

As Angular JS is not natively supported in SharePoint framework, we have to add a reference of it to our solution using NPM package. Type in the below commands on the command prompt.

npm i angular ng-office-ui-fabric --save

The --save option enables NPM to include the packages to dependencies section of the package.json file.

Install Angular typings for TypeScript. Typings will help for auto complete while writing the code in the code editor.

tsd install @types/angular --save

Expand node\_module folder to see the npm packages being added for Angular.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-146.png)

Open config.json under config folder. Under externals node, add Angular reference.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-147.png)

You may also add path from external CDN.

Open package.json and verify Angular dependency is listed.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-148.png)

## Configure Property for List Name

SPFx solution by default have description property created. Let us change the property to list name. We will use this property to configure the list name on which the CRUD operation is to perform.

1. Open mystrings.d.ts under \\src\\webparts\\angularCrud\\loc\\ folder.
2. Rename DescriptionFieldLabel to ListNameFieldLabel.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-149.png)

declare interface IAngularCrudWebPartStrings {  
  PropertyPaneDescription: string;  
  BasicGroupName: string;  
  ListNameFieldLabel: string;  
}  
  
declare module 'AngularCrudWebPartStrings' {  
  const strings: IAngularCrudWebPartStrings;  
  export = strings;  
}

1. In en-us.js file under \\src\\webparts\\angularCrud\\loc\\ folder set the display name for listName property.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-150.png)

define(\[\], function() {  
  return {  
    "PropertyPaneDescription": "Description",  
    "BasicGroupName": "Group Name",  
    "ListNameFieldLabel": "List Name"  
  }  
});

1. Open main webpart file (AngularCrudWebPart.ts) under \\src\\webparts\\angularCrud folder.
2. Rename description property pane field to listName.

export interface IAngularCrudWebPartProps {  
  listName: string;  
}  
  
export default class AngularCrudWebPart extends BaseClientSideWebPart<IAngularCrudWebPartProps> {  
  
  public render(): void {  
    this.domElement.innerHTML = \`  
      <div class="${ styles.angularCrud }">  
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

1. On the command prompt, type “gulp serve”
2. In the SharePoint local workbench page, add the web part.
3. Edit the web part to ensure the listName property pane field is getting reflected.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-152.png)

## Build an Angular application

Add folder named “app” as starting point for creating Angular app.

**Configure Data Service**

Add a file DataService.ts under app folder and define the data services we are going to perform.

export interface IListItem {  
    Id: number;  
    Title?: string;  
     ETag?: string;  
  }  
    
  export interface IDataService {  
    createItem(title: string, webUrl: string, listName: string): angular.IPromise<IListItem>;  
    readItem(itemId: number, webUrl: string, listName: string): angular.IPromise<IListItem>;  
    getLatestItemId(webUrl: string, listName: string): angular.IPromise<number>;  
    updateItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}>;  
    deleteItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}>;  
  }  
    
  export default class DataService implements IDataService {  
    public static $inject: string\[\] = \['$q', '$http'\];  
  
    constructor(private $q: angular.IQService, private $http: angular.IHttpService) {  
    }  
    
    public createItem(title: string, webUrl: string, listName: string): angular.IPromise<IListItem> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
        return deferred.promise;  
    }  
  
    public readItem(itemId: number, webUrl: string, listName: string): angular.IPromise<IListItem> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
        return deferred.promise;  
    }  
  
    public getLatestItemId(webUrl: string, listName: string): angular.IPromise<number> {  
        const deferred: angular.IDeferred<number> = this.$q.defer();  
        return deferred.promise;  
    }  
  
    public updateItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
        return deferred.promise;  
    }  
  
    public deleteItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
        return deferred.promise;  
    }  
  }

**Implement generic methods**

getLatestItemId - to get the id of the latest item.

public getLatestItemId(webUrl: string, listName: string): angular.IPromise<number> {  
        const deferred: angular.IDeferred<number> = this.$q.defer();  
  
        this.$http({  
            url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')/items?$orderby=Id desc&$top=1&$select=Id\`,  
            method: 'GET',  
            headers: {  
              'Accept': 'application/json;odata=nometadata'  
            }  
        }).then((result: angular.IHttpPromiseCallbackArg<{ value: { Id: number }\[\] }>): void => {  
            if (result.data.value.length === 0) {  
              deferred.resolve(-1);  
            }  
            else {  
              deferred.resolve(result.data.value\[0\].Id);  
            }  
          }, (error: any): void => {  
            deferred.reject(error);  
        });  
  
        return deferred.promise;  
    }

getRequestDigest - to get the request digest to send along with create, update, delete operations.

private getRequestDigest(webUrl: string): angular.IPromise<string> {  
        const deferred: angular.IDeferred<string> = this.$q.defer();  
      
        this.$http({  
          url: webUrl + '/\_api/contextinfo',  
          method: 'POST',  
          headers: {  
            'Accept': 'application/json;odata=nometadata'  
          }  
        })  
          .then((digestResult: angular.IHttpPromiseCallbackArg<{ FormDigestValue: string }>): void => {  
            deferred.resolve(digestResult.data.FormDigestValue);  
          }, (error: any): void => {  
            deferred.reject(error);  
          });  
      
        return deferred.promise;  
    }

getListItemEntityTypeName - to get the entity type name of list item to send along with create, update, delete operations.

private getListItemEntityTypeName(webUrl: string, listName: string): angular.IPromise<string> {  
      const deferred: angular.IDeferred<string> = this.$q.defer();  
    
      this.$http({  
        url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')?$select=ListItemEntityTypeFullName\`,  
        method: 'GET',  
        headers: {  
          'Accept': 'application/json;odata=nometadata'  
        }  
      })  
        .then((result: angular.IHttpPromiseCallbackArg<{ ListItemEntityTypeFullName: string }>): void => {  
          deferred.resolve(result.data.ListItemEntityTypeFullName);  
        }, (error: any): void => {  
          deferred.reject(error);  
        });  
    
      return deferred.promise;  
    }

**Implement Create Operation**

We will use the REST API to add the item to list.

public createItem(title: string, webUrl: string, listName: string): angular.IPromise<IListItem> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
  
        let listItemEntityTypeName: string = undefined;  
        this.getListItemEntityTypeName(webUrl, listName)  
          .then((typeName: string): angular.IPromise<string> => {  
            listItemEntityTypeName = typeName;  
            return this.getRequestDigest(webUrl);  
          })  
          .then((requestDigest: string): angular.IPromise<angular.IHttpPromiseCallbackArg<IListItem>> => {  
            const body: string = JSON.stringify({  
              '\_\_metadata': {  
                'type': listItemEntityTypeName  
              },  
              'Title': title  
            });  
            return this.$http({  
              url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')/items\`,  
              method: 'POST',  
              headers: {  
                'Accept': 'application/json;odata=nometadata',  
                'Content-type': 'application/json;odata=verbose',  
                'X-RequestDigest': requestDigest  
              },  
              data: body  
            });  
          })  
          .then((response: angular.IHttpPromiseCallbackArg<IListItem>): void => {  
            deferred.resolve(response.data);  
          }, (error: any): void => {  
            deferred.reject(error);  
          });  
  
        return deferred.promise;      
    }

**Implement Read Operation**

We will use REST API to read the latest item.

public readItem(itemId: number, webUrl: string, listName: string): angular.IPromise<IListItem> {  
        const deferred: angular.IDeferred<IListItem> = this.$q.defer();  
  
        this.$http({  
            url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')/items(${itemId})\`,  
            method: 'GET',  
            headers: {  
              'Accept': 'application/json;odata=nometadata'  
            }  
        })  
        .then((response: angular.IHttpPromiseCallbackArg<IListItem>): void => {  
                const item: IListItem = response.data;  
                item.ETag = response.headers('ETag');  
                deferred.resolve(item);  
            }, (error: any): void => {  
                deferred.reject(error);  
        });  
  
        return deferred.promise;  
    }

**Implement Update Operation**

We will use REST API to update the latest item.

public updateItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}> {  
      const deferred: angular.IDeferred<{}> = this.$q.defer();  
  
      let listItemEntityTypeName: string = undefined;  
      this.getListItemEntityTypeName(webUrl, listName)  
        .then((typeName: string): angular.IPromise<string> => {  
          listItemEntityTypeName = typeName;  
          return this.getRequestDigest(webUrl);  
        })  
        .then((requestDigest: string): angular.IPromise<angular.IHttpPromiseCallbackArg<{}>> => {  
          const body: string = JSON.stringify({  
            '\_\_metadata': {  
              'type': listItemEntityTypeName  
            },  
            'Title': item.Title  
          });  
          return this.$http({  
            url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')/items(${item.Id})\`,  
            method: 'POST',  
            headers: {  
              'Accept': 'application/json;odata=nometadata',  
              'Content-type': 'application/json;odata=verbose',  
              'X-RequestDigest': requestDigest,  
              'IF-MATCH': item.ETag,  
              'X-HTTP-Method': 'MERGE'  
            },  
            data: body  
          });  
        })  
        .then((result: {}): void => {  
          deferred.resolve();  
        }, (error: any): void => {  
          deferred.reject(error);  
        });  
    
      return deferred.promise;  
    }

**Implement Delete Operation**

We will use REST API to delete the latest item.

public deleteItem(item: IListItem, webUrl: string, listName: string): angular.IPromise<{}> {  
      const deferred: angular.IDeferred<{}> = this.$q.defer();  
  
      this.getRequestDigest(webUrl)  
        .then((requestDigest: string): angular.IPromise<angular.IHttpPromiseCallbackArg<{}>> => {  
          return this.$http({  
            url: \`${webUrl}/\_api/web/lists/getbytitle('${listName}')/items(${item.Id})\`,  
            method: 'POST',  
            headers: {  
              'Accept': 'application/json;odata=nometadata',  
              'X-RequestDigest': requestDigest,  
              'IF-MATCH': item.ETag,  
              'X-HTTP-Method': 'DELETE'  
            }  
          });  
        })  
        .then((result: {}): void => {  
          deferred.resolve();  
        }, (error: any): void => {  
          deferred.reject(error);  
        });  
    
      return deferred.promise;  
    }

**Configure Controller**

Add a file HomeController.ts under app folder.

import { IDataService, IListItem } from './DataService';  
  
interface IConfigurationChangedArgs {  
  webUrl: string;  
  listName: string;  
}  
  
export default class HomeController {  
  public static $inject: string\[\] = \['DataService', '$window', '$rootScope', '$scope'\];  
  
  public status: string = undefined;  
  public items: IListItem\[\] = \[\];  
  
  private webUrl: string = undefined;  
  private listName: string = undefined;  
  
  constructor(private dataService: IDataService, private $window: angular.IWindowService, private $rootScope: angular.IRootScopeService, private $scope: angular.IScope) {  
    const vm: HomeController = this;  
    this.init(undefined, undefined, undefined);  
  
    $rootScope.$on('configurationChanged',  
      (event: angular.IAngularEvent, args: IConfigurationChangedArgs): void => {  
        vm.init(args.webUrl, args.listName, vm.$scope);  
      });  
  }  
  
  private init(webUrl: string, listName: string, $scope: angular.IScope): void {  
    if (webUrl !== undefined && webUrl.length > 0 &&  
      listName !== undefined && listName.length > 0) {  
      this.webUrl = webUrl;  
      this.listName = listName;  
    }  
      
    if ($scope) {  
      $scope.$digest();  
    }  
  }  
  
  public createItem(): void {  
    const itemTitle: string = \`Item ${new Date()}\`;  
    this.status = 'Creating item...';  
    this.items.length = 0;  
    this.dataService.createItem(itemTitle, this.webUrl, this.listName)  
      .then((item: IListItem): void => {  
        this.status = \`Item '${item.Title}' (ID: ${item.Id}) successfully created\`;  
      }, (error: any): void => {  
        this.status = 'Error while creating the item: ' + error;  
      });  
  }  
  
  public readItem(): void {  
    this.status = 'Loading latest items...';  
    this.items.length = 0;  
    this.dataService.getLatestItemId(this.webUrl, this.listName)  
      .then((itemId: number): angular.IPromise<IListItem> => {  
        if (itemId === -1) {  
          throw new Error('No items found in the list');  
        }  
  
        this.status = \`Loading information about item ID: ${itemId}...\`;  
        return this.dataService.readItem(itemId, this.webUrl, this.listName);  
      })  
      .then((item: IListItem): void => {  
        this.status = \`Item ID: ${item.Id}, Title: ${item.Title}\`;  
      }, (error: any): void => {  
        this.status = 'Loading latest item failed with error: ' + error;  
      });  
  }  
  
  public updateItem(): void {  
    this.status = 'Loading latest items...';  
    this.items.length = 0;  
    let latestItemId: number = undefined;  
    this.dataService.getLatestItemId(this.webUrl, this.listName)  
      .then((itemId: number): angular.IPromise<IListItem> => {  
        if (itemId === -1) {  
          throw new Error('No items found in the list');  
        }  
  
        latestItemId = itemId;  
        this.status = \`Loading information about item ID: ${latestItemId}...\`;  
  
        return this.dataService.readItem(latestItemId, this.webUrl, this.listName);  
      })  
      .then((latestItem: IListItem): angular.IPromise<{}> => {  
        this.status = \`Updating item with ID: ${latestItemId}...\`;  
        latestItem.Title = \`Item ${new Date()}\`;  
        return this.dataService.updateItem(latestItem, this.webUrl, this.listName);  
      })  
      .then((result: {}): void => {  
        this.status = \`Item with ID: ${latestItemId} successfully updated\`;  
      }, (error: any): void => {  
        this.status = \`Error updating item: ${error}\`;  
      });  
  }  
  
  public deleteItem(): void {  
    if (!this.$window.confirm('Are you sure you want to delete this todo item?')) {  
      return;  
    }  
  
    this.status = 'Loading latest items...';  
    this.items.length = 0;  
    let latestItemId: number = undefined;  
    this.dataService.getLatestItemId(this.webUrl, this.listName)  
      .then((itemId: number): angular.IPromise<IListItem> => {  
        if (itemId === -1) {  
          throw new Error('No items found in the list');  
        }  
  
        latestItemId = itemId;  
        this.status = \`Loading information about item ID: ${latestItemId}...\`;  
  
        return this.dataService.readItem(latestItemId, this.webUrl, this.listName);  
      })  
      .then((latestItem: IListItem): angular.IPromise<{}> => {  
        this.status = \`Deleting item with ID: ${latestItemId}...\`;  
        return this.dataService.deleteItem(latestItem, this.webUrl, this.listName);  
      })  
      .then((result: {}): void => {  
        this.status = \`Item with ID: ${latestItemId} successfully deleted\`;  
      }, (error: any): void => {  
        this.status = \`Error deleting item: ${error}\`;  
      });  
  }  
}

**Configure Module**

Add a file app-module.ts under app folder.

import \* as angular from 'angular';  
import HomeController from './HomeController';  
import DataService from './DataService';  
  
import 'ng-office-ui-fabric';  
  
const crudapp: ng.IModule = angular.module('crudapp', \[  
  'officeuifabric.core',  
  'officeuifabric.components'  
\]);  
  
crudapp  
  .controller('HomeController', HomeController)  
  .service('DataService', DataService);

## Add Controls to WebPart

1. Open AngularCrudWebPart.ts under “\\src\\webparts\\angularCrud\\” folder.
2. Import Angular module.

import \* as angular from 'angular';  
import './app/app-module';

1. Modify Render method to include buttons for CRUD operations and add event handlers to each of the button.

export default class AngularCrudWebPart extends BaseClientSideWebPart<IAngularCrudWebPartProps> {  
  private $injector: angular.auto.IInjectorService;  
  
  public render(): void {  
    if (this.renderedOnce === false) {  
      this.domElement.innerHTML = \`  
        <div class="${styles.angularCrud}" data-ng-controller="HomeController as vm">  
          <div class="${styles.container}">  
            <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
              <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <span class="ms-font-xl ms-fontColor-white">  
                    Sample SharePoint CRUD operations in Angular  
                  </span>  
                </div>  
              </div>  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <uif-button ng-click="vm.createItem()" ng-disabled="vm.listNotConfigured">Create item</uif-button>  
                  <uif-button ng-click="vm.readItem()" ng-disabled="vm.listNotConfigured">Read item</uif-button>  
                </div>  
              </div>  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <uif-button ng-click="vm.updateItem()" ng-disabled="vm.listNotConfigured">Update item</uif-button>  
                  <uif-button ng-click="vm.deleteItem()" ng-disabled="vm.listNotConfigured">Delete item</uif-button>  
                </div>  
              </div>  
              <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">  
                <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">  
                  <div>{{vm.status}}</div>  
                  <ul>  
                    <li ng-repeat="item in vm.items">{{item.Title}} ({{item.Id}})</li>  
                  <ul>  
                </div>  
              </div>  
          </div>  
        </div>\`;  
  
      this.$injector = angular.bootstrap(this.domElement, \['crudapp'\]);  
    }  
  
    this.$injector.get('$rootScope').$broadcast('configurationChanged', {  
      webUrl: this.context.pageContext.web.absoluteUrl,  
      listName: this.properties.listName  
    });  
  }  
}

Test the WebPart

1. On the command prompt, type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Edit webpart, in the properties pane type the list name
6. Click the buttons (Create Item, Read Item, Update Item, and Delete Item) one by one to test the webpart
7. Verify the operations are taking place in SharePoint list.

**Create Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-153.png)

**Read Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-154.png)

**Update Operation**

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-155.png)

**Delete Operation**

## ![](https://nanddeepnachanblogs.com/wp-content/uploads/2018/08/angular-crud-delete.png)

## Troubleshooting

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-156.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

## Summary

Angular JS is not natively supported by the SharePoint framework. However, it can be used with SharePoint Framework. Angular is more famous for creating SPAs (Single Page Applications).

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-crud-operations-using-angular-js/).
