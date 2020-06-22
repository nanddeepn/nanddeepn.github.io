---
title: "Using React Components in SPFx Extension Application Customizer"
date: "2019-04-09"
---

## Overview

SharePoint Framework (SPFx) Extensions extends SharePoint user experience by customizing predefined placeholders in SharePoint site. As of today we can extend the top and bottom placeholders to customize header and footer section. React components helps to develop functionality independently by maintaining separation of concerns. These React components can be used as part of application customizer.

In this article, we will explore the how we can use React components in Application Customizer part of SharePoint extensions.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-react-applicationcustomizer

1. Navigate to above created directory.

cd spfx-react-applicationcustomizer

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-366.png)

- **Solution Name:** Hit enter to have default name (spfx-react-applicationcustomizer in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Use the curent folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: Y
- **Permissions to access web APIs:** Choose if the components in the solution require permissions to access web APIs that are unique and not shared with other components in the tenant.
    - Selected choice: N (solution contains unique permissions)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension.
    - Selected choice: Extension
- **Type of client-side extension to create:** We can choose to create Application customizer, Field customizer, or ListView Command Set.
    - Selected choice: Application Customizer
- **Application customizer name:** Hit enter to select the default name or type in any other name.
    - Selected choice: ReactHeaderFooter
- **Application customizer description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Adds custom header and footer to SharePoint site

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

1. To use Office 365 UI Fabric controls, type below command. We will explicitly install the 5.x version of the Office UI Fabric to our solution.

npm install office-ui-fabric-react@5.132.0 --save

One thing to note here is that, during the solution creation Yeoman generator did not ask us for the choice of language to use.

## Implement Application Customizer

Open ReactHeaderFooterApplicationCustomizer.ts under “\\src\\extensions\\ reactHeaderFooter \\” folder.

To get access to placeholders on the page, use below imports.

import {  
  BaseApplicationCustomizer,  
  PlaceholderContent,  
  PlaceholderName  
} from '@microsoft/sp-application-base';

Update the interface IReactHeaderFooterApplicationCustomizerProperties to include Top and Bottom properties.

export interface IReactHeaderFooterApplicationCustomizerProperties {  
  // This is an example; replace with your own property  
  Bottom: string;  
}

Implement OnInit method.

private \_bottomPlaceholder: PlaceholderContent | undefined;  
  
public onInit(): Promise<void> {  
    Log.info(LOG\_SOURCE, \`Initialized ${strings.Title}\`);  
  
    // Added to handle possible changes on the existence of placeholders.  
    this.context.placeholderProvider.changedEvent.add(this, this.\_renderPlaceHolders);  
      
    // Call render method for generating the HTML elements.  
    this.\_renderPlaceHolders();  
  
    return Promise.resolve();  
  }

Implement \_renderPlaceHolders method.

private \_renderPlaceHolders(): void {  
    console.log('Available placeholders: ',  
    this.context.placeholderProvider.placeholderNames.map(name => PlaceholderName\[name\]).join(', '));  
       
    // Handling the bottom placeholder  
    if (!this.\_bottomPlaceholder) {  
      this.\_bottomPlaceholder =  
        this.context.placeholderProvider.tryCreateContent(  
          PlaceholderName.Bottom,  
          { onDispose: this.\_onDispose });  
      
      // The extension should not assume that the expected placeholder is available.  
      if (!this.\_bottomPlaceholder) {  
        console.error('The expected placeholder (Bottom) was not found.');  
        return;  
      }  
    }  
  }

Use this.context.placeholderProvider.tryCreateContent to get access the placeholder, without assuming that the placeholder will exists.

Implement OnDispose method.

private \_onDispose(): void {  
    console.log('\[ReactHeaderFooterApplicationCustomizer.\_onDispose\] Disposed custom top and bottom placeholders.');  
}

 

Implement React Component as Footer

Add new file to solution – ReactFooter.tsx

import \* as React from "react";  
import { Link } from 'office-ui-fabric-react/lib/Link';  
import { CommandBar } from 'office-ui-fabric-react/lib/CommandBar';  
  
export interface IReactFooterProps {}  
  
export default class ReactFooter extends React.Component<IReactFooterProps> {  
  constructor(props: IReactFooterProps) {  
    super(props);  
  }  
  
  public render(): JSX.Element {  
    return (  
      <div className={"ms-bgColor-themeDark ms-fontColor-white"}>  
        <CommandBar  
          items={this.getItems()}  
        />       
      </div>  
    );  
  }  
  
  // Data for CommandBar  
  private getItems = () => {  
    return \[  
      {  
        key: 'microsoft',  
        name: 'Microsoft',  
        cacheKey: 'myCacheKey', // changing this key will invalidate this items cache  
        iconProps: {  
          iconName: 'AzureLogo'  
        },  
        href: 'https://www.Microsoft.com'  
      },  
      {  
        key: 'officeUIFabric',  
        name: 'Office UI Fabric',  
        iconProps: {  
          iconName: 'OneDrive'  
        },  
        href: 'https://dev.office.com/fabric',  
        \['data-automation-id'\]: 'uploadButton'  
      }  
    \];  
  }  
}

Open src\\extensions\\reactHeaderFooter\\ReactHeaderFooterApplicationCustomizer.ts

Add below imports.

import \* as React from "react";  
import \* as ReactDOM from "react-dom";  
import ReactFooter, { IReactFooterProps } from "./ReactFooter";

Update \_renderPlaceHolders method to include our React component.

private \_renderPlaceHolders(): void {  
    console.log('Available placeholders: ',  
    this.context.placeholderProvider.placeholderNames.map(name => PlaceholderName\[name\]).join(', '));  
       
    // Handling the bottom placeholder  
    if (!this.\_bottomPlaceholder) {  
      this.\_bottomPlaceholder =  
        this.context.placeholderProvider.tryCreateContent(  
          PlaceholderName.Bottom,  
          { onDispose: this.\_onDispose });  
      
      // The extension should not assume that the expected placeholder is available.  
      if (!this.\_bottomPlaceholder) {  
        console.error('The expected placeholder (Bottom) was not found.');  
        return;  
      }  
  
      const elem: React.ReactElement<IReactFooterProps> = React.createElement(ReactFooter);  
      ReactDOM.render(elem, this.\_bottomPlaceholder.domElement);      
    }  
  }

 

Test the extension

1. Open serve.json under config folder.
2. Update the properties section to include page url to test.

{  
  "$schema": "https://developer.microsoft.com/json-schemas/core-build/serve.schema.json",  
  "port": 4321,  
  "https": true,  
  "serveConfigurations": {  
    "default": {  
      "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",  
      "customActions": {  
        "c633afef-a94d-4970-89ca-30f403612550": {  
          "location": "ClientSideExtension.ApplicationCustomizer",  
          "properties": {  
            "testMessage": "Test message"  
          }  
        }  
      }  
    },  
    "reactHeaderFooter": {  
      "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",  
      "customActions": {  
        "c633afef-a94d-4970-89ca-30f403612550": {  
          "location": "ClientSideExtension.ApplicationCustomizer",  
          "properties": {  
            "testMessage": "Test message"  
          }  
        }  
      }  
    }  
  }  
}

1. On the command prompt, type below command.

gulp serve

1. The SharePoint site will open, click “Load debug scripts”

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-367.png)

1. Observe the footer on the page.

## Summary

Application customizer SharePoint Framework extension helps to extend the predefined placeholders on Modern SharePoint page. React components helps to develop functionality independently by maintaining separation of concerns. These React components can be used as part of application customizer.

This content was originally posted [here](https://www.c-sharpcorner.com/article/using-react-components-in-spfx-extension-application-customizer/).
