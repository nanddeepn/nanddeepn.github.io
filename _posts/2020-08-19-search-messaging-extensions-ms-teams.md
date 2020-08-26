---
title: "Implementing Search Messaging Extensions for MS Teams with Yo Teams"
date: "2020-08-19"
share: true
header:
  image: media/2020-08-19-search-messaging-extensions-ms-teams/15.gif
  teaser: media/2020-08-19-search-messaging-extensions-ms-teams/15.gif
categories:
  - MS Teams
tags:
  - "2020"
  - August 2020
last_modified_at: 2020-08-19T00:00:00-00:00
---

## Overview

Messaging extensions enable users to execute search queries or trigger actions in external systems. The results of these actions are sent from your custom web service to the Microsoft Teams client as embedded web pages or richly formatted cards.

In this article, we will create search messaging extensions in a custom Microsoft Teams app using Yo Teams. The search message extension searches the Northwind database.


## Messaging extension overview

Messaging extensions allow applications to extend the menus and the command box in MS Teams.

1. Command box which works with both search and action messaging extensions
2. Users can take action on messages.
3. Users can type messages from compose box.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/01.png)

    With any of above options, users can take action and send them to your application.

    **Search messaging extension** allows users to search external system and insert the results of that search into a message in the form of a card.


## Bots and Messaging extensions

MS Team's messaging extensions is based on the bot infrastructure. When a user clicks on a messaging extension, MS Teams sends an invoke activity across Azure bot services channel. Eventually the invoke activity ends up at your bot which is a web service.

![](/media/2020-08-19-search-messaging-extensions-ms-teams/02.png)


## Prerequisites

Install below to get started:

- [Node.js](https://nodejs.org/) - v10.* (or higher)
- NPM (installed with Node.js) - v6.* (or higher)
- [Gulp](https://gulpjs.com/) - v4.* (or higher)
- [Yeoman](https://yeoman.io/) - v3.* (or higher)
- [Yeoman Generator for Microsoft Teams](https://github.com/OfficeDev/generator-teams) - v2.13.0 (or higher)
- [Visual Studio Code](https://code.visualstudio.com/)

Install Yeoman, Gulp global command-line interface, and Typescript compiler globally using NPM.

```
npm install yo gulp-cli typescript --global
```

Install generator-teams globally using NPM.

```
npm install generator-teams --global
```

To install preview versions of the generator, run below command.

```
npm install generator-teams@preview --global
```

## Register Bot in Azure

Firstly, we need to register Bot in Azure to get the App Id and secret and configure the endpoint URL.

1. Open MS Azure Portal ([https://portal.azure.com](https://portal.azure.com/))
2. Click **Create a resource**.
3. Search **Bot**.
4. Select **Bot Channels Registration**.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/03.png)

5. Click **Create**.
6. Fill in the details to create Bot channels registration.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/04.png)

7. Click **Create**.
8. The new Bot channels registration is ready.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/05.png)


**Get the App Id and secret**

1. From the left menu, click **Settings**.
2. Add **Messaging endpoint** as ngrok URL.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/06.png)

3. Click **Microsoft App ID (Manage)**.
4. Under **Certificates &amp; secrets** , create **New client secret**.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/07.png)

5. Note down the values for future references.


**Configure MS Teams Channel**

1. From the left menu, click **Channels**.
2. Click **Configure Microsoft Teams Channel**.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/08.png)

3. Follow the instructions to configure the MS Teams channel.


## Implement Search Messaging extension with Yo Teams

We will generate a solution using Yo Teams.

1. On the command prompt, type below command.

    ```
    yo teams
    ```

2. Follow the wizard to set up the solution.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/09.png)

3. On the command prompt, type code . to open the solution in visual studio code.
4. In the .env file, configure application ID and client secret from Bot channels registration.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/10.png)


## Northwind Service

To implement the scenario, we will use the Northwind service at [https://services.odata.org/Northwind/Northwind.svc/Customers?$format=json](https://services.odata.org/Northwind/Northwind.svc/Customers?%24format=json)

## Implement Model

We will implement models as below.

1. Under "src\app\" create folder "model"
2. Add Customer model as below (ICustomer.ts)

    ```typescript
    // Response from Northwind customer query
    export interface ICustomerResponse {
        value: ICustomer[];
    }

    export interface ICustomer {
        CustomerID: string;
        CompanyName: string;
        ContactName: string | null;
        ContactTitle: string | null;
        Address: string | null;
        City: string | null;
        Region: string | null;
        PostalCode: string | null;
        Country: string | null;
        Phone: string | null;
        Fax: string | null;
    }
    ```

## Implement Services

Implement Northwind service as below:

1. Under "src\app", create folder "services"
2. Add "NorthwindService" under it.
3. Add below files.

INorthwindService.ts

```typescript
import { ICustomer } from "../../model/ICustomer";

export interface INorthwindService {
    getCustomersByName(nameQuery: string): Promise<ICustomer[]>
}
```


NorthwindService.ts

```typescript
import { ICustomerResponse, ICustomer } from "../../model/ICustomer";
import { INorthwindService } from "./INorthwindService";

export class NorthwindService implements INorthwindService {

    async getCustomersByName(nameQuery: string): Promise<ICustomer[]> {
        const serviceUrl = nameQuery ?
            // Filter by name
            `https://services.odata.org/Northwind/Northwind.svc/Customers?$format=json&$top=20&$filter=startswith(CompanyName, '${nameQuery}')` :
            // Query was blank, return all records
            `https://services.odata.org/Northwind/Northwind.svc/Customers?$format=json&$top=20`;

        let result: ICustomer[] = [];
        
        const response = await fetch(serviceUrl, {
            method: 'GET',
            headers: { "accept": "application/json" }
        });
        
        if (response.ok) {
            const responseJson: ICustomerResponse = await response.json();
            result = responseJson.value;
        }

        return result;
    }
}
```


NorthwindServiceMock.ts

```typescript
import { ICustomer } from "../../model/ICustomer";
import { INorthwindService } from "./INorthwindService";

