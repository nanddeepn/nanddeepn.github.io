---
title: "SharePoint Framework – Call Azure Function"
date: "2018-11-11"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-11-11-spfx-call-azure-function/10.png
  teaser: media/2018-11-11-spfx-call-azure-function/10.png
tags:
  - "2018"
  - November 2018
last_modified_at: 2018-11-11T00:00:00-00:00
---

## Overview

SharePoint Framework client web parts are used to develop visual components in modern and classic SharePoint. In some scenarios, all the processing cannot happen on SharePoint front and we also need to perform certain processing outside of SharePoint. Azure Functions is one of the most widely used options in these scenarios.

In this article, we will explore how to create a simple Azure Function and consume it in the SharePoint Framework client web part.


## Azure Function

Azure functions are serverless computing. It is an event driven, compute on demand experience. Follow below steps to create an Azure function

1. Open Azure Portal [https://portal.azure.com](https://portal.azure.com).
2. Click **Create Resource**.
3. Under **Compute**, select **Function App**.

    ![](/media/2018-11-11-spfx-call-azure-function/01.png)

4. Fill in the information to create the Function App.

    ![](/media/2018-11-11-spfx-call-azure-function/02.png)

5. Click **Create**.
6. Once the Azure function is ready, click **Functions** > **New function**.

    ![](/media/2018-11-11-spfx-call-azure-function/03.png)

7. Select **HTTP trigger**.

    ![](/media/2018-11-11-spfx-call-azure-function/04.png)

8. Type in function name, select Authorization level.
9. Click **Create**.
10. Click **Run** to test the Azure function.

    ![](/media/2018-11-11-spfx-call-azure-function/05.png)

11. Verify that for provided input, Azure function is returning expected output. Also, the status code is 200 OK.
12. Click **Get function URL**.

    ![](/media/2018-11-11-spfx-call-azure-function/06.png)

13. Copy the function URL.


## Enable CORS on Azure Function

The Azure functions are hosted in MS Azure and they run in different domain than our SharePoint site where our SharePoint Framework (SPFx) web part is hosted. By default cross domain calls are not allowed from SharePoint. To overcome this we will have to enable CORS (Cross-Origin Resource Sharing) in Azure function.

Follow below steps to enable CORS on Azure function:

1. Click **Platform features**.
2. Under **API**, click **CORS**.

    ![](/media/2018-11-11-spfx-call-azure-function/07.png)

3. Specify the Office 365 tenant domain url and SharePoint local workbench url.

    ![](/media/2018-11-11-spfx-call-azure-function/08.png)

4. Click **Save**.


## Develop SharePoint Framework Web Part

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-call-azure-function
    ```

2. Navigate to above created directory.

    ```
    cd spfx-call-azure-function
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-11-11-spfx-call-azure-function/09.png)

    - **Solution Name:** Hit enter to have default name (spfx-call-azure-function in this case) or type in any other name for your solution.
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
        - Selected choice: AzureFunctionWebPart
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Call Azure Function from SPFx
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: No JavaScript Framework

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## Code the web part

1. Open AzureFunctionWebPartWebPart.ts under "\src\webparts\azureFunctionWebPart\" folder.
2. Add below imports.

    ```typescript
    import { HttpClient, SPHttpClient, HttpClientConfiguration, HttpClientResponse, ODataVersion, IHttpClientConfiguration, IHttpClientOptions, ISPHttpClientOptions } from '@microsoft/sp-http';
    ```

3. Implement method _callAzureFunction_ as below:

    ```typescript
    export default class AzureFunctionWebPartWebPart extends BaseClientSideWebPart<IAzureFunctionWebPartWebPartProps> {    
      protected functionUrl: string = "https://spfxcaller.azurewebsites.net/api/HttpTrigger";
      
      protected callAzureFunction(): void {    
        const requestHeaders: Headers = new Headers();    
        requestHeaders.append("Content-type", "text/plain");    
        requestHeaders.append("Cache-Control", "no-cache");    
      
        var siteUrl: string = this.context.pageContext.web.absoluteUrl;    
        var userName: string = (<HTMLInputElement>document.getElementById("txtUserName")).value;    
          console.log(`SiteUrl: '${siteUrl}', UserName: '${userName}'`);    
          const postOptions: IHttpClientOptions = {    
          headers: requestHeaders,    
          body: `{ name: '${userName}' }`    
        };    
          let responseText: string = "";    
        let resultMsg: HTMLElement = document.getElementById("responseContainer");    
          this.context.httpClient.post(this.functionUrl, HttpClient.configurations.v1, postOptions).then((response: HttpClientResponse) => {    
            response.json().then((responseJSON: IData) => {    
              //responseText = JSON.stringify(responseJSON);    
              if (response.ok) {    
                  resultMsg.style.color = "white";    
              } else {    
                  resultMsg.style.color = "red";    
              }    
      
              resultMsg.innerText = responseJSON.name;    
            })    
            .catch ((response: any) => {    
              let errMsg: string = `WARNING - error when calling URL ${this.functionUrl}. Error = ${response.message}`;    
              resultMsg.style.color = "red";    
              console.log(errMsg);    
              resultMsg.innerText = errMsg;    
            });    
        });    
    }
    ```

4. Update render() method as below:

    ```typescript
    public render(): void {    
      this.domElement.innerHTML = `    
        <div class="${ styles.azureFunctionWebPart }">    
          <div class="${ styles.container }">    
            <div class="${ styles.row }">    
              <div class="${ styles.column }">    
                <span class="${ styles.title }">Call Azure Function</span>    
                <p class="${ styles.subTitle }">Customize SharePoint experiences using Web Parts.</p>    
                    
                <div class="${styles.row}">    
                  <span class="ms-font-l ms-fontColor-white ${styles.label}">User name:</span>    
                  <input type="text" id="txtUserName"></input>    
                </div>    
      
                <button id="btnCallAzureFunction" class="${styles.button}">Say Hello!</button>    
                <div id="responseContainer" class="${styles.label}"></div>    
      
              </div>    
            </div>    
          </div>    
        </div>`;    
      
        document.getElementById("btnCallAzureFunction").onclick = this.callAzureFunction.bind(this);    
    }
    ```


## Test the WebPart

1. On the command prompt, type ```gulp serve```.
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Add the webpart to page.

    ![](/media/2018-11-11-spfx-call-azure-function/10.png)


## Summary

Complex or long-running processing can be implemented outside of SharePoint in Azure functions and then can be easily consumed in SharePoint Framework client web parts.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-call-azure-function/).
