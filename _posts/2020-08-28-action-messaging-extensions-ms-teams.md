---
title: "Implementing Action Messaging Extensions for MS Teams with Yo Teams"
date: "2020-08-28"
share: true
header:
  image: media/2020-08-28-action-messaging-extensions-ms-teams/13.gif
  teaser: media/2020-08-28-action-messaging-extensions-ms-teams/13.gif
categories:
  - MS Teams
tags:
  - "2020"
  - August 2020
last_modified_at: 2020-08-28T00:00:00-00:00
---

## Overview

Action-based messaging extensions allow users to trigger actions in external services while inside of Teams.

In this article, we will create action messaging extensions in a custom Microsoft Teams app using Yo Teams. The action message extension searches the planet information from JSON.


## Action messaging extension overview

A messaging extension is a cloud-hosted service that listens to user requests and responds with structured data, such as a card. It allows us to present users with a modal popup (called a task module in Teams) to collect or display information, then process their interaction and send information back to Teams. It takes advantage of the Bot Framework's messaging schema and secure communication protocol to connect with Microsoft Teams Client App.


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

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/01.png)

5. Click **Create**.
6. Fill in the details to create Bot channels registration.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/02.png)

7. Click **Create**.
8. The new Bot channels registration is ready.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/03.png)


**Get the App Id and secret**

1. From the left menu, click **Settings**.
2. Add **Messaging endpoint** as ngrok URL.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/04.png)

3. Click **Microsoft App ID (Manage)**.
4. Under **Certificates &amp; secrets** , create **New client secret**.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/05.png)

5. Note down the values for future reference.


**Configure MS Teams Channel**

1. From the left menu, click **Channels**.
2. Click **Configure Microsoft Teams Channel**.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/06.png)

3. Follow the instructions to configure the MS Teams channel.


## Implement Action Messaging extension with Yo Teams

We will generate a solution using Yo Teams.

1. On the command prompt, type below command.

    ```
    yo teams
    ```

2. Follow the wizard to set up the solution.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/07.png)

3. On the command prompt, type code . to open the solution in visual studio code.
4. In the .env file, configure application ID and client secret from Bot channels registration.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/08.png)


## Planets Data as Backend

We will implement Planets data as a backend to query against.

Add a new file ```planets.json``` to the "/src/app" folder and add the following JSON to it:

