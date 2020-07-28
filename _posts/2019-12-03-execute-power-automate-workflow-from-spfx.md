---
title: "Execute Power Automate workflow from SPFx"
date: "2019-12-03"
share: true
categories:
  - MS Graph
  - Power Automate
  - Power Platform
header:
  image: media/2019-12-03-execute-power-automate-workflow-from-spfx/13.png
  teaser: media/2019-12-03-execute-power-automate-workflow-from-spfx/13.png
tags:
  - "2019"
  - December 2019
last_modified_at: 2020-12-03T00:00:00-00:00
---

## Overview

In many cases, we want the flow to be triggered by an event, such as an item added or updated. However, few times we would like to trigger the flow programmatically from our SPFx web part. For example, trigger an email from web part on a button click.

In this article, we will explore how we can trigger Power automate (formerly MS Flow) workflow from SPFx web part.


## Build Power Automate Workflow

We will start by building flow to send out an email. Later we will trigger it from SPFx web part using REST API call.

1. Navigate to Power Automate in Office 365.
2. From left menu, click **Create**.
3. Select **Instant flow**.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/01.png)

4. In the modal dialog, specify the flow name (e.g. QuickMailSend).
5. Select the trigger as **When an HTTP request is received**.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/02.png)

6. Click **Create**.
7. The flow should get created as below.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/03.png)

8. We want to compose email with information about email address, subject, and body. Our request body will look like as below:

    ```json
    {  
        "emailaddress":"someone@contoso.com",  
        "emailSubject": "Hello",  
        "emailBody": "Hello from Power Automate flow"  
    }
    ```

9. Expand the activity **When a HTTP request is received**.
10. Click **Generate from sample**.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/04.png)

11. In the pop up, add the above request body JSON schema.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/05.png)

12. Click **Done**.
13. The request body json schema will be generated.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/06.png)

14. Click **+ New step**.
15. Find and add an action **Send an email**.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/07.png)

16. Click on the field in the form, to add JSON parameters (from the schema).

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/08.png)

17. Click **+ New step**.
18. Choose action **Response**.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/09.png)

19. Click **Save**.
20. Note down the HTTP POST URL.

![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/10.png)


## Test Flow with Soap UI

Download the SoapUI from [https://www.soapui.org/](https://www.soapui.org/)

1. Select Method as **Post**.
2. Specify the end point as **HTTP POST URL**.
3. Specify the header as **Content-type** with value **application/json**.
4. In the Body, specify the JSON.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/11.png)

5. Click **Send**.
6. The email should be sent out to specified email.


## Develop SPFx Web Part to execute Power Automate Flow

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-flow-email
    ```

2. Navigate to the above created directory.

    ```
    cd spfx-flow-email
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/12.png)

5. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

6. On the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```
 

## Code the web part

1. Open the web part file at "src\webparts\flowEmailer\FlowEmailerWebPart.ts".
2. Add below imports.

    ```typescript
    import { IHttpClientOptions, HttpClientResponse, HttpClient } from '@microsoft/sp-http';
    ```

3. Update render() method to include basic controls to input email address, subject, body, and button to send email.

    ```typescript
    public render(): void {  
      this.domElement.innerHTML = `  
        <div class="${ styles.flowEmailer }">  
          <div class="${ styles.container }">  
            <div class="${ styles.row }">  
              <div class="${ styles.column }">  
                <span class="${ styles.title }">Welcome to SharePoint!</span>  
                <p class="${ styles.subTitle }">Customize SharePoint experiences using Web Parts.</p>  
                <p class="${ styles.description }">${escape(this.properties.description)}</p>  
                  
                <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">    
                  <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">    
                    <span class="${styles.fixedTitle}">Email</span>                      
                    <input type='textbox' name='txtEmail' id='txtEmail' class="form-control" value="" placeholder="" >  
                  </div>    
                </div>   
                  
                <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">    
                  <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">    
                    <span class="${styles.fixedTitle}">Subject</span>                      
                    <input type='textbox' name='txtSubject' id='txtSubject' class="form-control" value="" placeholder="" >  
                  </div>    
                </div>  
      
                <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">    
                  <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">    
                    <span class="${styles.fixedTitle}">Body</span>                
                    <textarea rows="4" cols="50" name='txtBody' id='txtBody' class="form-control"></textarea>  
                  </div>    
                </div>  
      
                <div class="ms-Grid-row ms-bgColor-themeDark ms-fontColor-white ${styles.row}">    
                  <div class="ms-Grid-col ms-u-lg10 ms-u-xl8 ms-u-xlPush2 ms-u-lgPush1">    
                    <button class="${styles.button} email-Button">    
                      <span class="${styles.label}">Send email</span>    
                    </button>  
                  </div>    
                </div>  
      
              </div>  
            </div>  
          </div>  
        </div>`;  
      
        this.domElement.querySelector('button.email-Button').addEventListener('click', () => { this._emailButtonClicked(); });  
    }
    ```

4. Implement button click event to read user inputs and send an email.

```typescript
private _emailButtonClicked(): void {    
  let emailAddress: string = (<HTMLInputElement>document.getElementById("txtEmail")).value;  
  let emailSubject: string = (<HTMLInputElement>document.getElementById("txtSubject")).value;  
  let emailBody: string = (<HTMLInputElement>document.getElementById("txtBody")).value;  
  
  this.sendEmail(emailAddress, emailSubject, emailBody);  
}   
  
private sendEmail(emailAddress: string, emailSubject: string, emailBody: string): Promise<HttpClientResponse> {  
  const postURL = "<Specify Power Automate Flow URL>";  
  
  const body: string = JSON.stringify({  
    'emailaddress': emailAddress,  
    'emailSubject': emailSubject,  
    'emailBody': emailBody,  
  });  
  
  const requestHeaders: Headers = new Headers();  
  requestHeaders.append('Content-type', 'application/json');  
  
  const httpClientOptions: IHttpClientOptions = {  
    body: body,  
    headers: requestHeaders  
  };  
  
  console.log("Sending Email");  
  
  return this.context.httpClient.post(  
    postURL,  
    HttpClient.configurations.v1,  
    httpClientOptions)  
    .then((response: HttpClientResponse): Promise<HttpClientResponse> => {  
      console.log("Email sent.");  
      return response.json();  
    });  
}
```
 

## Test the Web Part

1. On the command prompt, type **gulp serve -nobrowser**.
2. Open SharePoint site.
3. Navigate to /_layouts/15/workbench.aspx
4. Locate and add the web part (named FlowEmailer) to page.

    ![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/13.png)

5. Provide the email, subject, and body.
6. Verify the email from outlook.

![](/media/2019-12-03-execute-power-automate-workflow-from-spfx/14.png)


## Summary

In this article, we explored the practical use case of triggering the Power Automate workflow from SPFx web part. The HTTP POST URL exposed by Power Automate flow can be triggered from SPFx web part using HttpClient.

This content was originally posted [here](https://www.c-sharpcorner.com/article/execute-power-automate-workflow-from-spfx/).
