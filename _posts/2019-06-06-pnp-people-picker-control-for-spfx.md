---
title: "PnP People Picker Control for SPFx"
date: "2019-06-06"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-06-06-pnp-people-picker-control-for-spfx/02.png
  teaser: media/2019-06-06-pnp-people-picker-control-for-spfx/02.png
tags:
  - "2019"
  - June 2019
last_modified_at: 2019-06-06T00:00:00-00:00
---

## Overview

PnP has provided a control that renders as People picker field, which can be used to select one or more users from SharePoint site or group. This control is useful to be used in SPFx web part to get the people information from users. People Picker control offers various configuration options to support most of the business needs.

During this article, we will explore the People Picker control from PnP on how to use, configure it in SPFx web part. We will develop a practical scenario to capture the people information in SPFx web part using PnP People Picker Control and store it in SharePoint list.
 

## Develop SharePoint Framework Web Part

1. Open a command prompt. Create a directory for SPFx solution.

    ```
    md spfx-pnp-people-picker
    ```

2. Navigate to the above created directory.

    ```
    cd spfx-pnp-people-picker
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-06-06-pnp-people-picker-control-for-spfx/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-pnp-people-picker in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for the component:** Here we can select the target environment where we are planning to deploy the client web part i.e. SharePoint Online or SharePoint On-Premises (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the same folder or create a sub-folder for our solution.
        - Selected choice: Same folder
    - **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
        - Selected choice: N (install on each site explicitly)
    - **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
        - Selected choice: N (solution contains unique permissions)
    - **Type of client-side component to create:** We can choose to create client side web part or an extension. Choose web part option.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: PnPPeoplePicker
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Use PnP People Picker control in SPFx solution
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: React

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in the code editor of your choice.

    ```
    code .
    ```


## NPM Packages Used

**@pnp/spfx-controls-react** ([https://sharepoint.github.io/sp-dev-fx-controls-react/](https://sharepoint.github.io/sp-dev-fx-controls-react/))

On the command prompt, run below command to include the npm package.

```
npm install @pnp/spfx-controls-react --save
```


**@pnp/sp** ([https://www.npmjs.com/package/@pnp/sp](https://www.npmjs.com/package/@pnp/sp))

On the command prompt, run below command.

```
npm i @pnp/logging @pnp/common @pnp/odata @pnp/sp --save
```

 
## Pass the context from web part to React Component

As we need the SharePoint context to work with people picker, we will have to pass it from our web part to the React component.

1. Open React component properties at "src\webparts\pnPPeoplePicker\components\IPnPPeoplePickerProps.ts”
2. Add below properties.

    ```typescript
    import { WebPartContext } from '@microsoft/sp-webpart-base';  
      
    export interface IPnPPeoplePickerProps {  
      description: string;  
      context: WebPartContext;  
    }
    ```

3. From our web part (src\webparts\pnPPeoplePicker\PnPPeoplePickerWebPart.ts) pass the context to React component.

    ```typescript
    export default class PnPPeoplePickerWebPart extends BaseClientSideWebPart<IPnPPeoplePickerWebPartProps> {  
      
      public render(): void {  
        const element: React.ReactElement<IPnPPeoplePickerProps > = React.createElement(  
          PnPPeoplePicker,  
          {  
            description: this.properties.description,  
            context: this.context  
          }  
        );  
      
        ReactDom.render(element, this.domElement);  
      }  
      .  
      .  
      .  
    }
    ```


## Code the Web Part

1. On the command prompt type below command to open the solution in the code editor of your choice.

    ```
    code .
    ```

2. Open the React component file at "src\webparts\pnPPeoplePicker\components\PnPPeoplePicker.tsx"
3. Add below imports.

    ```typescript
    import { PeoplePicker, PrincipalType } from "@pnp/spfx-controls-react/lib/PeoplePicker";
    ```

4. Use the PeoplePicker control in the render method as follows.

    ```typescript
    export default class PnPPeoplePicker extends React.Component<IPnPPeoplePickerProps, {}> {    
      public render(): React.ReactElement<IPnPPeoplePickerProps> {    
        return (    
          <div className={ styles.pnPPeoplePicker }>    
            <div className={ styles.container }>    
              <div className={ styles.row }>    
                <div className={ styles.column }>    
                  <span className={ styles.title }>Welcome to SharePoint!</span>    
                  <p className={ styles.subTitle }>Customize SharePoint experiences using Web Parts.</p>    
                      
                  <PeoplePicker    
                    context={this.props.context}    
                    titleText="People Picker"    
                    personSelectionLimit={3}    
                    groupName={""} // Leave this blank in case you want to filter from all users    
                    showtooltip={true}    
                    isRequired={true}    
                    disabled={false}    
                    ensureUser={true}    
                    selectedItems={this.\_getPeoplePickerItems}    
                    showHiddenInUI={false}    
                    principalTypes={\[PrincipalType.User\]}    
                    resolveDelay={1000} />        
                </div>    
              </div>    
            </div>    
          </div>    
        );    
      }    
    }
    ```

    In the PeoplePicker component, set ensure user property to true. It will return the local user ID on the current site.

5. Implement selectedItems property to get the selected People from the PeoplePicker.

    ```typescript
    private _getPeoplePickerItems(items: any[]) {  
      console.log('Items:', items);  
    }
    ```


## Define the State

Let us define the state to store the selected user ids.

Add file IPnPPeoplePickerState.ts under folder "\src\webparts\pnPPeoplePicker\components\".

```typescript
export interface IPnPPeoplePickerState {  
  addUsers: string[];  
}
```

Update the React component "\src\webparts\pnPPeoplePicker\components\PnPPeoplePicker.tsx" to use the state.

```typescript
import * as React from 'react';    
import styles from './PnPPeoplePicker.module.scss';    
import { IPnPPeoplePickerProps } from './IPnPPeoplePickerProps';    
import { IPnPPeoplePickerState } from './IPnPPeoplePickerState';    
    
