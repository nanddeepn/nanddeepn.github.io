---
title: "SharePoint Framework Extensions - Application Customizer Overview"
date: "2018-09-09"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-09-09-spfx-extensions-application-customizer-overview/04.png
  teaser: media/2018-09-09-spfx-extensions-application-customizer-overview/04.png
tags:
  - "2018"
  - September 2018
last_modified_at: 2018-09-09T00:00:00-00:00
---

## Overview

SharePoint Framework (SPFx) Extensions allows to extend the SharePoint user experience. Using SharePoint Framework Extensions, we can customize overall SharePoint user experience involving notification area, list views, and toolbars.

In this article, we will explore the Application Customizer part of SharePoint extensions.


## Brief about Application Customizer

Application customizer provides access to predefined locations on SharePoint page and allows to customize them. In this article, we will add header and footer to SharePoint site using application customizer.

In the classic SharePoint, we used to extend predefined HTML elements or placeholders from master page to display the custom content. In the modern SharePoint as JavaScript cannot be used on page, the application customizer helps to implement these scenarios.


## Page Placeholders

The application customizer helps to get access to below zones on Modern SharePoint page

- Top: Header section of page
- Bottom: Footer section of page


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-extensions-applicationcustomizer
    ```

2. Navigate to above created directory.

    ```
    cd spfx-extensions-applicationcustomizer
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-09-09-spfx-extensions-application-customizer-overview/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-logging in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
        - Selected choice: Same folder
    - **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
        - Selected choice: N (install on each site explicitly)
    - **Type of client-side component to create:** We can choose to create client side webpart or an extension.
        - Selected choice: Extension
    - **Type of client-side extension to create:** We can choose to create Application customizer, Field customizer, or ListView Command Set.
        - Selected choice: Application customizer
    - **Application customizer name:** Hit enter to select the default name or type in any other name.
        - Selected choice: CustomHeaderFooter
    - **Application customizer description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Adds custom header and footer to SharePoint site

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## Solution Structure

The solution structure is similar to client-side web parts with similar configuration options.

![](/media/2018-09-09-spfx-extensions-application-customizer-overview/02.png)

