---
title: "SharePoint Framework - React AutoBind Events"
date: "2019-01-05"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-01-05-spfx-react-autobind-events/03.png
  teaser: media/2019-01-05-spfx-react-autobind-events/03.png
tags:
  - "2019"
  - January 2019
last_modified_at: 2019-01-05T00:00:00-00:00
---

## Overview

SharePoint Framework (SPFx) based web parts are key to develop custom solutions in modern SharePoint. A web part can contain various controls in it. Some controls helps to get the inputs from end users (like textbox, dropdowns, choices, etc.). Also some controls generate an events to process the user inputs (e.g. button click event). The event defines what happens when certain action is taken on the control. As a developer, we need to define the piece of code that should run when an action is triggered on control.

In this article, we will explore how we can bind actions to controls in SharePoint framework solutions developed using React. Also how we can autobind the events to the control.


## Develop SharePoint Framework Web Part

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-react-autobind
    ```

2. Navigate to above created directory.

    ```
    cd spfx-react-autobind
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-01-05-spfx-react-autobind-events/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-react-autobind in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the current folder or create a subfolder for our solution.
        - Selected choice: Use the current folder
    - **Deployment option:** We may choose to allow the tenant admin the choice of being able to deploy the solution to all sites immediately without running any feature deployment or adding apps in sites.
        - Selected choice: N (install on each site explicitly)
    - **Permissions to access Web APIs:** We may choose to the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
        - Selected choice: N (No explicit permissions are required)
    - **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: ReactAutoBind
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Auto bind events in React
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


## Development Scenario

We will add a textbox and button to our webpart. On the click of button, we will display greeting message to text entered in textbox.


## Define a State

Let us define a state to store the user entered text. Add a file IReactAutoBindState.ts under "\src\webparts\reactAutoBind\components\" folder.

```typescript
export interface IReactAutoBindState {  
    userText: string;  
}
```


## Add Controls to Web Part

1. Open ReactAutoBind.tsx under "\src\webparts\reactAutoBind\components\" folder.
2. Add below imports.

    ```typescript
    // Import text field component   
    import { TextField } from 'office-ui-fabric-react/lib/TextField';  

    // Import button component    
    import { IButtonProps, DefaultButton } from 'office-ui-fabric-react/lib/Button';
    ```

3. Define textbox and button controls.

    ```typescript
    public render(): React.ReactElement<IReactAutoBindProps> {  
      return (  
        <div className={styles.reactAutoBind}>  
          <div className={styles.container}>  
            <div className={styles.row}>  
              <div className={styles.column}>  
                <span className={styles.title}>Welcome to SharePoint!</span>  
                <p className={styles.subTitle}>Autobind events demo</p>  
      
                <TextField  
                  required={true}  
                  name="txtSearchText"  
                  placeholder="Search..."  
                  value={this.state.userText}  
                  onChanged={e => this.setState({ userText: e })}  
                />  
      
                <DefaultButton  
                  data-automation-id="search"  
                  target="_blank"  
                  title="Greet"  
                  onClick={this.greetButtonClicked}  
                >  
                  Greet  
                </DefaultButton>  
      
                <div>{this.state.userText}</div>  
      
              </div>  
            </div>  
          </div>  
        </div>  
      );  
    }
    ```

4. Define button click event.

    ```typescript
    private greetButtonClicked(): void {  
        alert("Hello " + this.state.userText);  
    }
    ```

5. Define constructor to set the initial state.

    ```typescript
    constructor(props: IReactAutoBindProps, state: IReactAutoBindState) {  
      super(props);  
      
      this.state = {  
        userText: ""  
      }  
    }
    ```


## Run the SPFx WebPart

1. On the command prompt, type ```gulp serve```.
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Type in some text to the textbox.
6. Click **Greet** button.

    ![](/media/2019-01-05-spfx-react-autobind-events/02.png)

    We are expected to see the alert message greeting the text entered in the textbox. But nothing happens. If we observe the developer toolbar console, we have an error “Cannot read property 'state' of undefined at ReactAutoBind.greetButtonClicked”. This means our event has not bind to our button control.
 

## Bind event to button

In the constructor add below code:

```typescript
constructor(props: IReactAutoBindProps, state: IReactAutoBindState) {  
  super(props);  
  
  this.state = {  
    userText: ""  
  }  
  
  this.greetButtonClicked = this.greetButtonClicked.bind(this);  
}
```

Refresh the SharePoint workbench. Add some text to textbox and click Greet button. As the event is bind to the button, we see an alert.

![](/media/2019-01-05-spfx-react-autobind-events/03.png)
 

## Binding all events at one

There are chances that we might forget to add the bindings in constructor for our control. As the number of control grows on web part it might be a huge code for only event binding. To handle this in a graceful way, we can use autobind.

1. Add below import.

    ```typescript
    import { autobind } from 'office-ui-fabric-react';
    ```

2. Remove any bindings from Constructor.
3. Decorate event with @autobind.

    ```typescript
    @autobind  
    private greetButtonClicked(): void {  
      alert("Hello " + this.state.userText);  
    }
    ```


## Summary

It is necessary to bind actions to controls. An action can be bind in constructor individually to the control. Autobind helps to bind the action to control by decorating the method.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-react-autobind-events/).
