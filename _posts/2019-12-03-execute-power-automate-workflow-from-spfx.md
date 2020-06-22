---
title: "Execute Power Automate workflow from SPFx"
date: "2019-12-03"
---

## Overview

In many cases, we want the flow to be triggered by an event, such as an item added or updated. However, few times we would like to trigger the flow programmatically from our SPFx web part. For example, trigger an email from web part on a button click.

In this article, we will explore how we can trigger Power automate (formerly MS Flow) workflow from SPFx web part.

## Build Power Automate Workflow

We will start by building flow to send out an email. Later we will trigger it from SPFx web part using REST API call.

1. Navigate to Power Automate in Office 365.
2. From left menu, click Create.
3. Select “Instant flow”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-454.png)

1. In the modal dialog, specify the flow name (e.g. QuickMailSend).
2. Select the trigger as “When an HTTP request is received”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-455.png)

1. Click Create.
2. The flow should get created as below.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-456.png)

1. We want to compose email with information about email address, subject, and body. Our request body will look like as below:

{  
    "emailaddress":"someone@contoso.com",  
    "emailSubject": "Hello",  
    "emailBody": "Hello from Power Automate flow"  
}

1. Expand the activity “When a HTTP request is received”.
2. Click “Generate from sample”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-457.png)

1. In the pop up, add the above request body JSON schema.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-458.png)

1. Click Done.
2. The request body json schema will be generated.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-459.png)

1. Click “+ New step”.
2. Find an add an action “Send an email”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-460.png)

1. Click on the field in the form, to add JSON parameters (from the schema).

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-461.png)

1. Click “+ New step”.
2. Choose action “Response”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-462.png)

1. Click Save.
2. Note down the HTTP POST URL.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-463.png)

## Test Flow with Soap UI

Download the SoapUI from [https://www.soapui.org/](https://www.soapui.org/)

1. Select Method as Post.
2. Specify the end point as HTTP POST URL.
3. Specify the header as Content-type with value application/json
4. In the Body, specify the JSON.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-464.png)

1. Click Send.
2. The email should be sent out to specified email.

## Develop SPFx Web Part to execute Power Automate Flow

1. Open a command prompt. Create a directory for the SPFx solution.

md spfx-flow-email

1. Navigate to the above created directory.

cd spfx-flow-email

1. Run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-465.png)

1. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in the code editor of your choice.

code .

 

Code the web part

1. Open the web part file at “src\\webparts\\flowEmailer\\FlowEmailerWebPart.ts”.
2. Add below imports.

import { IHttpClientOptions, HttpClientResponse, HttpClient } from '@microsoft/sp-http';

1. Update render() method to include basic controls to input email address, subject, body, and button to send email.

public render(): void {  
  this.domElement.innerHTML = \`  
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
    </div>\`;  
  
    this.domElement.querySelector('button.email-Button').addEventListener('click', () => { this.\_emailButtonClicked(); });  
}

1. Implement button click event to read user inputs and send an email.

private \_emailButtonClicked(): void {    
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

 

Test the Web Part

1. On the command prompt, type “gulp serve -nobrowser”.
2. Open SharePoint site.
3. Navigate to /\_layouts/15/workbench.aspx
4. Locate and add the web part (named FlowEmailer) to page.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-466.png)

1. Provide the email, subject, and body.
2. Verify the email from outlook.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-467.png)

## Summary

In this article, we explored the practical use case of triggering the Power Automate workflow from SPFx web part. The HTTP POST URL exposed by Power Automate flow can be triggered from SPFx web part using HttpClient.

This content was originally posted [here](https://www.c-sharpcorner.com/article/execute-power-automate-workflow-from-spfx/).