The file CustomHeaderFooterApplicationCustomizer.manifest.json inside the folder "\src\extensions\customHeaderFooter\" defines the extension type and unique identifier for the solution. Please note down the id. We will need it later for debugging purpose.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-extension-manifest.schema.json",  
  
  "id": "f58a0902-c9d8-4cce-bd5e-4da887e21bed",  
  "alias": "CustomHeaderFooterApplicationCustomizer",  
  "componentType": "Extension",  
  "extensionType": "ApplicationCustomizer",  
  
  // The "*" signifies that the version should be taken from the package.json  
  "version": "*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false  
}
```
 

## Implement Application Customizer

Open CustomHeaderFooterApplicationCustomizer.ts under “\\src\\extensions\\customHeaderFooter\\” folder.

To get access to placeholders on the page, use below imports.

```typescript
import {  
  BaseApplicationCustomizer,  
  PlaceholderContent,  
  PlaceholderName  
} from '@microsoft/sp-application-base';
```

Update the interface IAlertApplicationCustomizerProperties to include Top and Bottom properties.

```typescript
export interface ICustomHeaderFooterApplicationCustomizerProperties {  
  Top: string;  
  Bottom: string;  
}
```


Implement OnInit method.

```typescript
public onInit(): Promise<void> {  
    Log.info(LOG_SOURCE, `Initialized ${strings.Title}`);  
  
    // Added to handle possible changes on the existence of placeholders.  
    this.context.placeholderProvider.changedEvent.add(this, this._renderPlaceHolders);  
      
    // Call render method for generating the HTML elements.  
    this._renderPlaceHolders();  
  
    return Promise.resolve();  
}
```


Implement _renderPlaceHolders method.

```typescript
private _renderPlaceHolders(): void {  
  console.log('HelloWorldApplicationCustomizer._renderPlaceHolders()');  
  console.log('Available placeholders: ',  
  this.context.placeholderProvider.placeholderNames.map(name => PlaceholderName[name]).join(', '));  
    
  // Handling the top placeholder  
  if (!this._topPlaceholder) {  
    this._topPlaceholder =  
      this.context.placeholderProvider.tryCreateContent(  
        PlaceholderName.Top,  
        { onDispose: this._onDispose });  
    
    // The extension should not assume that the expected placeholder is available.  
    if (!this._topPlaceholder) {  
      console.error('The expected placeholder (Top) was not found.');  
      return;  
    }  
    
    if (this.properties) {  
      let topString: string = this.properties.Top;  
      if (!topString) {  
        topString = '(Top property was not defined.)';  
      }  
    
      if (this._topPlaceholder.domElement) {  
        this._topPlaceholder.domElement.innerHTML = `  
          <div class="${styles.app}">  
            <div class="ms-bgColor-themeDark ms-fontColor-white ${styles.top}">  
              <i class="ms-Icon ms-Icon--Info" aria-hidden="true"></i> ${escape(topString)}  
            </div>  
          </div>`;  
      }  
    }  
  }  
    
  // Handling the bottom placeholder  
  if (!this._bottomPlaceholder) {  
    this._bottomPlaceholder =  
      this.context.placeholderProvider.tryCreateContent(  
        PlaceholderName.Bottom,  
        { onDispose: this._onDispose });  
    
    // The extension should not assume that the expected placeholder is available.  
    if (!this._bottomPlaceholder) {  
      console.error('The expected placeholder (Bottom) was not found.');  
      return;  
    }  
    
    if (this.properties) {  
      let bottomString: string = this.properties.Bottom;  
      if (!bottomString) {  
        bottomString = '(Bottom property was not defined.)';  
      }  
    
      if (this._bottomPlaceholder.domElement) {  
        this._bottomPlaceholder.domElement.innerHTML = `  
          <div class="${styles.app}">  
            <div class="ms-bgColor-themeDark ms-fontColor-white ${styles.bottom}">  
              <i class="ms-Icon ms-Icon--Info" aria-hidden="true"></i> ${escape(bottomString)}  
            </div>  
          </div>`;  
      }  
    }  
  }  
}
```

Use this.context.placeholderProvider.tryCreateContent to get access the placeholder, without assuming that the placeholder will exists

**Custom Styles**

Add a file CustomHeaderFooterApplicationCustomizer.module.scss under "\src\extensions\customHeaderFooter\" folder.

```css
.app {  
  .top {  
    height:60px;  
    text-align:center;  
    line-height:2.5;  
    font-weight:bold;  
    display: flex;  
    align-items: center;  
    justify-content: center;  
  }  
  
  .bottom {  
    height:40px;  
    text-align:center;  
    line-height:2.5;  
    font-weight:bold;  
    display: flex;  
    align-items: center;  
    justify-content: center;  
  }  
}
```

Include the styles in extension CustomHeaderFooterApplicationCustomizer.ts

```typescript
import styles from './CustomHeaderFooterApplicationCustomizer.module.scss';
```


## Test the extension

1. Open serve.json under config folder.
2. Update the properties section to include Top and Bottom messages.

    ```json
    {  
      "$schema": "https://developer.microsoft.com/json-schemas/core-build/serve.schema.json",  
      "port": 4321,  
      "https": true,  
      "serveConfigurations": {  
        "default": {  
          "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",  
          "customActions": {  
            "f58a0902-c9d8-4cce-bd5e-4da887e21bed": {  
              "location": "ClientSideExtension.ApplicationCustomizer",  
              "properties": {  
                "Top": "Header of the page",  
                "Bottom": "Footer of the page"  
              }  
            }  
          }  
        },  
        "customHeaderFooter": {  
          "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",  
          "customActions": {  
            "f58a0902-c9d8-4cce-bd5e-4da887e21bed": {  
              "location": "ClientSideExtension.ApplicationCustomizer",  
              "properties": {  
                "Top": "Header of the page",  
                "Bottom": "Footer of the page"  
              }  
            }  
          }  
        }  
      }  
    }
    ```

3. On the command prompt, type below command.

    ```
    gulp serve
    ```

4. The SharePoint site will open, click **Load debug scripts**.

    ![](/media/2018-09-09-spfx-extensions-application-customizer-overview/03.png)

5. Observe the header and footer on the page.

    ![](/media/2018-09-09-spfx-extensions-application-customizer-overview/04.png)


## Summary

Application customizer SharePoint Framework extension helps to extend the predefined placeholders on Modern SharePoint page. These extensions can be deployed tenant wide.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-extensions-application-customizer-overview/).
