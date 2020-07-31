---
title: "JSON to Code for SPFx Solutions"
date: "2019-07-09"
share: true
categories:
  - SharePoint
  - SharePoint Framework
  - Office 365 CLI
header:
  image: media/2019-07-09-json-to-code-for-spfx-solutions/01.png
  teaser: media/2019-07-09-json-to-code-for-spfx-solutions/01.png
tags:
  - "2019"
  - July 2019
last_modified_at: 2019-07-09T00:00:00-00:00
---

## Introduction

SharePoint Framework web part often involves interaction with the SharePoint list, data within Office 365, or any external Rest API. Most of these can be configured to return a response in JSON format. Dealing with JSON data is very frequent with SPFx development.

In this article, we will explore how we can interact with the JSON data and make use of extensions or plug-ins in visual studio code to generate code from JSON.


## JSON Response in SPFx

Below are a few examples of how data being returned in JSON format in various scenarios.

**SharePoint List Item Processing**

The below code shows the JSON response being returned during SharePoint list item retrieval using REST API.

```typescript
this._spHttpClient.get(`${this._currentWebUrl}/_api/web/lists/getbytitle('${listName}')/items?$select=FileRef/FileRef&$filter=FSObjType eq 0`,  
    SPHttpClient.configurations.v1,  
    {  
        headers: {  
            'Accept': 'application/json;odata=nometadata',  
            'odata-version': ''  
        }  
    })  
    .then((response: SPHttpClientResponse): Promise<{ value: ICarouselImage[] }> => {  
        return response.json();  
    })  
    .then((response: { value: ICarouselImage[] }): void => {  
        resolve(response.value);  
    }, (error: any): void => {  
        reject(error);  
    });
```


**MS Graph Processing**

The below code shows the JSON response being returned during the MS Graph operation.

```typescript
graphClient  
  .api("users")  
  .version("v1.0")  
  .select("displayName,mail,userPrincipalName")  
  .get((err, res) => {    
  
    if (err) {  
      console.error(err);  
      return;  
    }  
  
    // Prepare the output array  
    var users: Array<IUserItem> = new Array<IUserItem>();  
  
    // Map the JSON response to the output array  
    res.value.map((item: any) => {  
      users.push( {   
        displayName: item.displayName,  
        mail: item.mail,  
        userPrincipalName: item.userPrincipalName,  
      });  
    });  
  
    // Update the component state accordingly to the result  
    this.setState(  
      {  
        users: users,  
      }  
    );  
  });
```


**Other Examples**

Calling REST API or MS Azure function from SPFx returns the response in the JSON format.


## Process JSON in SPFx Code

In SPFx solution, we define an interface to represent the data returned from JSON and use it in our code. To define the interface, firstly we have to analyze the JSON data and its format. If the JSON response is complicated, creating the interface by hand to represent it is a difficult job. Also, the interface should be revised if there is a change in the returned JSON (e.g. addition of new field, removal of an existing field, rename of fields, reposition of fields, etc.)


### Paste JSON as Code – VSO Extension

An extension **Paste JSON as Code** for Visual Studio Code is available in the marketplace for free to help convert JSON to any language. This extension can prove handy to convert JSON to any language including TypeScript, Python, Go, Ruby, C#, Java, Swift, Rust, Kotlin, C++, Flow, Objective-C, JavaScript, Elm, and JSON Schema.

Follow the below steps to install the extension to VSO.

1. Navigate to [https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/01.png)

2. Click **Install**.
3. VS Code will open.

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/02.png)

4. Click Install to install the extension.


## Use the extension

Follow the below steps to use the extension.

1. Create a new file in VSO by **File** > **New File**.
2. Paste JSON in the created file.
3. In the VSO, click **View** > **Command Palette**.

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/03.png)

4. Select **Open quicktype for JSON**.
5. The corresponding TypeScript code will be generated.

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/04.png)

6. As the JSON is modified in the left section, the corresponding code will be auto generated in the right section.


## Generate code in other languages

Follow the below steps to generate code in other languages

1. Copy the JSON code to clipboard (CTRL + C).
2. In the VSO, click **View** > **Command Palette**.
3. Select **Paste JSON as Code**.

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/05.png)

4. Select the target language.

    ![](/media/2019-07-09-json-to-code-for-spfx-solutions/06.png)

5. The types will be generated from JSON in the selected language.


## Summary

SPFx development involves dealing with JSON response, which needs to be mapped to the equivalent interface in TypeScript to deal with the returned JSON response. The VSO extension – **Paste JSON as Code** is a handy extension for every SharePoint developer to generate types from JSON, which saves a lot of manual efforts.

This content was originally posted [here](https://www.c-sharpcorner.com/article/upgrade-spfx-solutions-with-office-365-cli/).
