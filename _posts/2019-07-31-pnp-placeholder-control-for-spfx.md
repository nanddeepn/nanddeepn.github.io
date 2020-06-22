---
title: "PnP Placeholder Control for SPFx"
date: "2019-07-31"
---

## Overview

SharePoint Framework web part usually offers configurable web part properties to configure itself. The values for web part properties can be setup later by end user to configure it as per need. If the needed properties are not set on the web part, the same should be notified to the end user in a graceful way. PnP (Patterns and Practices) offers Placeholder control to show the message for pending configurations.

During this article, we will explore the Placeholder control from PnP on how to use, configure it in SPFx web part. We will develop a practical scenario to integrate Placeholder control in SPFx web part.

## Develop SharePoint Framework Web Part

1. Open a command prompt. Create a directory for SPFx solution.

md spfx-pnp-placeholder

1. Navigate to the above created directory.

cd spfx-pnp-placeholder

1. Run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-529.png)

- **Solution Name:** Hit enter to have default name (spfx-pnp-placeholder in this case) or type in any other name for your solution.
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
    - Selected choice: PnPPlaceholder
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Use PnP Placeholder control in SPFx solution
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. In the command prompt type below command to open the solution in the code editor of your choice.

code .

 

NPM Packages Dependency

**@pnp/spfx-controls-react** ([https://sharepoint.github.io/sp-dev-fx-controls-react/](https://sharepoint.github.io/sp-dev-fx-controls-react/))

SharePoint framework React controls is an open source library which offers reusable set of React controls to be used in SPFx solution.

On the command prompt, run below command to include the npm package.

npm install @pnp/spfx-controls-react --save

 

Pass the context from web part to React Component

We will have to pass the SharePoint context from our web part to the React component to be used in Placeholder control.

1. Open React component properties at “src\\webparts\\pnPPlaceholder\\components\\IPnPPlaceholderProps.ts”
2. Add below properties.

import { WebPartContext } from '@microsoft/sp-webpart-base';  
import { DisplayMode } from '@microsoft/sp-core-library';  
  
export interface IPnPPlaceholderProps {  
  description: string;  
  displayMode: DisplayMode;  
  context: WebPartContext;  
}

1. From our web part (src\\webparts\\pnPPlaceholder\\PnPPlaceholderWebPart.ts) pass the context to React component.

export default class PnPPlaceholderWebPart extends BaseClientSideWebPart<IPnPPlaceholderWebPartProps> {  
  
  public render(): void {  
    const element: React.ReactElement<IPnPPlaceholderProps> = React.createElement(  
      PnPPlaceholder,  
      {  
        description: this.properties.description,  
        displayMode: this.displayMode,  
        context: this.context  
      }  
    );  
  
    ReactDom.render(element, this.domElement);  
  }  
     .  
     .  
     .  
}

 

Use Placeholder Control in the SPFx Web Part

During the implementation, we will use Placeholder control to notify the user for missing configuration, if the out of box description property is not set. We will show the description, when set instead of Placeholder control.

1. On the command prompt, type below command to open the solution in the code editor of your choice.

code .

1. Open the React component file at “src\\webparts\\pnPPlaceholder\\components\\PnPPlaceholder.tsx”
2. Add below imports.

import { Placeholder } from "@pnp/spfx-controls-react/lib/Placeholder";  
import { DisplayMode } from '@microsoft/sp-core-library';

1. Use the Placeholder control in the render method as follows.

export default class PnPPlaceholder extends React.Component<IPnPPlaceholderProps, {}> {  
  public render(): React.ReactElement<IPnPPlaceholderProps> {  
    return (  
      <div className={styles.pnPPlaceholder}>  
        <div className={styles.container}>  
          <div className={styles.row}>  
            <div className={styles.column}>  
              <span className={styles.title}>Welcome to SharePoint!</span>  
              <p className={styles.subTitle}>Customize SharePoint experiences using Web Parts.</p>  
  
              {/\* Show Placeholder control, when description web part property is not set \*/}  
              {this.props.description === "" &&  
                <Placeholder iconName='Edit'  
                  iconText='Configure your web part'  
                  description='Please configure the web part.'  
                  buttonLabel='Configure'  
                  hideButton={this.props.displayMode === DisplayMode.Read}  
                  onConfigure={this.\_onConfigure} />  
              }  
  
              {/\* Show description web part property, when set \*/}  
              {this.props.description &&  
                <p className={styles.description}>{escape(this.props.description)}</p>  
              }  
            </div>  
          </div>  
        </div>  
      </div>  
    );  
  }  
  
  private \_onConfigure() {  
    // Context of the web part  
    this.props.context.propertyPane.open();  
  }  
}

The onConfigure property defines the action to be executed on the button click event (e.g. opening the property pane).

 

## Test the PnP Placeholder Control

1. On the command prompt, type “gulp serve”.
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Locate and add the webpart (named PnPPlaceHolder) to page.

The web part by default is displayed as below (when the description web part property is set).

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-530.png)

When the description web part property is not set, it will prompt user to configure the web part.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-531.png)

## Summary

In this article, we explored the practical use of Placeholder control in SPFx web part. The Placeholder control helps to show message to the end user, if web part configuration is pending.

## References

- PnP Placeholder Control ([https://sharepoint.github.io/sp-dev-fx-controls-react/controls/Placeholder/](https://sharepoint.github.io/sp-dev-fx-controls-react/controls/Placeholder/))
- PnP Reusable React Controls ([https://sharepoint.github.io/sp-dev-fx-controls-react/](https://sharepoint.github.io/sp-dev-fx-controls-react/))

This content was originally posted [here](https://www.c-sharpcorner.com/article/pnp-placeholder-control-for-spfx/).
