---
title: "PnP WebPartTitle Control for SPFx"
date: "2019-08-18"
---

## Overview

The classic SharePoint web part by default had few basic properties. Web part title is one of it, which can be configured to show short text at the top of web part describing the purpose. The SPFx web parts by default does not provide any similar functionality. PnP (Patterns and Practices) offers WebPartTitle control to show the title at top of web part, which can be changed in edit mode.

During this article, we will explore the WebPartTitle control from PnP on how to use, configure it in SPFx web part. We will develop a practical scenario to integrate WebPartTitle control in SPFx web part.

## Develop SharePoint Framework Web Part

1. Open a command prompt. Create a directory for SPFx solution.

md spfx-pnp-webparttitle

1. Navigate to the above created directory.

cd spfx-pnp-webparttitle

1. Run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-527.png)

- **Solution Name:** Hit enter to have default name (spfx-pnp-webparttitle in this case) or type in any other name for your solution.
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
    - Selected choice: PnPWebPartTitle
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Use PnP WebPartTitle control in SPFx solution
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

We will have to pass the SharePoint context from our web part to the React component to be used in WebPartTitle control.

1. Open React component properties at “src\\webparts\\pnPWebPartTitle\\components\\IPnPWebPartTitleProps.ts”
2. Add below properties.

import { WebPartContext } from '@microsoft/sp-webpart-base';  
import { DisplayMode } from '@microsoft/sp-core-library';  
  
export interface IPnP WebPartTitleProps {  
  description: string;  
  displayMode: DisplayMode;  
  context: WebPartContext;  
  updateProperty: (value: string) => void;  
}

1. From our main web part class (src\\webparts\\pnPWebPartTitle\\PnPWebPartTitleWebPart.ts) pass the context to React component.

export default class PnPWebPartTitleWebPart extends BaseClientSideWebPart<IPnPWebPartTitleWebPartProps> {  
  
  public render(): void {  
    const element: React.ReactElement<IPnPWebPartTitleProps> = React.createElement(  
      PnPWebPartTitle,  
      {  
        description: this.properties.description,  
        displayMode: this.displayMode,  
        updateProperty: (value: string) => {  
          this.properties.description = value;  
        }  
      }  
    );  
  
    ReactDom.render(element, this.domElement);  
  }  
     .  
     .  
     .  
}

Use WebPartTitle Control in the SPFx Web Part

During the implementation, we will use WebPartTitle control to display the description as changeable web part title, when in edit mode.

1. Open the React component file at “src\\webparts\\pnPWebPartTitle\\components\\PnPWebPartTitle.tsx”
2. Add below imports.

import { WebPartTitle } from "@pnp/spfx-controls-react/lib/WebPartTitle";  
import { DisplayMode } from '@microsoft/sp-core-library';  
import { Link } from 'office-ui-fabric-react/lib/Link';

1. Use the WebPartTitle control in the render method as follows.

export default class PnPWebPartTitle extends React.Component<IPnPWebPartTitleProps, {}> {  
  public render(): React.ReactElement<IPnPWebPartTitleProps> {  
    return (  
      <div className={styles.pnPWebPartTitle}>  
        <div className={styles.container}>  
          <div className={styles.row}>  
            <div className={styles.column}>  
              <WebPartTitle displayMode={this.props.displayMode}  
                title={this.props.description}  
                updateProperty={this.props.updateProperty}  
                moreLink={  
                  <Link href="https://sharepoint.github.io/sp-dev-fx-controls-react/">See all</Link>  
                } />  
              <span className={styles.title}>Welcome to SharePoint!</span>  
              <p className={styles.subTitle}>Customize SharePoint experiences using Web Parts.</p>  
              <p className={styles.description}>{escape(this.props.description)}</p>  
              <a href="https://aka.ms/spfx" className={styles.button}>  
                <span className={styles.label}>Learn more</span>  
              </a>  
            </div>  
          </div>  
        </div>  
      </div>  
    );  
  }  
}

Test the PnP WebPartTitle Control

1. On the command prompt, type “gulp serve”.
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Locate and add the webpart (named PnPWebPartTitle) to page.

The web part title can be set, when the page is in edit mode.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-528.png)

## Summary

In this article, we explored the practical use of WebPartTitle control in SPFx web part. The WebPartTitle control helps to show title at top of web part, which can be changed in edit mode.

## References

- PnP WebPartTitle Control ([https://sharepoint.github.io/sp-dev-fx-controls-react/controls/WebPartTitle/](https://sharepoint.github.io/sp-dev-fx-controls-react/controls/WebPartTitle/))
- PnP Reusable React Controls ([https://sharepoint.github.io/sp-dev-fx-controls-react/](https://sharepoint.github.io/sp-dev-fx-controls-react/))

This content was originally posted [here](https://www.c-sharpcorner.com/article/pnp-webparttitle-control-for-spfx/).
