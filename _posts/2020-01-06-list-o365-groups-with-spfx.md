---
title: "List out Office 365 Groups with SPFx"
date: "2020-01-06"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - MS Teams
header:
  image: media/2020-01-06-list-o365-groups-with-spfx/04.png
  teaser: media/2020-01-06-list-o365-groups-with-spfx/04.png
tags:
  - "2020"
  - January 2020
last_modified_at: 2020-01-06T00:00:00-00:00
---

## Overview

MS Graph APIs offer a wide range of capabilities. It can be effectively used to list Office 365 groups from SharePoint Framework solutions. An SPFx web part can be a useful asset to list all Office 365 groups, instead of navigating to Office 365 admin center.

During this article, we will develop a practical scenario to list all Office 365 groups using MS Graph.


## Develop SPFx Web Part

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-list-o365-groups
    ```

2. Navigate to the above created directory.

    ```
    cd spfx-list-o365-groups
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2020-01-06-list-o365-groups-with-spfx/01.png)

5. Yeoman generator will perform the scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
6. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```


## NPM Packages Dependency

**Microsoft Graph TypeScript types**

The typings will help in providing IntelliSense while writing the code.

On the command prompt, run below command to include the npm package.

```
npm install @microsoft/microsoft-graph-types --save-dev
```


## Set Permission Scopes

To consume MS Graph or any third-party REST APIs, the permissions need to be explicitly set in the solution manifest.

Open "config\package-solution.json" and add below permission scope.

```
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",  
  "solution": {  
    "name": "spfx-list-o365-groups-client-side-solution",  
    "id": "5d134841-70c0-47a5-a838-217b6fd79c55",  
    "version": "1.0.0.0",  
    "includeClientSideAssets": true,  
    "isDomainIsolated": false,  
    "webApiPermissionRequests": [  
      {  
        "resource": "Microsoft Graph",  
        "scope": "Group.Read.All"  
      },  
      {  
        "resource": "Microsoft Graph",  
        "scope": "Group.ReadWrite.All"  
      }  
    ]  
  },  
  "paths": {  
    "zippedPackage": "solution/spfx-list-o365-groups.sppkg"  
  }  
}
```


## Define State

Define a state to represent O365 group creation information.

Add a file IListO365GroupsState.ts file under folder "src\webparts\listO365Groups\components\IListO365GroupsState.ts".

```typescript
export interface IGroup {  
    id: string;  
    displayName: string;  
    description?: string;  
    visibility?: string;  
}  
  
export interface IGroupCollection {  
    value: IGroup[];  
}  
  