export class NorthwindServiceMock implements INorthwindService {
    getCustomersByName(nameQuery: string): Promise<ICustomer[]> {
        const mockData: ICustomer[] =
        [
            {
                "CustomerID": "GALED",
                "CompanyName": "Galería del gastrónomo",
                "ContactName": "Eduardo Saavedra",
                "ContactTitle": "Marketing Manager",
                "Address": "Rambla de Cataluña, 23",
                "City": "Barcelona",
                "Region": null,
                "PostalCode": "08022",
                "Country": "Spain",
                "Phone": "(93) 203 4560",
                "Fax": "(93) 203 4561"
            },
            {
                "CustomerID": "GODOS",
                "CompanyName": "Godos Cocina Típica",
                "ContactName": "José Pedro Freyre",
                "ContactTitle": "Sales Manager",
                "Address": "C/ Romero, 33",
                "City": "Sevilla",
                "Region": null,
                "PostalCode": "41101",
                "Country": "Spain",
                "Phone": "(95) 555 82 82",
                "Fax": null
            },
            {
                "CustomerID": "GOURL",
                "CompanyName": "Gourmet Lanchonetes",
                "ContactName": "André Fonseca",
                "ContactTitle": "Sales Associate",
                "Address": "Av. Brasil, 442",
                "City": "Campinas",
                "Region": "SP",
                "PostalCode": "04876-786",
                "Country": "Brazil",
                "Phone": "(11) 555-9482",
                "Fax": null
            },
            {
                "CustomerID": "GREAL",
                "CompanyName": "Great Lakes Food Market",
                "ContactName": "Howard Snyder",
                "ContactTitle": "Marketing Manager",
                "Address": "2732 Baker Blvd.",
                "City": "Eugene",
                "Region": "OR",
                "PostalCode": "97403",
                "Country": "USA",
                "Phone": "(503) 555-7555",
                "Fax": null
            },
            {
                "CustomerID": "GROSR",
                "CompanyName": "GROSELLA-Restaurante",
                "ContactName": "Manuel Pereira",
                "ContactTitle": "Owner",
                "Address": "5ª Ave. Los Palos Grandes",
                "City": "Caracas",
                "Region": "DF",
                "PostalCode": "1081",
                "Country": "Venezuela",
                "Phone": "(2) 283-2951",
                "Fax": "(2) 283-3397"
            }
        ];

        return new Promise<ICustomer[]>((resolve) => {
            resolve (mockData);
        });
    }
}
```


Add below file "src\app\services\serviceFactory.ts"

```typescript
import { INorthwindService } from './NorthwindService/INorthwindService';
import { NorthwindServiceMock } from './NorthwindService/NorthwindServiceMock';
import { NorthwindService } from './NorthwindService/NorthwindService';

export class ServiceFactory {
    public static getNorthwindService(): INorthwindService {
        if (process.env["ENVIRONMENT"] === "mock") {
            return new NorthwindServiceMock();
        } else {
            return new NorthwindService();
        }
    }
}
```


## Implement Cards

We will implement cards to return in response to search messaging extension query.

1. Under "src\app", create folder "cards".
2. Add below files.

customerPreviewCard.ts

```typescript
import { ICustomer } from '../model/ICustomer';

interface IThumbnailCard {
    contentType: string;
    content: {
        title: string;
        text: string;
        images: {
            url: string;
        }[];
    };
}

export class CustomerPreviewCard {
    public static getCard(customer: ICustomer): IThumbnailCard {
        const address = `${customer.Address}, ${customer.City}, ${customer.Region ? customer.Region :""} ${customer.Country}`;

        return {
            contentType: "application/vnd.microsoft.card.thumbnail",
            content: {
                title: customer.CompanyName,
                text: address,
                images: [
                    {
                        url: `https://${process.env.HOSTNAME}/assets/northwindLogoSmall.png`
                    }
                ]
            }
        };
    }
}
```


customerResultCard.ts

```typescript
import { ICustomer } from '../model/ICustomer';
import { Attachment, CardFactory } from "botbuilder";