```json
[
    {
      "id": "1",
      "name": "Mercury",
      "summary": "Mercury is the smallest and innermost planet in the Solar System. Its orbit around the Sun takes 87.97 days, the shortest of all the planets in the Solar System. It is named after the Roman deity Mercury, the messenger of the gods.",
      "solarOrbitYears": 0.24,
      "solarOrbitAvgDistanceKm": 57909050,
      "numSatellites": 0,
      "wikiLink": "https://en.wikipedia.org/wiki/Mercury_(planet)",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/d/d9/Mercury_in_color_-_Prockter07-edit1.jpg",
      "imageAlt": "NASA/Johns Hopkins University Applied Physics Laboratory/Carnegie Institution of Washington [Public domain]"
    },
    {
      "id": "2",
      "name": "Venus",
      "summary": "Venus is the second planet from the Sun. It is named after the Roman goddess of love and beauty. As the second-brightest natural object in the night sky after the Moon, Venus can cast shadows and, rarely, is visible to the naked eye in broad daylight. Venus lies within Earth's orbit, and so never appears to venture far from the Sun, setting in the west just after dusk and rising in the east a bit before dawn.",
      "solarOrbitYears": 0.62,
      "solarOrbitAvgDistanceKm": 108208000,
      "numSatellites": 0,
      "wikiLink": "https://en.wikipedia.org/wiki/Venus",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/e/e5/Venus-real_color.jpg",
      "imageAlt": ""Image processing by R. Nunes", link to http://www.astrosurf.com/nunes [Public domain]"
    },
    {
      "id": "3",
      "name": "Earth",
      "summary": "Earth is the third planet from the Sun and the only astronomical object known to harbor life. According to radiometric dating and other sources of evidence, Earth formed over 4.5 billion years ago. Earth's gravity interacts with other objects in space, especially the Sun and the Moon, which is Earth's only natural satellite. Earth orbits around the Sun in 365.256 days, a period known as an Earth sidereal year. During this time, Earth rotates about its axis about 366.256 times.",
      "solarOrbitYears": 1.00,
      "solarOrbitAvgDistanceKm": 149597500,
      "numSatellites": 1,
      "wikiLink": "https://en.wikipedia.org/wiki/Earth",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/9/97/The_Earth_seen_from_Apollo_17.jpg",
      "imageAlt": "Apollo 17 [Public domain]"
    },
    {
      "id": "4",
      "name": "Mars",
      "summary": "Mars is the fourth planet from the Sun and the second-smallest planet in the Solar System after Mercury. In English, Mars carries a name of the Roman god of war and is often referred to as the 'Red Planet'. The latter refers to the effect of the iron oxide prevalent on Mars' surface, which gives it a reddish appearance distinctive among the astronomical bodies visible to the naked eye.",
      "solarOrbitYears": 1.88,
      "solarOrbitAvgDistanceKm": 134935000,
      "numSatellites": 2,
      "wikiLink": "https://en.wikipedia.org/wiki/Mars",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/0/02/OSIRIS_Mars_true_color.jpg",
      "imageAlt": "ESA - European Space Agency &amp; Max-Planck Institute for Solar System Research for OSIRIS Team ESA/MPS/UPD/LAM/IAA/RSSD/INTA/UPM/DASP/IDA [CC BY-SA 3.0-IGO (https://creativecommons.org/licenses/by-sa/3.0-igo)]"
    },
    {
      "id": "5",
      "name": "Jupiter",
      "summary": "Jupiter is the fifth planet from the Sun and the largest in the Solar System. It is a gas giant with a mass one-thousandth that of the Sun, but two-and-a-half times that of all the other planets in the Solar System combined. Jupiter is one of the brightest objects visible to the naked eye in the night sky, and has been known to ancient civilizations since before recorded history. It is named after the Roman god Jupiter. When viewed from Earth, Jupiter can be bright enough for its reflected light to cast shadows, and is on average the third-brightest natural object in the night sky after the Moon and Venus.",
      "solarOrbitYears": 11.86,
      "solarOrbitAvgDistanceKm": 445336000,
      "numSatellites": 78,
      "wikiLink": "https://en.wikipedia.org/wiki/Jupiter",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/5/50/Jupiter%2C_image_taken_by_NASA%27s_Hubble_Space_Telescope%2C_June_2019_-_Edited.jpg",
      "imageAlt": "NASA, ESA, and A. Simon (NASA Goddard), edited by PlanetUser [Public domain]"
    },
    {
      "id": "6",
      "name": "Saturn",
      "summary": "Saturn is the sixth planet from the Sun and the second-largest in the Solar System, after Jupiter. It is a gas giant with an average radius about nine times that of Earth. It has only one-eighth the average density of Earth; however, with its larger volume, Saturn is over 95 times more massive. Saturn is named after the Roman god of wealth and agriculture; its astronomical symbol (♄) represents the god's sickle.",
      "solarOrbitYears": 29.46,
      "solarOrbitAvgDistanceKm": 1433525000,
      "numSatellites": 82,
      "wikiLink": "https://en.wikipedia.org/wiki/Saturn",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/c/c7/Saturn_during_Equinox.jpg",
      "imageAlt": "NASA / JPL / Space Science Institute [Public domain]"
    },
    {
      "id": "7",
      "name": "Uranus",
      "summary": "Uranus is the seventh planet from the Sun. It has the third-largest planetary radius and fourth-largest planetary mass in the Solar System. Uranus is similar in composition to Neptune, and both have bulk chemical compositions which differ from that of the larger gas giants Jupiter and Saturn. For this reason, scientists often classify Uranus and Neptune as \"ice giants\" to distinguish them from the gas giants.",
      "solarOrbitYears": 84.02,
      "solarOrbitAvgDistanceKm": 2883000000,
      "numSatellites": 27,
      "wikiLink": "https://en.wikipedia.org/wiki/Uranus",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/3/3d/Uranus2.jpg",
      "imageAlt": "NASA/JPL-Caltech [Public domain]"
    },
    {
      "id": "8",
      "name": "Neptune",
      "summary": "Neptune is the eighth and farthest known planet from the Sun in the Solar System. In the Solar System, it is the fourth-largest planet by diameter, the third-most-massive planet, and the densest giant planet. Neptune is 17 times the mass of Earth, slightly more massive than its near-twin Uranus. Neptune is denser and physically smaller than Uranus because its greater mass causes more gravitational compression of its atmosphere.",
      "solarOrbitYears": 164.80,
      "solarOrbitAvgDistanceKm": 4500000000,
      "numSatellites": 14,
      "wikiLink": "https://en.wikipedia.org/wiki/Neptune",
      "imageLink": "https://upload.wikimedia.org/wikipedia/commons/6/63/Neptune_-_Voyager_2_%2829347980845%29_flatten_crop.jpg",
      "imageAlt": "Justin Cowart [CC BY (https://creativecommons.org/licenses/by/2.0)]"
    }
  ]
```

