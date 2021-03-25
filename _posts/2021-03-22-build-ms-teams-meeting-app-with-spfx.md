---
title: "Build Microsoft Teams meeting app with SPFx"
date: "2021-03-22"
share: true
header:
  image: media/2021-03-22-build-ms-teams-meeting-app-with-spfx/web-part-preview.gif
  teaser: media/2021-03-22-build-ms-teams-meeting-app-with-spfx/web-part-preview.gif
categories:
  - SharePoint Framework
  - MS Teams
tags:
  - "2021"
  - March 2021
last_modified_at: 2021-03-22T00:00:00-00:00
---

## Overview

SPFx v1.12 support for Microsoft Teams meeting apps development.

In this article, we will develop pre-meeting app experience using SPFx. Questionnaire meeting app provides Pre-meeting app experience for MS Teams meeting attendees to ask the questions related to meeting before the meeting starts.

I am privileged to implement this solution along with my colleagues [Ravi Kulkarni](https://www.linkedin.com/in/ravi-kulkarni-a5381723/) and [Smita Nachan](https://www.linkedin.com/in/smitanachan/).

> **Note**
> 
> SPFx v1.12 has added support for Node.js v12 and Gulp 4.

# SharePoint Asset
A SharePoint list (named `Teams Meeting Questionnaire`) should be manually created to store the meeting questionnaires. The attendees questions will be stored per meeting against unique MeetingID from MS Teams meeting. The schema of the list is as below.

Display Name|Internal Name|Type|Required|Comments
------------|-------------|----|--------|--------
Title|Title|Single line of text|Y|OOB Title column
Description|Description|Multiple lines of text|N|
MeetingID|MeetingID|Single line of text|N|

## Develop SPFx Solution

We will develop the SPFx web part as shown below.

![](/media/2021-03-22-build-ms-teams-meeting-app-with-spfx/yo-spfx-solution.png)

We don't need to use a specific host in the supportedHosts property (like TeamsTab or TeamsPersonalApp) inside web parts's `manifest.json`. However, we just need to extend the teams manifest in your SharePoint Framework solution with `configurableTabs`.

## NPM Packages Used
Below NPM package(s) are used to develop this sample:
1. [@pnp/sp](https://pnp.github.io/pnpjs/sp/)
2. [moment](https://www.npmjs.com/package/moment)

## Add Teams Manifest to the solution

We will configure the manifest schema to surface the SPFx web part as Microsoft Teams meeting app.

In the solution, add `manifest.json` file to teams folder.

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/teams/v1.5/MicrosoftTeams.schema.json",
    "manifestVersion": "1.8",
    "version": "1.0.0",
    "id": "%MICROSOFT-APP-ID%",
    "packageName": "Questionnaire Meeting App",
    "developer": {
        "name": "Nanddeep Nachan",
        "websiteUrl": "https://aka.ms/sppnp",
        "privacyUrl": "https://privacy.microsoft.com/privacystatement",
        "termsOfUseUrl": "https://www.microsoft.com/servicesagreement"
    },
    "name": {
        "short": "Questionnaire Meeting App",
        "full": "SPFx based MS Teams Questionnaire Meeting App"
    },
    "description": {
        "short": "MS Teams pre-meeting questionnaire experience",
        "full": "Provides MS Teams pre-meeting experience with questionnaire"
    },
    "icons": {
        "outline": "%MICROSOFT-APP-ID%_outline.png",
        "color": "%MICROSOFT-APP-ID%_color.png"
    },
    "accentColor": "#004578",
    "configurableTabs": [
        {
            "configurationUrl": "https://{teamSiteDomain}{teamSitePath}/_layouts/15/TeamsLogon.aspx?SPFX=true&dest={teamSitePath}/_layouts/15/teamshostedapp.aspx%3FopenPropertyPane=true%26teams%26componentId=%MICROSOFT-APP-ID%%26forceLocale={locale}",
            "canUpdateConfiguration": true,
            "scopes": [
                "team",
                "groupchat"
            ],
            "context": [
                "channelTab",
                "privateChatTab",
                "meetingChatTab",
                "meetingDetailsTab",
                "meetingSidePanel"
            ]
        }
    ],
    "validDomains": [
        "*.login.microsoftonline.com",
        "*.sharepoint.com",
        "*.sharepoint-df.com",
        "spoppe-a.akamaihd.net",
        "spoprod-a.akamaihd.net",
        "resourceseng.blob.core.windows.net",
        "msft.spoppe.com"
    ],
    "webApplicationInfo": {
        "resource": "https://{teamSiteDomain}",
        "id": "00000003-0000-0ff1-ce00-000000000000"
    }
}
```

In the above json, replace `%MICROSOFT-APP-ID%` with your SPFx solution GUID.


## Web Part Properties

The properties should be pre-configured inside `QuestionnaireMeetingAppWebPart.manifest.json` as when the web part is added as MS Teams meeting experience, we do not get any settings to configure.

Property|Type|Required|Default value|Comments
--------|----|--------|-------------|--------
siteUrl|Text|Yes|/|Provide the relative URL of the site where below list exists.
listName|Text|Yes|Teams Meeting Questionnaire|Title of the list storing meeting questionnaires.

## Set SharePoint context using PnP JS

The SharePoint site URL will be availale as configured web part property. In the web part file `src\webparts\questionnaireMeetingApp\QuestionnaireMeetingAppWebPart.ts`, set the SharePoint context as follows:

```typescript
public async onInit(): Promise<void> {
    return super.onInit().then(_ => {
      if (this.context.sdks.microsoftTeams) {
        // checking that we're in Teams
        const context = this.context.sdks.microsoftTeams!.context;
        this._applyTheme(context.theme || 'default');
        this.context.sdks.microsoftTeams.teamsJs.registerOnThemeChangeHandler(this._applyTheme);

        // Setup context to PnPjs
        sp.setup({
          spfxContext: this.context,
          sp: {
            baseUrl: `https://${this.context.sdks.microsoftTeams.context.teamSiteDomain}${this.properties.siteUrl}`
          }
        });
      }
    });
}
```

### Project setup and important files

Below are the major components in the SPFx solution.

```txt
spfx-react-teams-meeting-app-questionnaire
    ├── teams                                                     <-- MS Teams manifest
    │   └── manifest.json
    └── src
        └── models
            ├── IQuestionnaireItem.ts
        └── services
            ├── SPOService.ts                                     <-- Extensible Service
        └── webparts
            └── questionnaireMeetingApp
                ├── QuestionnaireMeetingAppWebPart.manifest.json  <-- Configurable web part properties
                ├── QuestionnaireMeetingAppWebPart.ts
                ├── components
                │   └── QuestionnaireMeetingApp
                │   │   ├── QuestionnaireMeetingApp.tsx           <-- Questionnaire Component
                │   │   ├── QuestionnaireMeetingApp.module.scss
                │   │   ├── IQuestionnaireMeetingAppProps.ts
                │   │   ├── IQuestionnaireMeetingAppState.ts
                │   └── Popup                                     <-- New Question Creation Component
                |   │   ├── AskQuestion.tsx
                |   │   ├── IAskQuestionProps.ts
                |   │   ├── IAskQuestionState.ts
                └── loc
                    ├── en-us.js
                    └── mystrings.d.ts
```

## Questionnaire Meeting App Component

Implement the React component (src\webparts\questionnaireMeetingApp\components\QuestionnaireMeetingApp.tsx), to let attendees ask the qeustions and display them based on MeetingID. 

```typescript
import * as React from 'react';
import * as strings from 'QuestionnaireMeetingAppWebPartStrings';
import styles from './QuestionnaireMeetingApp.module.scss';
import { IQuestionnaireMeetingAppProps } from './IQuestionnaireMeetingAppProps';
import { IQuestionnaireMeetingAppState } from './IQuestionnaireMeetingAppState';
import { IQuestionnaireItem } from "../../../models/IQuestionnaireItem";
import SPOService from '../../../services/SPOService';
import { PrimaryButton } from 'office-ui-fabric-react';
import { AskQuestion } from './Popup/AskQuestion';
import { ActivityItem, IActivityItemProps, Link, mergeStyleSets } from 'office-ui-fabric-react';
import * as moment from 'moment';

const classNames = mergeStyleSets({
  exampleRoot: {
    marginTop: '20px',
  },
  nameText: {
    fontWeight: 'bold',
  },
});

export default class QuestionnaireMeetingApp extends React.Component<IQuestionnaireMeetingAppProps, IQuestionnaireMeetingAppState> {
  private SPOService: SPOService = null;

  public constructor(props) {
    super(props);

    this.state = {
      infoLoaded: false,
      meetingQuestionnaire: [],
      showPopup: false
    };

    this.SPOService = new SPOService();
    this.onDismissPanel = this.onDismissPanel.bind(this);
  }

  public async componentDidMount() {
    const meetingQuestionnaireInfo: IQuestionnaireItem[] = await this.SPOService.getQuestionnaire(this.props.listName, this.props.context.sdks.microsoftTeams.context.meetingId);

    this.setState({
      infoLoaded: true,
      meetingQuestionnaire: meetingQuestionnaireInfo
    });
  }

  private async onDismissPanel(refresh: boolean) {
    this.setState({ showPopup: false, infoLoaded: false });
    if (refresh === true) {
      const meetingQuestionnaireInfo: IQuestionnaireItem[] = await this.SPOService.getQuestionnaire(this.props.listName, this.props.context.sdks.microsoftTeams.context.meetingId);
      this.setState({
        infoLoaded: true,
        meetingQuestionnaire: meetingQuestionnaireInfo
      });
    }
  }

  public render(): React.ReactElement<IQuestionnaireMeetingAppProps> {
    return (
      <div className={styles.questionnaireMeetingApp}>
        <div className={styles.container}>
          <div className={styles.row}>
            <div className={styles.column}>
              <PrimaryButton className={styles.button} onClick={() => { this.setState({ showPopup: true }); }} text={strings.AddQuestion} />
            </div>
          </div>
          <div className={styles.row}>
            <div className={styles.column}>
              <div>
                {
                  this.state.meetingQuestionnaire.map(item => {
                    const activityItem: (IActivityItemProps & { key: string | number }) = {
                      key: item.ID,
                      activityDescription: [
                        <Link
                          key={item.ID}
                          className={classNames.nameText}
                        >
                          {item.Author.Title}
                        </Link>,
                        <span key={2}> {strings.Posted} </span>
                        <span key={3} className={classNames.nameText}>
                          {item.Title}
                        </span>
                      ],
                      activityPersonas: [{ imageUrl: `/_layouts/15/userphoto.aspx?size=S&username=${item.Author.EMail}` }],
                      comments: item.Description,
                      timeStamp: moment(item.Modified).format("LLL")
                    };

                    return (
                      <ActivityItem {...activityItem} key={activityItem.key} className={classNames.exampleRoot} />
                    );
                  })
                }
              </div>

              <div>
                {
                  this.state.showPopup &&
                  <AskQuestion
                    onDissmissPanel={this.onDismissPanel}
                    showPopup={this.state.showPopup}
                    context={this.props.context}
                    listName={this.props.listName}
                  />
                }
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }
}
```

## SPO Service
Implement the service for SPO operations.

```typescript
import { IQuestionnaireItem } from "../models/IQuestionnaireItem";
import { sp } from '@pnp/sp/presets/all';

export default class SPOService {
    public async getQuestionnaire(listTitle: string, meetingId: string): Promise<IQuestionnaireItem[]> {
        let meetingQuestionnaire: IQuestionnaireItem[] = [];

        try {
            // Get Client POC Master
            meetingQuestionnaire = await sp.web.lists.getByTitle(listTitle)
                .items
                .select("ID,MeetingID,Title,Description,Author/Title,Author/EMail,Modified")
                .expand("Author")
                .filter(`MeetingID eq '${meetingId}'`)
                .orderBy("Modified", false)
                .get<IQuestionnaireItem[]>();
        }
        catch (error) {
            console.log(error);
            return Promise.reject(error);
        }
        return meetingQuestionnaire;
    }

    public async addQuestion(listTitle: string, item: IQuestionnaireItem): Promise<boolean> {
        try {
            // Get Client POC Master
            return sp.web.lists.getByTitle(listTitle)
                .items
                .add({
                    Title: item.Title,
                    Description: item.Description,
                    MeetingID: item.MeetingID
                })
                .then((value) => {
                    return Promise.resolve(true);
                });
        }
        catch (error) {
            return Promise.reject(error);
        }
    }
}
```

## Deploy the solution

**SharePoint Deployment**

- Bundle the solution: `$ gulp bundle --ship`
- Package the solution: `$ gulp package-solution --ship`
- Locate the solution at `./sharepoint/solution/spfx-ms-teams-questionnaire-meeting-app.sppkg` and upload it to SharePoint tenant app catalog

![Deploy SPFx solution](/media/2021-03-22-build-ms-teams-meeting-app-with-spfx/deploy-spfx-solution.png)
- Select `Make this solution available to all sites in the organization`.
- Click `Deploy`

**MS Teams Deployment**

- Navigate to `teams` folder and zip the content (2 png files and manifest.json).
- Open MS Teams.
- Click `Apps`.
- Click `Upload a custom app` > `Upload for <tenant>`.

![Deploy to MS Teams](/media/2021-03-22-build-ms-teams-meeting-app-with-spfx/deploy-to-ms-teams.png)


## The meeting app in action

The Questionnaire meeting app displays the questions from attendees as pre-meeting app experience.
![Questionnaire Preview](/media/2021-03-22-build-ms-teams-meeting-app-with-spfx/questionnaire-preview.png)

## Summary

This article illustrates the possibilities to surface SPFx web part as Microsoft Teams meeting app.

## References

- [Announcing SharePoint Framework 1.12](https://developer.microsoft.com/en-us/microsoft-365/blogs/announcing-sharepoint-framework-1-12-extending-more-of-microsoft-teams/?WT.mc_id=M365-MVP-5003693)
- [Apps in Teams meetings](https://docs.microsoft.com/en-us/microsoftteams/platform/apps-in-teams-meetings/teams-apps-in-meetings?WT.mc_id=M365-MVP-5003693)
- [Manifest schema for Microsoft Teams](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema?WT.mc_id=M365-MVP-5003693)
- [PnPjs Configuration](https://pnp.github.io/pnpjs/concepts/configuration/)
- [Support Microsoft Teams Themes in SharePoint Framework Solutions](https://blog.aterentiev.com/support-microsoft-teams-themes-in) by Alex Terentiev, [@alexaterentiev](https://twitter.com/alexaterentiev)
- [Getting started with SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant?WT.mc_id=M365-MVP-5003693)
- [Building for Microsoft teams](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/build-for-teams-overview?WT.mc_id=M365-MVP-5003693)


## Code Download
The code developed during this article is proudly available under PnP repo and can be found here:
[https://github.com/pnp/sp-dev-fx-webparts/tree/master/samples/react-teams-meeting-app-questionnaire](https://github.com/pnp/sp-dev-fx-webparts/tree/master/samples/react-teams-meeting-app-questionnaire) 
