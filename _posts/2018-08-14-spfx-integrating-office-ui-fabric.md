---
title: "SharePoint Framework - Integrating Office UI Fabric"
date: "2018-08-14"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-14-spfx-integrating-office-ui-fabric/03.png
  teaser: media/2018-08-14-spfx-integrating-office-ui-fabric/03.png
tags:
  - "2018"
  - August 2018
last_modified_at: 2018-08-14T00:00:00-00:00
---

## Overview

SharePoint Framework (SPFx) offers lightweight, responsive client side web part development for classic as well modern SharePoint. In this article, we will explore how we can integrate the Office UI Fabric in to SharePoint Framework.


## The UI Challenges

In the SharePoint journey so far, we have been developing custom web parts. Developing the visual interface for each of the custom web part had involved significant efforts from SharePoint developers as well UX designers and UI developers to make the web part look like an integral part of the SharePoint portal. Huge efforts were invested in designing the CSS classes and placing the required controls (like labels, textboxes, buttons, etc.) on the web part. This also had its own challenge to rewrite while re-branding the SharePoint portals.

Office UI Fabric handles this challenges gracefully for SharePoint developers and designers so that they can build the client side web parts which can resemble their appearance to Office 365 and provide responsiveness.


## Brief information about Office UI Fabric

Office UI Fabric is official front end framework to build the experiences that fit seamlessly in Office 365. The Office UI Fabric components are built with React JS. Microsoft uses Fabric Core and Fabric React which offers numerous components and styles. Read more about Office UI Fabric at [https://developer.microsoft.com/en-us/fabric](https://developer.microsoft.com/en-us/fabric)


## Office UI Fabric for SharePoint Framework

SharePoint Framework Fabric Core npm package (@microsoft/sp-office-ui-fabric-core) is a subset of Fabric core styles that can be easily integrated with SharePoint Framework. Yeoman generator for SharePoint Framework (from version 1.3.4 onwards) supports Office UI Fabric by default.

To install Fabric Core package in your existing project, use below command.

```
npm install @microsoft/sp-office-ui-fabric-core --save
```

To use Fabric Core styles, use below declaration.

```typescript
@import '~@microsoft/sp-office-ui-fabric-core/dist/sass/SPFabricCore.scss';
```

However, it is recommended to use Office UI Fabric React package with SharePoint Framework.


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-officeuifabric
    ```

2. Navigate to above created directory.

    ```
    cd spfx-officeuifabric
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-08-14-spfx-integrating-office-ui-fabric/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-officeuifabric in this case) or type in any other name for your solution.
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
        - Selected choice: OfficeUIFabricExamples
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Office UI Fabric Integration with SPFx
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: React

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## Office UI Fabric Components

Office UI Fabric majorly have below categorized components:

**Basic Inputs**

- Button
- Checkbox
- ChoiceGroup
- ComboBox
- ContextualMenu
- Dropdown
- Label
- Link
- Rating
- Slider
- SpinButton
- TextField
- Toggle


**Navigation**

- Breadcrumb
- CommandBar
- Nav
- OverflowSet
- Pivot
- SearchBox


**Content**

- ActivityItem
- Calendar
- DetailsList
- Facepile
- GroupedList
- Icon
- Image
- List
- Persona


**Pickers**

- Pickers
- ColorPicker
- DatePicker
- PeoplePicker
- SwatchColorPicker


**Progress & Validation**

- MessageBar
- ProgressIndicator
- Shimmer
- Spinner


**Surfaces**

- Callout
- Dialog
- DocumentCard
- HoverCard
- Layer
- Modal
- Overlay
- Panel
- ScrollablePane
- TeachingBubble
- Tooltip
- Coachmark


**Utilities**

- FocusTrapZone
- FocusZone
- MarqueeSelection
- ResizeGroup
- Selection
- Themes


## How to use Office UI Fabric Components in SPFx WebPart

1. Navigate to Office UI Fabric Components site at [https://developer.microsoft.com/en-us/fabric#/components](https://developer.microsoft.com/en-us/fabric#/components).
2. Select and expand the Category of your component.
3. Click the component node.
4. Below is an example of selecting a button component.

    ![](/media/2018-08-14-spfx-integrating-office-ui-fabric/02.png)

5. Alternatively, search your component from search box.
6. Click **Variants** tab to see the variations for use of the component.

    ![](/media/2018-08-14-spfx-integrating-office-ui-fabric/03.png)

7. Click **Show code** to get the code for the component.

    ![](/media/2018-08-14-spfx-integrating-office-ui-fabric/04.png)

8. Copy entire code or part of it to use in your SPFx solution.


## Implement Greet Message WebPart using Office UI Fabric

Using the Office UI Fabric components, we will implement simple webpart, having below components

- Textbox: Accepts user name.

- Button: with text as "Greet" on click of which will show an alert greeting the text typed in textbox.

1. In the solution, add file IComponentState.ts, which will represent the state of entered user name.

    ```typescript
    export interface IComponentState {  
        userName: string;  
    }
    ```

2. Open OfficeUiFabricExamples.tsx under "\src\webparts\officeUiFabricExamples\components\" folder.
3. Import Textfield and button components.

    ```typescript
    // Import Textfield component  
    import { TextField } from 'office-ui-fabric-react/lib/TextField';  
      
    // Import Button component  
    import { IButtonProps, DefaultButton } from 'office-ui-fabric-react/lib/Button';
    ```

4. Add components to Render method.

    ```typescript
    export default class OfficeUiFabricExamples extends React.Component<IOfficeUiFabricExamplesProps, IComponentState> {  
      constructor(props: IOfficeUiFabricExamplesProps, state: IComponentState) {  
        super(props);  
      
        this.state = ({  
          userName: ''  
        });  
      
        this._greetClicked = this._greetClicked.bind(this);  
      }  
      
      public render(): React.ReactElement<IOfficeUiFabricExamplesProps> {  
        return (  
          <div className={ styles.officeUiFabricExamples }>  
            <div className={ styles.container }>  
              <div className={ styles.row }>  
                <div className={ styles.column }>  
                  <div className="docs-TextFieldExample">  
                    <TextField   
                      required={true}   
                      name="txtUserName"   
                      placeholder="Your name please!"  
                      value={this.state.userName}  
                      onChanged={e => this.setState({ userName: e })}  
                    />  
      
                    <DefaultButton  
                      data-automation-id="greet"  
                      target="_blank"  
                      title="Greet the user!"  
                      onClick={this._greetClicked}  
                      >  
                      Greet  
                    </DefaultButton>  
      
                    <p>{this.state.userName}</p>  
                  </div>  
                </div>  
              </div>  
            </div>  
          </div>  
        );  
      }  
      
      private _greetClicked(): void {  
        alert('Hello ' + this.state.userName);  
      }    
    }
    ```


## Test the web part

1. On the command prompt, type ```gulp serve```.
2. SharePoint local workbench will open.
3. Add the webpart to page.
4. Enter text in TextBox component, click Greet button. An alert should appear greeting the value entered in textbox

    ![](/media/2018-08-14-spfx-integrating-office-ui-fabric/05.png)


## Summary

Office UI Fabric React components helps to develop robust and consistent design across SharePoint modern experiences. Office UI Fabric is available out of the box for developers in SharePoint Framework.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-integrating-office-ui-fabric/).