import { ServiceFactory } from "../services/serviceFactory";

export class CustomerResultCard {

    public static async getCard(customer: ICustomer): Promise<Attachment> {

        // Get card text
        const address = customer.Address ? customer.Address : "";
        const city = `${customer.City}, ${customer.Region ? customer.Region : ""} ${customer.Country}`;
        const details1 = customer.ContactName && customer.ContactTitle ?
            `Contact: ${customer.ContactName}, ${customer.ContactTitle}` : "";
        const details2 = customer.Phone ? `Phone: ${customer.Phone}` : "";
        const details3 = customer.Fax ? `Fax: ${customer.Fax}` : "";

        // Get map
        let mapUrl = `https://${process.env.HOSTNAME}/assets/northwindLogoSmall.png`;

        return CardFactory.adaptiveCard(
            {
                type: "AdaptiveCard",
                body: [
                    {
                        type: "ColumnSet",
                        columns: [
                            {
                                type: "Column",
                                width: "auto",
                                items: [
                                    {
                                        type: "TextBlock",
                                        size: "Large",
                                        text: customer.CompanyName
                                    },
                                    {
                                        type: "TextBlock",
                                        text: address
                                    },
                                    {
                                        type: "TextBlock",
                                        text: city
                                    },
                                ]
                            },
                            {
                                type: "Column",
                                width: "stretch",
                                items: [
                                    {
                                        type: "Image",
                                        url: mapUrl
                                    }
                                ]
                            }
                        ]
                    }
                ],
                actions: [
                    {
                        "type": "Action.ShowCard",
                        "title": "Details",
                        "card": {
                            "type": "AdaptiveCard",
                            "body": [
                                {
                                    "type": "TextBlock",
                                    "text": details1
                                },
                                {
                                    "type": "TextBlock",
                                    "text": details2
                                },
                                {
                                    "type": "TextBlock",
                                    "text": details3
                                }
                            ]
                        }
                    },
                    {
                        "type": "Action.OpenUrl",
                        "title": "Open",
                        "url": "https://pnp.github.io/"
                    }
                ],
                $schema: "http://adaptivecards.io/schemas/adaptive-card.json",
                version: "1.0"
            }
        );
    }
}
```


## Consume Northwind Service from Search Messaging extension

We will now consume the Northwind service from our search messaging extension at "src\app\customerSearchMessageExtension\CustomerSearchMessageExtension.ts".

1. Import service and cards.

    ```typescript
    import { ServiceFactory } from '../services/serviceFactory';
    import { CustomerPreviewCard } from '../cards/customerPreviewCard';
    import { CustomerResultCard } from '../cards/customerResultCard';
    ```

2. Update onQuery method.

    ```typescript
    public async onQuery(context: TurnContext, query: MessagingExtensionQuery): Promise<MessagingExtensionResult> {
        if (query.parameters && query.parameters[0]) {
            const nameQuery = query.parameters[0].name === "initialRun" ? "" : query.parameters[0].value;
            const nwService = ServiceFactory.getNorthwindService();
            const customers = await nwService.getCustomersByName(nameQuery);
            const attachments: Attachment[] = [];

            for (const c of customers) {
                const card = await CustomerResultCard.getCard(c);
                const preview = CustomerPreviewCard.getCard(c);
                const attachment = { ...card, preview };
                attachments.push(attachment);
            }

            var result = {
                type: "result",
                attachmentLayout: "list",
                attachments: attachments
            } as MessagingExtensionResult;

            return Promise.resolve(result);
        } 
        else {
            throw new Error("Invalid query");
        }
    }
    ```


## Test the solution

Follow below steps to test the search messaging extension.

1. On a separate command prompt, run below command.

    ```
    ngrok.exe http 3007
    ```

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/11.png)

3. Inside .env file, update HOSTNAME=35a70dfecd85.ngrok.io
4. In Azure Bot Channel registration, update Messaging endpoint.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/12.png)

5. On the command prompt, type below command.

    ```
    gulp build
    gulp manifest
    ```

6. The .zip package will be available under packages folder.
7. On the command prompt, type below command.

    ```
    gulp serve --debug
    ```

8. Open MS Teams.
9. Under **Apps** , click **Upload a custom app**.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/13.png)

10. Click **Add**.

    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/14.png)

11. Here is how it works from compose box of MS Teams.
    ![](/media/2020-08-19-search-messaging-extensions-ms-teams/15.gif)


## Summary

Messaging extensions enable users to execute search queries or trigger actions in external systems. The results of these actions are sent from your custom web service to the Microsoft Teams client as embedded web pages or richly formatted cards. Yo Teams helps to build search messaging extensions with ease.


## References

- [Search with messaging extensions](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/messaging-extension-v3/search-extensions)
- [Yo Teams](https://docs.microsoft.com/en-us/microsoftteams/platform/tutorials/get-started-yeoman)


## Code Download

The code developed during this article can be found at: [https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/msgext-customer-search](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/msgext-customer-search)
