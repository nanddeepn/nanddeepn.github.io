---
title: "Building Classified Listings Copilot in Microsoft 365"
date: "2024-06-27"
share: true
header:
  image: media/2024-06-27-m365-classified-listings-copilot/preview-teams.png
  teaser: media/2024-06-27-m365-classified-listings-copilot/preview-teams.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - June 2024
last_modified_at: 2024-06-27T00:00:00-00:00
---
## Overview

Microsoft Teams messaging extensions can be extended as Microsoft 365 Copilot. Microsoft Teams Toolkit allows creating this functionality.

In this article, I will walk you through one of such Copilot created as a messaging extension using Teams Toolkit – Classified Listings copilot.


## Classified Listings Copilot

This sample implements a Teams message extension that can be used as a plugin for Microsoft Copilot for Microsoft 365. It allows users to post the classified listings of items they want to sell, buy, or rent. Users can search listings posted by others.

![](/media/2024-06-27-m365-classified-listings-copilot/preview-teams.png)


## Prerequisites

Below are the prerequisites to build this Copilot:

- [Node.js 18.x](https://nodejs.org/download/release/v18.18.2/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Teams Toolkit](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
- You will need a Microsoft work or school account with [permissions to upload custom Teams applications](https://learn.microsoft.com/microsoftteams/platform/concepts/build-and-test/prepare-your-o365-tenant#enable-custom-teams-apps-and-turn-on-custom-app-uploading). The account will also need a Microsoft Copilot for Microsoft 365 license to use the extension in Copilot.
- You will need to create [Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) resource on Azure portal.
- [Azure Storage Explorer](https://azure.microsoft.com/products/storage/storage-explorer/) (OPTIONAL) - Download this if you want to view and edit the Azure tables used in this sample.


## Setup Copilot Project with Teams Toolkit

Follow below steps to setup Copilot with Teams Toolkit:

1. Open Visual Studio Code.
2. Open Teams Toolkit extension from the left menu.
3. Click **Create a New App**.
4. Select **Messaging Extension** > **Custom Search Results**.

    ![](/media/2024-06-27-m365-classified-listings-copilot/01.png)


## Azure Table Schema

The solution uses below 3 tables:

1. `AZURE_TABLE_PREFIX`Listing

    This table stores the classified listings information posted by users with below schema:

    | # | Name | Description |
    |---|------|-------------|
    | 1 | PartitionKey | Set as `Listing` |
    | 2 | RowKey | Set as the conversation id |
    | 3 | Title |  |
    | 4 | Description |  |
    | 5 | Price |  |
    | 6 | CityID | Referenced from `AZURE_TABLE_PREFIX`ListingCity |
    | 7 | TypeID | Referenced from `AZURE_TABLE_PREFIX`ListingType |
    | 8 | OwnerID | AAD ID of user |
    | 9 | OwnerName | Display name of user |

2. `AZURE_TABLE_PREFIX`ListingCity

    This table stores the city information with below schema:

    | # | Name | Description |
    |---|------|-------------|
    | 1 | PartitionKey | Set as `City` |
    | 2 | RowKey | ID of city (e.g. 1, 2, 3) |
    | 3 | Name | Name of city |

    Below is a sample data:

    | PartitionKey | RowKey | Name |
    |--------------|--------|------|
    | City | 1 | Pune |
    | City | 2 | Mumbai |
    | City | 3 | New York |
    | City | 4 | Sydney |
    | City | 5 | Chicago |
    | City | 6 | London |

3. `AZURE_TABLE_PREFIX`ListingType

    This table stores the information about type of classified listing with below schema:

    | # | Name | Description |
    |---|------|-------------|
    | 1 | PartitionKey | Set as `Type` |
    | 2 | RowKey | ID of listing type (e.g. 1, 2, 3) |
    | 3 | Name | Name of listing type |

    Below is a sample data:

    | PartitionKey | RowKey | Name |
    |--------------|--------|------|
    | Type | 1 | Buy |
    | Type | 2 | Rent |
    | Type | 3 | Sell |


### Environment configuration

In the file `env/.env.local.user`, includes below line.

```
SECRET_STORAGE_ACCOUNT_CONNECTION_STRING=xxxxxxxxxxxxxxxxxxxxxxx
```

In the file `env/.env.local`, include below line. Please replace "Contoso" with your desired prefix for the Azure table.

```
AZURE_TABLE_PREFIX=Contoso
```


### Manifest changes

A messaging extension works as a Copilot with the configurations below in `appPackage\manifest.json` file.

In the `composeExtensions` > `commands` element in the file, define the `id` and `parameters` as follows:

```
...

"composeExtensions": [
{
    "botId": "${{BOT_ID}}",
    "commands": [
        {
            "id": "classifiedListingSearch",
            "context": [
                "compose",
                "commandBox"
            ],
            "description": "Post classified listings of items you want to sell, buy, or rent. Search listings posted by others.",
            "title": "Classified Listings",
            "type": "query",
            "semanticDescription": "This command allows users to post the classified listings of items they want to sell, buy, or rent. Users can also search listings posted by others. For e.g., Find bikes in classified listings at New York for sell under 60000",
            "parameters": [
                {
                    "name": "itemName",
                    "title": "Title of the listing item",
                    "description": "The name or title of the listings item to be searched.",
                    "inputType": "text",
                    "semanticDescription": "This parameter is used to identify the specific classified item to be queried. Users should provide the exact name or title of the classified item they want to retrieve information for as the value of this parameter."
                },
                {
                    "name": "location",
                    "title": "Location of the listings item",
                    "description": "The location of the listings items to be searched.",
                    "inputType": "text",
                    "semanticDescription": "This parameter is used to identify the location of items to be queried. Users should provide the value of this parameter as either city, state, or country to filter the classified items based on the location."
                },
                {
                    "name": "type",
                    "title": "Type of the listings item",
                    "description": "The type of the listings items to be searched.",
                    "inputType": "text",
                    "semanticDescription": "This parameter is used to identify the type of items to be queried. Users should provide the value of this parameter as either 'sell', 'buy', or 'rent' to filter the classified items based on the type of transaction."
                },
                {
                    "name": "price",
                    "title": "Price of the listings item",
                    "description": "The price range of the listings items to be searched.",
                    "inputType": "text",
                    "semanticDescription": "This parameter is used to specify the price range of the items to be queried. Users should provide the value of this parameter as price range to filter the classified items based on the price."
                }
            ]
        }
    ]
}
...
```

The natural language prompt from the user is then mapped to each of these parameters.

## Define messaging extension

The parameters are then passed to the messaging extension to process further as follows:

```js
async function handleTeamsMessagingExtensionQuery(
    context: TurnContext,
    query: MessagingExtensionQuery
): Promise<MessagingExtensionResponse> {
    let itemName, type, location, price;

    if (query.parameters.length === 1 && query.parameters[0]?.name === "itemName") {
        [itemName, type, location, price] = (query.parameters[0]?.value.split(','));
    }
    else {
        itemName = cleanupParam(query.parameters.find((element) => element.name === "itemName")?.value);
        type = cleanupParam(query.parameters.find((element) => element.name === "type")?.value);
        location = cleanupParam(query.parameters.find((element) => element.name === "location")?.value);
        price = cleanupParam(query.parameters.find((element) => element.name === "price")?.value);
    }
    ...
}
```


## Adaptive Cards

Light weight JSON based Adaptive Cards are used to display the information back to the user as a response.

Below code can help to push the adaptive card:

```js
const preview = CardFactory.heroCard(classifiedItem.Title,
            `Posted by ${classifiedItem.OwnerName} in ${classifiedItem.CityName} <br />With price of ${classifiedItem.Price} for ${classifiedItem.TypeName}`);

const resultCard = cardHandler.getEditCard(classifiedItem, cityChoices, typeChoices);
const attachment = { ...resultCard, preview };
attachments.push(attachment);
```

The adaptive card at the location `src\adaptiveCards\successCard.json` displays the content as follows:

```json
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.5",
  "body": [
    {
      "type": "Container",
      "style": "good",
      "separator": true,
      "items": [
        {
          "type": "TextBlock",
          "text": "${message}",
          "weight": "Bolder",
          "size": "Medium",
          "color": "Good"
        }
      ]
    },
    {
      "type": "Container",
      "separator": true,
      "items": [
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "items": [
                {
                  "type": "TextBlock",
                  "size": "large",
                  "weight": "bolder",
                  "text": "📦 ${title}",
                  "wrap": true,
                  "style": "heading"
                }
              ],
              "width": "60"
            }
          ]
        }
      ],
      "bleed": true
    },
    {
      "type": "Container",
      "style": "emphasis",
      "items": [
        {
          "type": "TextBlock",
          "weight": "Bolder",
          "text": "**📍Owner information**",
          "wrap": true,
          "size": "Medium",
          "isSubtle": false
        },
        {
          "type": "ColumnSet",
          "separator": true,
          "columns": [
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "FactSet",
                  "spacing": "Large",
                  "facts": [
                    {
                      "title": "Name:",
                      "value": "${ownerName}"
                    }
                  ],
                  "separator": true
                }
              ]
            }
          ]
        },
        {
          "type": "TextBlock",
          "weight": "Bolder",
          "text": "**🛒 Classified listing information**",
          "wrap": true,
          "size": "Medium",
          "isSubtle": false
        },
        {
          "type": "ColumnSet",
          "separator": true,
          "columns": [
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "FactSet",
                  "spacing": "Large",
                  "facts": [
                    {
                      "title": "Type:",
                      "value": "${typeName}"
                    },
                    {
                      "title": "Price:",
                      "value": "${price}"
                    }
                  ],
                  "separator": true
                }
              ]
            },
            {
              "type": "Column",
              "width": "stretch",
              "items": [
                {
                  "type": "FactSet",
                  "spacing": "Large",
                  "facts": [
                    {
                      "title": "Location:",
                      "value": "${cityName}"
                    },
                    {
                      "title": "Posted on:",
                      "value": "${timestamp}"
                    }
                  ],
                  "separator": true
                }
              ]
            }
          ]
        },
        {
          "type": "TextBlock",
          "weight": "Bolder",
          "text": "${description}",
          "wrap": true,
          "isSubtle": false
        }
      ]
    }
  ]
}
```

## Test in Copilot

- Enable the plugin
- Use a basic prompt: `Find bikes in classified listings`

![Screenshot of the basic prompt working in Copilot in Microsoft Teams](/media/2024-06-27-m365-classified-listings-copilot/basic-prompt-teams.png)

- Use an advanced prompt: `Find bikes in classified listings in Mumbai for sell under 200000`

![Screenshot of the advanced prompt working in Copilot in Microsoft Teams](/media/2024-06-27-m365-classified-listings-copilot/advanced-prompt-teams.png)


## Summary

Microsoft Teams messaging extensions can be extended as Microsoft 365 Copilot. Microsoft Teams Toolkit allows creating this functionality.

This sample was co-developed with [Smita Nachan](https://www.linkedin.com/in/smitanachan/). Try out the sample and let us know your thoughts.


## References

- [Extend Microsoft 365 Copilot](https://learn.microsoft.com/en-us/microsoftteams/platform/archive/how-to-extend-copilot?WT.mc_id=M365-MVP-5003693)
- [Build message extensions for Microsoft Teams and Copilot](https://devblogs.microsoft.com/microsoft365dev/build-message-extensions-for-microsoft-teams-and-copilot/?WT.mc_id=M365-MVP-5003693)


## Code download

The is available in the PnP Microsoft Teams Development Community Samples repository at: [https://github.com/pnp/teams-dev-samples/tree/main/samples/msgext-classified-listings-ts](https://github.com/pnp/teams-dev-samples/tree/main/samples/msgext-classified-listings-ts).
