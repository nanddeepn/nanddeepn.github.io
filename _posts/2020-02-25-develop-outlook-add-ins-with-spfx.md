---
title: "Develop Outlook Add-ins with SharePoint Framework"
date: "2020-02-25"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2020-02-25-develop-outlook-add-ins-with-spfx/10.png
  teaser: media/2020-02-25-develop-outlook-add-ins-with-spfx/10.png
tags:
  - "2020"
  - February 2020
last_modified_at: 2020-02-25T00:00:00-00:00
---

## Overview

SharePoint Framework is spreading its wings and now ready to go beyond of SharePoint and develop solutions targeting Office 365 platform. SPFx already supports to host the SPFx web part as tabs in MS Teams and create personal tabs in MS Team. Now, SPFx v1.10 allows to develop Outlook Web App add-ins hosted on SharePoint.

In this article, we will explore how can develop Outlook Add-ins with SharePoint Framework.


## Develop SPFx Web Part for Outlook Add-ins

Before we dive in to implementing SPFx web part, please note that this feature is available in beta and at the moment, only supported within the context of the Outlook Web Access. The GA should support Office desktop too.

As this feature is in preview, we have to scaffold the project with Yeoman using --plusbeta switch.

Follow the below steps to develop the SPFx solution.

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-outlook-addin
    ```

2. Navigate to the directory.

    ```
    cd spfx-outlook-addin
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint --plusbeta
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/01.png)

    When prompted:

    - Accept the default **spfx-outlook-addin** as your solution name, and then select Enter.
    - Select **SharePoint Online only (latest)**, and then select Enter.
    - Select **Use the current folder** as the location for the files.
    - Select **y** to ensure that your web part is automatically deployed tenant-wide when it's added to the tenant App Catalog.
    - Select **N** on the question if solution contains unique permissions.
    - Select **WebPart** as the client-side component type to be created.
    - Enter **SPFxOutlookAddIn** for the web part name, and then select Enter.
    - Enter the description of the web part.
    - Select **No JavaScript framework** to develop the web part.

5. Yeoman generator will perform the scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.

6. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. In the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```

## Use Office JavaScript SDK (Office.js)

Include correct types by installing below npm package. The type declarations should go in devDependencies.

```
npm install @types/office-js --save-dev
```

The below property will help to identify if the web part is running under Office context.

```
this.context.sdks.office
```


## Support for Office AddIn

The SPFx solution contains an additional folder "officeAddin". This supports one time configuration option when the add-in is initially viewed. This option is controlled from URL of the solution.

![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/02.png)

By default, configuration option is enabled. If your add-in does not have any initial configuration options, you can remove the isConfigureMode query parameter.


## Code the SPFx Outlook AddIn

We will update the code to work as Outlook AddIn.

1. Open the web part file at `src\webparts\spFxOutlookAddIn\SpFxOutlookAddInWebPart.ts`.
2. Update the render method to check the office context.

```typescript
public render(): void {  
  let title: string = "";  
  let subTitle: string = "";  
  let contextInfo: string = "";  
  
  if (this.context.sdks.office) {  
    // Office context  
    title = "Welcome to Office!";  
    subTitle = "Extending Office with SPFx.";  
    contextInfo = "Email: " + this.context.sdks.office.context.mailbox.userProfile.emailAddress;  
  }  
  else {  
    // SharePoint context  
    title = "Welcome to SharePoint!";  
    subTitle = "Customize SharePoint experiences using Web Parts.";  
    contextInfo = "SharePoint site: " + this.context.pageContext.web.title;  
  }  
  
  this.domElement.innerHTML = `  
    <div class="${ styles.spFxOutlookAddIn}">  
      <div class="${ styles.container}">  
        <div class="${ styles.row}">  
          <div class="${ styles.column}">  
            <span class="${ styles.title}">${title}</span>  
            <p class="${ styles.subTitle}">${subTitle}</p>  
            <p class="${ styles.description}">${contextInfo}</p>  
          </div>  
        </div>  
      </div>  
    </div>`;  
}
```


## Deploy the Outlook Add-in

The deployment process involves 2 steps.

**Step 1: Deploy solution to SharePoint App Catalog**

1. Package and deploy the solution by running below commands:

```
gulp bundle --ship
gulp package-solution --ship
```

2. Deploy the solution to SharePoint app catalog.

![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/03.png)

**Step 2: Add Add-in to Outlook Web Access**

Since this feature is in preview, it can be only added to Outlook web access.

1. Open Office 365 Outlook Web access.
2. Open any existing email.
3. Click "…" and Select "Get Add-ins".

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/04.png)

4. From left menu, click "My add-ins".
5. Under "Custom add-ins", click "Add from file…"

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/05.png)

6. Upload the manifest xml file from SPFx project solution under the officeAddin folder.
7. Click **Install**.

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/06.png)

8. The custom add-in will appear.

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/07.png)

9. Close the add-in window.
10. From an email, click "…"

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/08.png)

11. The configuration option is available during initial view of the add-in.

    ![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/09.png)

12. After the initial configuration options set, the SPFx Outlook AddIn can be seen as below:

![](/media/2020-02-25-develop-outlook-add-ins-with-spfx/10.png)


## Summary

SPFx v1.10 allows to develop Outlook Web App add-ins hosted on SharePoint. This feature is available in beta and at the moment, only supported within the context of the Outlook Web Access.

This content was originally posted [here](https://www.c-sharpcorner.com/article/develop-outlook-add-ins-with-sharepoint-framework/).