Add a new file ```planetDisplayCard.json``` to the "/src/app" folder and add the following JSON to it. This file contains the Adaptive Card used to generate the details of the planet:

```json
{
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "type": "AdaptiveCard",
    "version": "1.0",
    "body": [
      {
        "id": "cardHeader",
        "type": "Container",
        "items": [
          {
            "id": "planetName",
            "type": "TextBlock",
            "weight": "bolder",
            "size": "medium"
          }
        ]
      },
      {
        "type": "Container",
        "id": "cardBody",
        "items": [
          {
            "id": "planetSummary",
            "type": "TextBlock",
            "wrap": true
          },
          {
            "id": "planetDetails",
            "type": "ColumnSet",
            "columns": [
              {
                "type": "Column",
                "width": "100",
                "items": [
                  {
                    "id": "planetImage",
                    "size": "stretch",
                    "type": "Image"
                  }
                ]
              },
              {
                "type": "Column",
                "width": "250",
                "items": [
                  {
                    "type": "FactSet",
                    "facts": [
                      {
                        "id": "orderFromSun",
                        "title": "Order from the sun:"
                      },
                      {
                        "id": "planetNumSatellites",
                        "title": "Known satellites:"
                      },
                      {
                        "id": "solarOrbitYears",
                        "title": "Solar orbit (*Earth years*):"
                      },
                      {
                        "id": "solarOrbitAvgDistanceKm",
                        "title": "Average distance from the sun (*km*):"
                      }
                    ]
                  }
                ]
              }
            ]
          },
          {
            "id": "imageAttribution",
            "type": "TextBlock",
            "size": "medium",
            "isSubtle": true,
            "wrap": true
          }
        ]
      }
    ],
    "actions": [
      {
        "type": "Action.OpenUrl",
        "title": "Learn more on Wikipedia"
      }
    ]
  }
```


## Implement Action Messaging extension with Yo Teams

Update the action messaging extension to use Planet information.

1. Open file "src\app\planetActionMessagingMessageExtension\PlanetActionMessagingMessageExtension.ts".
2. Update ```onFetchTask``` method as below to show input message for planet selection:

    ```typescript
    public async onFetchTask(context: TurnContext, value: MessagingExtensionQuery): Promise<MessagingExtensionResult | TaskModuleContinueResponse> {
        return Promise.resolve<TaskModuleContinueResponse>({
            type: "continue",
            value: {
                title: "Input form",
                card: CardFactory.adaptiveCard({
                    $schema: "http://adaptivecards.io/schemas/adaptive-card.json",
                    type: "AdaptiveCard",
                    version: "1.2",
                    body: [
                        {
                            type: "TextBlock",
                            text: "Please enter planet name to insert into the message:"
                        },
                        {
                            type: "Input.Text",
                            id: "planetName",
                            placeholder: "Enter planet name (e.g. Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune)"
                        },
                    ],
                    actions: [
                        {
                            type: "Action.Submit",
                            title: "OK",
                            data: { id: "unique-id" }
                        }
                    ]
                })
            }
        });
    }
    ```