// @pnp/sp imports      
import { sp, Web } from '@pnp/sp';    
    
import { PeoplePicker, PrincipalType } from "@pnp/spfx-controls-react/lib/PeoplePicker";    
    
export default class PnPPeoplePicker extends React.Component<IPnPPeoplePickerProps, IPnPPeoplePickerState> {      
    
  constructor(props: IPnPPeoplePickerProps, state: IPnPPeoplePickerState) {    
    super(props);    
    
    this.state = {    
      addUsers: []    
    };    
  }    
  .    
  .    
  .    
}
```


## Define the controls

Let us add a button control. On click of the button, we will add the selected users from people picker to the SharePoint list.

1. Open React component PnPPeoplePicker.tsx at folder "src\webparts\pnPPeoplePicker\components\".
2. Add below import for button control.

    ```typescript
    // Import button component      
    import { IButtonProps, DefaultButton } from 'office-ui-fabric-react/lib/Button';   
    import { autobind } from 'office-ui-fabric-react';
    ```

3. Define a button inside the render method.

    ```tsx
    <DefaultButton    
      data-automation-id="addSelectedUsers"    
      title="Add Selected Users"    
      onClick={this.addSelectedUsers}>    
      Add Selected Users    
    </DefaultButton>
    ```

4. Implement the supporting methods as below.

    ```typescript
    @autobind   
    private addSelectedUsers(): void {    
      sp.web.lists.getByTitle("SPFx Users").items.add({  
        Title: getGUID(),  
        Users: {   
            results: this.state.addUsers  
        }  
      }).then(i => {  
          console.log(i);  
      });  
    }
    ```


## Add Selected Users to SharePoint List

We are adding the selected users to React component state using addSelectedUsers method of PeoplePicker PnP control. Now, we will implement a logic to add the selected users from React state to actual SharePoint list.

1. Add below imports.

    ```typescript
    // @pnp/sp imports    
    import { sp } from '@pnp/sp';    
    import { getGUID } from "@pnp/common";  
      
    // Import button component      
    import { DefaultButton } from 'office-ui-fabric-react/lib/Button';   
    import { autobind } from 'office-ui-fabric-react';
    ```

2. Add button insider Render method.

    ```tsx
    <DefaultButton    
      data-automation-id="addSelectedUsers"    
      title="Add Selected Users"    
      onClick={this.addSelectedUsers}>    
      Add Selected Users    
    </DefaultButton>
    ```

3. Implement addSelectedUsers method.

    ```typescript
    @autobind   
    private addSelectedUsers(): void {    
      sp.web.lists.getByTitle("SPFx Users").items.add({  
        Title: getGUID(),  
        Users: {   
            results: this.state.addUsers  
        }  
      }).then(i => {  
          console.log(i);  
      });
    }
    ```
 

## Setup SharePoint List

Setup SharePoint list (named "SPFx Users") with below schema.

**Column**|**Type**|**Comments**
Title|Single line of text|Out of box title column
Users|Person or Group|Set "Allow multiple selections" to Yes


## Test the PnP People Picker

1. On the command prompt, type ```gulp serve```.
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Locate and add the webpart (named PnPPeoplePicker) to page.
5. Type in the user names in the people picker.
6. Click "Add Selected Users".

    ![](/media/2019-06-06-pnp-people-picker-control-for-spfx/02.png)

7. The selected users should get added to the SharePoint list.

    ![](/media/2019-06-06-pnp-people-picker-control-for-spfx/03.png)


## Summary

In this article, we explore the practical use of People Picker control in SPFx web part. We configured the PnP People Picker control to capture the information from the user and used PnP list item operation to add the information to SharePoint list.


## References

- [PnP People Picker](https://sharepoint.github.io/sp-dev-fx-controls-react/controls/PeoplePicker/)
- [PnP list item operations](https://pnp.github.io/pnpjs/sp/docs/items/)

This content was originally posted [here](https://www.c-sharpcorner.com/article/pnp-people-picker-control-for-spfx/).
