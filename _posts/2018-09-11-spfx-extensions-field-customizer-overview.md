---
title: "SharePoint Framework Extensions - Field Customizer Overview"
date: "2018-09-11"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-09-11-spfx-extensions-field-customizer-overview/10.png
  teaser: media/2018-09-11-spfx-extensions-field-customizer-overview/10.png
tags:
  - "2018"
  - September 2018
last_modified_at: 2018-09-10T00:00:00-00:00
---

## Overview

SharePoint Framework (SPFx) Extensions are client side components which allows extending the SharePoint user experience.

In this article, we will explore the Field Customizer part of SharePoint extensions.


## Brief about Field Customizer

Field customizer allows to modify views for the field in a list view. It can be used to override the field representation in the list. Field customizer can be used with site columns or directly to the field inside a list.

In the classic SharePoint, we used to modify the representation of SharePoint list fields using JSLink property of the web part. In the modern SharePoint as JavaScript cannot be used on page, the field customizer helps to implement these scenarios.


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-extensions-fieldcustomize
    ```

2. Navigate to above created directory.

    ```
    cd spfx-extensions-fieldcustomize
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-extensions-fieldcustomizer in this case) or type in any other name for your solution.
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
        - Selected choice: Field customizer
    - **Field customizer name:** Hit enter to select the default name or type in any other name.
        - Selected choice: PercentFieldCustomizer
    - **Field customizer description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Displays field in percentage
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React)
        - Selected choice - No JavaScript Framework

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

![](/media/2018-09-11-spfx-extensions-field-customizer-overview/02.png)

The file PercentFieldCustomizerFieldCustomizer.manifest.json inside the folder "\src\extensions\percentFieldCustomizer\" defines the extension type and unique identifier for the solution. Please note down the id. We will need it later for debugging purpose.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-extension-manifest.schema.json",  
  
  "id": "8db272d2-a083-48e7-b6da-dbf14fc98bbf",  
  "alias": "PercentFieldCustomizerFieldCustomizer",  
  "componentType": "Extension",  
  "extensionType": "FieldCustomizer",  
  
  // The "*" signifies that the version should be taken from the package.json  
  "version": "*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false  
}
```


## Implement Field Customizer

Open PercentFieldCustomizerFieldCustomizer.ts under "\src\extensions\percentFieldCustomizer\" folder. The class has below important methods:

- **onInit()** event occurs before the page DOM is ready. It returns promise to perform asynchronous operations. onRenderCell() is not invoked until this promise is resolved.
- **onRenderCell()** event occurs when each cell is rendered. It provides event.domElement to customize the representation of field.
- **onDisposeCell()** can be used to free up any used resources that were allocated during rendering of the field to avoid any resource leak.
 

## Debug Field Customizer

The SharePoint local workbench now cannot be used to test the field customizer as we will need an actual SharePoint site to create needed list and fields.

1. Open SharePoint site.
2. From site contents create new list named **Projects**.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/03.png)

3. Click **Add column** to add a number field.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/04.png)

4. Name it as **Completion**.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/05.png)

5. Click **Save**.
6. Add some test data to the list.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/06.png)


## Update the Solution for Field changes

1. Open serve.json file under config folder. Below is the default content.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/07.png)

2. Update InternalFieldName attribute with the name of our field i.e. Completion.

    ```json
    {    
      "$schema": "https://developer.microsoft.com/json-schemas/core-build/serve.schema.json",    
      "port": 4321,    
      "https": true,    
      "serveConfigurations": {    
        "default": {    
          "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",    
          "fieldCustomizers": {    
            "Completion": {    
              "id": "8db272d2-a083-48e7-b6da-dbf14fc98bbf",    
              "properties": {    
                "sampleText": "Value"    
              }    
            }    
          }    
        },    
        "percentFieldCustomizer": {    
          "pageUrl": "https://contoso.sharepoint.com/sites/mySite/SitePages/myPage.aspx",    
          "fieldCustomizers": {    
            "Completion": {    
              "id": "8db272d2-a083-48e7-b6da-dbf14fc98bbf",    
              "properties": {    
                "sampleText": "Value"    
              }    
            }    
          }    
        }    
      }    
    }
    ```

3. On the command prompt, type below command to run the extension.

    ```
    gulp serve
    ```

4. Accept loading of debug manifests by clicking **Load debug scripts**.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/08.png)

5. The list view should display the formatted Completion column as below:

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/09.png)


## Apply Field Customization

1. Navigate back to the solution
2. Open PercentFieldCustomizerFieldCustomizer.module.scss file from "\src\extensions\percentFieldCustomizer\" folder and update the css.

    ```css
    .PercentFieldCustomizer {    
      .cell {    
        background-color: "[theme:themePrimary, default:#e5e5e5]";    
        display: 'inline-block';    
      }    
      .full {    
        background-color: #e6e6e6;    
        width: 100px;    
      }    
    }
    ```

3. Open PercentFieldCustomizerFieldCustomizer.ts file from "\src\extensions\percentFieldCustomizer\” folder.
4. Update onRenderCell method.

    ```typescript
    public onRenderCell(event: IFieldCustomizerCellEventParameters): void {    
      // Use this method to perform your custom cell rendering.    
      event.domElement.classList.add(styles.cell);    
      event.domElement.innerHTML = `    
              <div class='${styles.PercentFieldCustomizer}'>    
                <div class='${styles.full}'>    
                  <div style='width: ${event.fieldValue}px; background:#0094ff; color:#ffffff'>    
                    ${event.fieldValue}    
                  </div>    
                </div>
              </div>`;    
    }
    ```

5. Make sure the gulp serve is running. Refresh the SharePoint list – Projects.

    ![](/media/2018-09-11-spfx-extensions-field-customizer-overview/10.png)

    The Completion field is now customized with the progress completion using field customizer.


## Summary

Field customizer SharePoint Framework extension helps to modify the representation of the field in SharePoint list. This is an alternative to JS Link in the modern SharePoint sites.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-extensions-field-customizer-overview/).