export interface IListO365GroupsState {  
    filterText?: string;  
    groups?: IGroup[];  
}
```


## Implement O365 Groups Service

Implement a service to perform operations against O365 groups.

1. Under src, create a folder named "services".
2. Add a file O365GroupService.ts file under it.

    ```typescript
    import { MSGraphClient } from "@microsoft/sp-http";  
    import { WebPartContext } from "@microsoft/sp-webpart-base";  
    import { IGroup, IGroupCollection } from "../models";  
      
    export class O365GroupService {  
      public context: WebPartContext;  
      
      public setup(context: WebPartContext): void {  
        this.context = context;  
      }  
      
      .  
      .  
      .  
      
    const GroupService = new O365GroupService();  
    export default GroupService;
    ```

3. Implement a method to get all O365 groups.

    ```typescript
    public getGroups(): Promise<IGroup[]> {  
        return new Promise<IGroup[]>((resolve, reject) => {  
          try {  
            // Prepare the output array  
            var o365groups: Array<IGroup> = new Array<IGroup>();  
      
            this.context.msGraphClientFactory  
              .getClient()  
              .then((client: MSGraphClient) => {  
                client  
                  .api("/groups?$filter=groupTypes/any(c:c eq 'Unified')")  
                  .get((error: any, groups: IGroupCollection, rawResponse: any) => {  
                    // Map the response to the output array  
                    groups.value.map((item: any) => {  
                      o365groups.push({  
                        id: item.id,  
                        displayName: item.displayName,  
                        description: item.description,  
                        visibility: item.visibility  
                      });  
                    });  
      
                    resolve(o365groups);  
                  });  
              });  
          } catch (error) {  
            console.error(error);  
          }  
        });  
      }  
    }
    ```

4. Implement a method to get the O365 group link.

    ```typescript
    public getGroupLink(groups: IGroup): Promise<any> {  
        return new Promise<any>((resolve, reject) => {  
          try {  
            this.context.msGraphClientFactory  
              .getClient()  
              .then((client: MSGraphClient) => {  
                client  
                  .api(`/groups/${groups.id}/sites/root/weburl`)  
                  .get((error: any, group: any, rawResponse: any) => {  
                    resolve(group);  
                  });  
              });  
          } catch (error) {  
            console.error(error);  
          }  
        });  
      }
    ```

5. Implement a method to get the O365 group thumbnail.

    ```typescript
    public getGroupThumbnail(groups: IGroup): Promise<any> {  
        return new Promise<any>((resolve, reject) => {  
          try {  
            this.context.msGraphClientFactory  
              .getClient()  
              .then((client: MSGraphClient) => {  
                client  
                  .api(`/groups/${groups.id}/photos/48x48/$value`)  
                  .responseType('blob')  
                  .get((error: any, group: any, rawResponse: any) => {  
                    resolve(window.URL.createObjectURL(group));  
                  });  
              });  
          } catch (error) {  
            console.error(error);  
          }  
        });  
      }
    ```

6. Under the services folder, add a file index.ts.

```
export \* from './O365GroupService';
```


## Consume service from the web part

1. Open web part file at "src\webparts\listO365Groups\ListO365GroupsWebPart.ts"
2. Import the service.

    ```typescript
    import O365GroupService from './services/O365GroupService';
    ```

3. Implement onInit() method to set up the service.

    ```typescript
    protected onInit(): Promise<void> {  
      return super.onInit().then(() => {  
        O365GroupService.setup(this.context);  
      });  
    }
    ```


## Implement the ListO365Groups React Component

1. Open the React component at src\webparts\listO365Groups\components\ListO365Groups.tsx
2. Add below imports.

    ```typescript
    import { FocusZone, FocusZoneDirection } from 'office-ui-fabric-react/lib/FocusZone';  
    import { TextField } from 'office-ui-fabric-react/lib/TextField';  
    import { Image, ImageFit } from 'office-ui-fabric-react/lib/Image';  
    import { Icon } from 'office-ui-fabric-react/lib/Icon';  
    import { List } from 'office-ui-fabric-react/lib/List';  
    import { ITheme, mergeStyleSets, getTheme, getFocusStyle } from 'office-ui-fabric-react/lib/Styling';  
    import { IGroup, IGroupCollection } from "../models/IGroup";  
    import O365GroupService from '../services/O365GroupService';
    ```

3. Use the React state.

    ```typescript
    import { IListO365GroupsState } from './IListO365GroupsState';  
      
    export default class ListO365Groups extends React.Component<IListO365GroupsProps, IListO365GroupsState> {  
      constructor(props: IListO365GroupsProps) {  
        super(props);  
      
        this.state = {  
          groups: []  
        };  
      }  
    .  
    .  
    .  
    }
    ```

4. As we are using Office 365 UI Fabric List, let us initialize the display of it.

    ```typescript
    interface IGroupListClassObject {  
      itemCell: string;  
      itemImage: string;  
      itemContent: string;  
      itemName: string;  
      itemIndex: string;  
      chevron: string;  
    }  
      
    const theme: ITheme = getTheme();  
    const { palette, semanticColors, fonts } = theme;  
      
    const classNames: IGroupListClassObject = mergeStyleSets({  
      itemCell: [  
        getFocusStyle(theme, { inset: -1 }),  
        {  
          minHeight: 54,  
          padding: 10,  
          boxSizing: 'border-box',  
          borderBottom: `1px solid ${semanticColors.bodyDivider}`,  
          display: 'flex',  
          selectors: {  
            '&:hover': { background: palette.neutralLight }  
          }  
        }  
      ],  
      itemImage: {  
        flexShrink: 0  
      },  
      itemContent: {  
        marginLeft: 10,  
        overflow: 'hidden',  
        flexGrow: 1  
      },  
      itemName: [  
        fonts.xLarge,  
        {  
          whiteSpace: 'nowrap',  
          overflow: 'hidden',  
          textOverflow: 'ellipsis'  
        }  
      ],  
      itemIndex: {  
        fontSize: fonts.small.fontSize,  
        color: palette.neutralTertiary,  
        marginBottom: 10  
      },  
      chevron: {  
        alignSelf: 'center',  
        marginLeft: 10,  
        color: palette.neutralTertiary,  
        fontSize: fonts.large.fontSize,  
        flexShrink: 0  
      }  
    });
    ```

5. Implement render method to add the UI controls.

    ```typescript
    public render(): React.ReactElement<IListO365GroupsProps> {  
      const { groups = [] } = this.state;  
      const resultCountText = groups.length === this._originalItems.length ? '' : ` (${groups.length} of ${this._originalItems.length} shown)`;  
      
      return (  
        <FocusZone direction={FocusZoneDirection.vertical}>  
          <TextField label={'Filter by name' + resultCountText} onChange={this._onFilterChanged} />  
          <List items={groups} onRenderCell={this._onRenderCell} />  
        </FocusZone>  
      );  
    }
    ```

6. Implement the event handlers for UI controls.

    ```typescript
    private _onFilterChanged = (_: any, text: string): void => {  
        this.setState({  
          filterText: text,  
          groups: text ? this._originalItems.filter(item => item.displayName.toLowerCase().indexOf(text.toLowerCase()) >= 0) : this._originalItems  
        });  
      
        this._getGroupLinks(this.state.groups);  
      }  
      
      private _onRenderCell(group: IGroup, index: number | undefined): JSX.Element {  
        return (  
          <div className={classNames.itemCell} data-is-focusable={true}>  
            <Image className={classNames.itemImage} src={group.thumbnail} width={50} height={50} imageFit={ImageFit.cover} />  
            <div className={classNames.itemContent}>  
              <div className={classNames.itemName}>  
                <a href={group.url} target="_blank">{group.displayName}</a>  
              </div>  
              <div className={classNames.itemIndex}>{group.visibility}</div>  
              <div>{group.description}</div>  
            </div>  
          </div>  
        );  
      }
    ```

7. Implement the helper methods.

```typescript
public _getGroups = (): void => {  
  O365GroupService.getGroups().then(groups => {  
    console.log(groups);  
    this._originalItems = groups;  
  
    this.setState({  
      groups: groups  
    });  
  
    this._getGroupLinks(groups);  
  });  
}  
  