3. Update ```onSubmitAction``` method to query the planet data in json format and display the planet information.

    ```typescript
    public async onSubmitAction(context: TurnContext, value: TaskModuleRequest): Promise<MessagingExtensionResult> {
        // load planets & sort them by their order from the sun
        const planets: any = require("../planets.json");

        // get the selected planet
        const selectedPlanet: any = planets.filter((planet) => planet.name === value.data.planetName)[0];

        // load display card
        const adaptiveCardSource: any = require("../planetDisplayCard.json");

        // update planet fields in display card
        adaptiveCardSource.actions[0].url = selectedPlanet.wikiLink;
        find(adaptiveCardSource.body, { "id": "cardHeader" }).items[0].text = selectedPlanet.name;
        const cardBody: any = find(adaptiveCardSource.body, { "id": "cardBody" });
        find(cardBody.items, { "id": "planetSummary" }).text = selectedPlanet.summary;
        find(cardBody.items, { "id": "imageAttribution" }).text = "*Image attribution: " + selectedPlanet.imageAlt + "*";
        const cardDetails: any = find(cardBody.items, { "id": "planetDetails" });
        cardDetails.columns[0].items[0].url = selectedPlanet.imageLink;
        find(cardDetails.columns[1].items[0].facts, { "id": "orderFromSun" }).value = selectedPlanet.id;
        find(cardDetails.columns[1].items[0].facts, { "id": "planetNumSatellites" }).value = selectedPlanet.numSatellites;
        find(cardDetails.columns[1].items[0].facts, { "id": "solarOrbitYears" }).value = selectedPlanet.solarOrbitYears;
        find(cardDetails.columns[1].items[0].facts, { "id": "solarOrbitAvgDistanceKm" }).value = Number(selectedPlanet.solarOrbitAvgDistanceKm).toLocaleString();

        // return the adaptive card
        const card = CardFactory.adaptiveCard(adaptiveCardSource);

        return Promise.resolve({
            type: "result",
            attachmentLayout: "list",
            attachments: [card]
        } as MessagingExtensionResult);
    }
    ```


## Test the solution

Follow the below steps to test the action messaging extension.

1. On a separate command prompt, run below command.

    ```
    ngrok.exe http 3007
    ```

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/09.png)

2. Inside .env file, update HOSTNAME=35a70dfecd85.ngrok.io
3. In Azure Bot Channel registration, update Messaging endpoint.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/10.png)

4. On the command prompt, type below command.

    ```
    gulp build
    gulp manifest
    ```

5. The .zip package will be available under the packages folder.
6. On the command prompt, type below command.

    ```
    gulp serve --debug
    ```

7. Open MS Teams.
8. Under **Apps**, click **Upload a custom app**.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/11.png)

9. Click **Add**.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/12.png)

10. Here is how it works from the compose box of MS Teams.

    ![](/media/2020-08-28-action-messaging-extensions-ms-teams/13.gif)


## Summary

Action-based messaging extensions allow users to trigger actions in external services while inside of Teams. Yo Teams helps to build action messaging extensions with ease.


## References

- [Action based messaging extensions](https://docs.microsoft.com/en-us/microsoftteams/platform/messaging-extensions/how-to/action-commands/define-action-command)
- [Yo Teams](https://docs.microsoft.com/en-us/microsoftteams/platform/tutorials/get-started-yeoman)


## Code Download

The code developed during this article can be found at: [https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/planet-action-messaging-extension](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/yo-teams/planet-action-messaging-extension)
