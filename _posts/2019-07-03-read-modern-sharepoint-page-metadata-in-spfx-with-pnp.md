---
title: "Read Modern SharePoint Page Metadata in SPFx WebPart with PnP"
date: "2019-07-03"
---

## Overview

SharePoint modern pages with page metadata provide more capabilities for end users to tag the content more appropriately​. In certain development scenarios, we have to read these page properties in SPFx web part. SharePoint PnP (Pattern and Practices) provides simple APIs for reading page properties.

In this article, we will explore how to add page properties to the modern page and read it inside SPFx web part using PnP.

 

## Add page properties to the modern page

Microsoft has provided an easy interface to handle page properties on the modern page, leaving behind the complexities with classic SharePoint of adding the fields to content type (which was not an easy option for the end users).

Follow the below steps to create a new page property on a modern page.

1. Navigate to Pages library.
2. Click “Add column” to add new columns to pages library.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-548.png)

1. Add columns as below.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-549.png)

 

## Set Individual Page Property Values

Now that we have defined custom properties, we will add values for those properties at the individual page level.

1. Open a SharePoint page from Pages library.
2. Click Edit.
3. Click “Page details”.
4. Specify values for our custom page properties.
5. Click Republish.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-550.png)

 

## SPFx Web Part to read page properties

Modern SharePoint offers out of box web part named "Page properties" to display the properties on a page. However, in a few scenarios, we have to read the page properties and process it further.

1. Open a command prompt. Create a directory for SPFx solution.

md spfx-read-page-properties

1. Navigate to the above created directory.

cd spfx-read-page-properties

1. Run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-551.png)

- **Solution Name:** Hit enter to have default name (spfx-read-page-properties in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for the component:** Here we can select the target environment where we are planning to deploy the client web part i.e. SharePoint Online or SharePoint On-Premises (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a sub-folder for our solution.
    - Selected choice: Same folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: N (install on each site explicitly)
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create a client side web part or an extension. Choose web part option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: PagePropertiesRead
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Read page properties using PnP
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. In the command prompt type below command to open the solution in the code editor of your choice.

code .

 

NPM Packages Used

**@pnp/sp** ([https://www.npmjs.com/package/@pnp/sp](https://www.npmjs.com/package/@pnp/sp))

On the command prompt, run below command.

npm i @pnp/logging @pnp/common @pnp/odata @pnp/sp --save

 

Setup SP PnP for the Web Part

We will start by setting up the PnP to be used in our SPFx web part.

1. Open web part file at src\\webparts\\pagePropertiesReader\\PagePropertiesReaderWebPart.ts
2. Import PnP.

// @pnp/sp imports    
import { sp, Web } from '@pnp/sp';

1. Implement OnInit method to setup PnP.

public onInit(): Promise<void> {    
  return super.onInit().then(\_ => {    
    sp.setup({    
      spfxContext: this.context    
    });    
  });    
}

 

Pass Page Context from Web Part to React Component

React component cannot directly access the SharePoint context. It has to be passed from the web part to the React component.

1. Add pageContext property to React Props at src\\webparts\\pagePropertiesReader\\components\\IPagePropertiesReaderProps.ts

import { PageContext } from '@microsoft/sp-page-context';  
  
export interface IPagePropertiesReaderProps {  
  description: string;  
  pageContext: PageContext;  
}

1. Set the pageConext inside render method of web part at src\\webparts\\pagePropertiesReader\\PagePropertiesReaderWebPart.ts

export default class PagePropertiesReaderWebPart extends BaseClientSideWebPart<IPagePropertiesReaderWebPartProps> {  
.  
.  
.  
  public render(): void {  
    const element: React.ReactElement<IPagePropertiesReaderProps > = React.createElement(  
      PagePropertiesReader,  
      {  
        description: this.properties.description,  
        pageContext: this.context.pageContext  
      }  
    );  
  
    ReactDom.render(element, this.domElement);  
  }  
.  
.  
.  
}

 

Define the State

Let us define the state to store the custom page property values. Create a file named IPagePropertiesReaderState.ts at “src\\webparts\\pagePropertiesReader\\components\\”.

export interface IPagePropertiesReaderState {  
    department: string;  
    freeText: string;  
}

 

Code the Web Part

Update the React component at “src\\webparts\\pagePropertiesReader\\components\\PagePropertiesReader.tsx” to read the page properties using PnP.

import \* as React from 'react';  
import styles from './PagePropertiesReader.module.scss';  
import { IPagePropertiesReaderProps } from './IPagePropertiesReaderProps';  
import { IPagePropertiesReaderState } from './IPagePropertiesReaderState';  
import { escape } from '@microsoft/sp-lodash-subset';  
  
// @pnp/sp imports    
import { sp } from '@pnp/sp';  
  
export default class PagePropertiesReader extends React.Component<IPagePropertiesReaderProps, IPagePropertiesReaderState> {  
  
  constructor(props: IPagePropertiesReaderProps, state: IPagePropertiesReaderState) {  
    super(props);  
  
    this.state = {  
      department: "",  
      freeText: ""  
    };  
  }  
  
  componentDidMount() {    
    sp.web.lists.getById(this.props.pageContext.list.id.toString())  
      .items.getById(this.props.pageContext.listItem.id)  
        .select("Department,FreeText")  
          .get()  
            .then(d => {  
              console.log(d);  
              this.setState({  
                department: d.Department,  
                freeText: d.FreeText  
              });  
            });  
  }  
  
  public render(): React.ReactElement<IPagePropertiesReaderProps> {  
    return (  
      <div className={ styles.pagePropertiesReader }>  
        <div className={ styles.container }>  
          <div className={ styles.row }>  
            <div className={ styles.column }>  
              <span className={ styles.title }>Welcome to SharePoint!</span>  
              <p className={ styles.subTitle }>Page Properties</p>  
              <p className={ styles.description }>Department: {escape(this.state.department)}</p>  
              <p className={ styles.description }>FreeText: {escape(this.state.freeText)}</p>  
            </div>  
          </div>  
        </div>  
      </div>  
    );  
  }  
}

 

Test the web part

We will test the web part on a live SharePoint page, where we have set the custom property values.

1. On the command prompt, type “gulp serve --nobrowser”.
2. Open the SharePoint page with custom property values set. Append the below text to URL in the browser.

?loadSPFX=true&debugManifestsFile=https://localhost:4321/temp/manifests.js

1. Click “Load debug scripts”, when prompted.
2. Edit the page and add the web part (named PagePropertiesReader) to page.
3. Verify the page property values getting displayed in our web part.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-552.png)

 

## Summary

Page metadata provides more capabilities for end users to tag the content more appropriately in SharePoint. Modern SharePoint out of the box provides a web part named "Page properties". PnP provides simple APIs to read the custom page property values and process in SPFx web part.

 

This content was originally posted [here](https://www.c-sharpcorner.com/article/read-modern-sharepoint-page-metadata-in-spfx-webpart-using-pnp/).