public _getGroupLinks = (groups: any): void => {  
  groups.map(groupItem => (  
    O365GroupService.getGroupLink(groupItem).then(groupUrl => {  
      if (groupUrl !== null) {  
        this.setState(prevState => ({  
          groups: prevState.groups.map(group => group.id === groupItem.id ? { ...group, url: groupUrl.value } : group)  
        }));  
      }  
    })  
  ));  
  
  this._getGroupThumbnails(groups);  
}  
  
public _getGroupThumbnails = (groups: any): void => {  
  groups.map(groupItem => (  
    O365GroupService.getGroupThumbnail(groupItem).then(grouptb => {  
      console.log(grouptb);  
      this.setState(prevState => ({  
        groups: prevState.groups.map(group => group.id === groupItem.id ? { ...group, thumbnail: grouptb } : group)  
      }));  
    })  
  ));  
}  
```


## Deploy the SPFx Package to SharePoint App Catalog

Follow the below steps to deploy the SPFx package (.sppkg) to the SharePoint app catalog.

**Build minified assets**

On the command prompt, type the below command.

```
gulp bundle --ship
```


**Prepare the package**

On the command prompt, type the below command.

```
gulp package-solution --ship
```

The .sppkg package will be available inside the "sharepoint\solution" folder.

**Upload package to the app catalog**

1. Open the SharePoint app catalog site.
2. Upload the package to the app catalog.

    ![](/media/2020-01-06-list-o365-groups-with-spfx/02.png)

3. Click **Deploy**.


## API Management

After deploying the web part, follow the below steps to approve API requests.

1. Open SharePoint Admin Center ([https://[tenant]-admin.sharepoint.com](https://[tenant]-admin.sharepoint.com)).
2. From left navigation, click "API Management".
3. Approve the pending requests.

![](/media/2020-01-06-list-o365-groups-with-spfx/03.png)


## Test the WebPart

1. On the command prompt, type "gulp serve".
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Locate and add the web part (named O365 Groups Listing) to page.

![](/media/2020-01-06-list-o365-groups-with-spfx/04.png)


## Troubleshoot

The page generates an alert "To view the information on this page, you need to verify your identity", to resolve this add below URLs to the trusted site in the browser.

- https://graph.microsoft.com
- https://.office365.com
- https://.microsoftonline.com
- https://.sharepoint.com
- https://.outlook.com


## Summary

Microsoft Graph offers a wide range of APIs to access the content and services provided by Office 365. Listing all O365 groups by using MS Graph API is one of the business cases supporting it.

This content was originally posted [here](https://www.c-sharpcorner.com/article/list-out-office-365-groups-with-spfx/).
