---
title: "Build ACE for M365 Groups Membership"
date: "2022-01-27"
share: true
header:
  image: media/2022-01-27-ace-m365-groups-membership/demo.gif
  teaser: media/2022-01-27-ace-m365-groups-membership/demo.gif
categories:
  - SharePoint Framework
  - Adaptive Card Extension
  - Microsoft Viva
tags:
  - "2022"
  - January 2022
last_modified_at: 2022-01-27T00:00:00-00:00
---
## Overview

Adaptive Card Extensions (ACEs) component type in SharePoint Framework (SPFx) enables developers to build a rich experience for Viva Connections' Dashboards and SharePoint Pages.

In this article, we will build an ACE component to display the M365 (Unified) groups in a tenant, of which user is owner or member.

## Highlights of the ACE component

Below are the highlights of the ACE component:

- Displays current logged in user's M365 (Unified) groups in card view.
- Action buttons on card view to browse to groups, of which user is owner or member.
- Quick view showing M365 Group details.
- Buttons on Quick view to take user to associated SharePoint site and MS Teams (if connected).

## Scaffold Adaptive Card Extensions (ACE) solution

Start the scaffolding process by running the below command:

```
yo @microsoft/sharepoint
```

![](/media/2022-01-27-ace-m365-groups-membership/01.png)

> Note:
> SPFx v1.13 or greater supports creation of ACEs.

## Permissions configuration

We need following set of permissions configured in the file `config\package-solution.json` in order to get Office 365 groups.

```json
"webApiPermissionRequests": [
      {
        "resource": "Microsoft Graph",
        "scope": "Group.Read.All"
      },
      {
        "resource": "Microsoft Graph",
        "scope": "Directory.Read.All"
      },
      {
        "resource": "Microsoft Graph",
        "scope": "User.Read.All"
      }
    ]
```

## Service to get the Group information

We will develop a service at `src\services\M365GroupService.ts` to get the group related information.

The service will have below methods implemented.

getMyMemberGroups: Returns the groups where the current user is a member of.

```typescript
public getMyMemberGroups(): Promise<IGroup[]> {
    return new Promise<IGroup[]>((resolve, reject) => {
        try {
            // Prepare the output array
            var m365groups: Array<IGroup> = new Array<IGroup>();

            this.context.msGraphClientFactory
                .getClient()
                .then((client: MSGraphClient) => {
                    client
                        .api("/me/memberOf/$/microsoft.graph.group?$filter=groupTypes/any(a:a eq 'unified')")
                        .get((error: any, groups: IGroupCollection, rawResponse: any) => {
                            // Map the response to the output array
                            groups.value.map((item: any) => {
                                m365groups.push({
                                    id: item.id,
                                    displayName: item.displayName,
                                    description: item.description,
                                    visibility: item.visibility,
                                    teamsConnected: item.resourceProvisioningOptions.indexOf("Team") > -1 ? true : false
                                });
                            });

                            resolve(m365groups);
                        });
                });
        } catch (error) {
            console.error(error);
        }
    });
}
```

getMyOwnerGroups: Returns the groups where the current user is an owner of.

```typescript
public getMyOwnerGroups(): Promise<any> {
    return new Promise<any>((resolve, reject) => {
        try {
            // Prepare the output array
            var m365groups: Array<IGroup> = new Array<IGroup>();

            this.context.msGraphClientFactory
                .getClient()
                .then((client: MSGraphClient) => {
                    client
                        .api("/me/ownedObjects/$/microsoft.graph.group")
                        .get((error: any, groups: any, rawResponse: any) => {
                            // Map the response to the output array
                            groups.value.map((item: any) => {
                                if (item.groupTypes.indexOf('Unified') > -1) {
                                    m365groups.push({
                                        id: item.id,
                                        displayName: item.displayName,
                                        description: item.description,
                                        visibility: item.visibility,
                                        teamsConnected: item.resourceProvisioningOptions.indexOf("Team") > -1 ? true : false
                                    });
                                }
                            });

                            resolve(m365groups);
                        });
                });
        } catch (error) {
            console.error(error);
        }
    });
}
```

## ACE Class

The file `src\adaptiveCardExtensions\myM365Groups\MyM365GroupsAdaptiveCardExtension.ts` contains the definition of an ACE, which extends from the `BaseAdaptiveCardExtension` class.

In the `OnInit`, we will populate the state with group information where the current user is a member and owner of.

```typescript
public async onInit(): Promise<void> {
    this.state = {
      selectedGroupType: "",
      ownerGroups: [],
      memberGroups: []
    };

    this.cardNavigator.register(CARD_VIEW_REGISTRY_ID, () => new CardView());
    this.quickViewNavigator.register(GROUPS_SUMMARY_VIEW_REGISTRY_ID, () => new GroupsSummaryView());
    this.quickViewNavigator.register(GROUPS_LISTING_VIEW_REGISTRY_ID, () => new GroupsListingView());

    M365GroupService.setup(this.context);

    let ownerGroups: IGroup[] = await M365GroupService.getMyOwnerGroups();
    let memberGroups: IGroup[] = await M365GroupService.getMyMemberGroups();

    this.setState({ ownerGroups, memberGroups });

    return Promise.resolve();
}
```

## Card View

The card view at `src\adaptiveCardExtensions\myM365Groups\cardView\CardView.ts` displays the group summary and a button to open a quick view.

```typescript
export class CardView extends BasePrimaryTextCardView<IMyM365GroupsAdaptiveCardExtensionProps, IMyM365GroupsAdaptiveCardExtensionState> {
  public get cardButtons(): [ICardButton] | [ICardButton, ICardButton] | undefined {
    return [
      {
        title: strings.QuickViewButton,
        action: {
          type: 'QuickView',
          parameters: {
            view: GROUPS_SUMMARY_VIEW_REGISTRY_ID
          }
        }
      }
    ];
  }

  public get data(): IPrimaryTextCardParameters {
    return {
      primaryText: `${this.state.ownerGroups.length + this.state.memberGroups.length} ${strings.PrimaryText}`,
      description: `${this.state.ownerGroups.length} owned, ${this.state.memberGroups.length} member groups`
    };
  }
}
```

## Quick Views

We will develop 2 quick views as follows:

**Groups Summary View**

Quick view at `src\adaptiveCardExtensions\myM365Groups\groupsSummaryView\GroupsSummaryView.ts`, displays the group count by ownerGroups and memberGroups.

```typescript
public get data(): IGroupsSummaryViewData {
    const { ownerGroups, memberGroups, selectedGroupType } = this.state;

    let groupTypes: any[] = [
      {
        id: "ownerGroups",
        title: "Group Owner",
        Count: ownerGroups.length
      },
      {
        id: "memberGroups",
        title: "Group Member",
        Count: memberGroups.length
      }
    ];

    return {
      groupTypes
    };
}
```

The `OnAction` method detects the type of group is clicked and calls the Groups Listing View.

```typescript
public onAction(action: IActionArguments): void {
    this.setState({ selectedGroupType: action?.id });
    this.quickViewNavigator.push(GROUPS_LISTING_VIEW_REGISTRY_ID);
}
```

The json template at `src\adaptiveCardExtensions\myM365Groups\groupsSummaryView\template\GroupsSummaryViewTemplate.json` displays the group summary as follows:

![](/media/2022-01-27-ace-m365-groups-membership/viewcard-m365groups-summary.png)

**Groups Listing View**

Quick view at `src\adaptiveCardExtensions\myM365Groups\groupsListingView\GroupsListingView.ts`, displays the groups based on the selection from groups summary view (e.g., owner or member groups)

```typescript
public get data(): IGroupsListingViewData {
    const { ownerGroups, memberGroups, selectedGroupType } = this.state;

    let groups: any[] = [];

    if (selectedGroupType === "ownerGroups") {
      groups = ownerGroups;
    }
    else if (selectedGroupType === "memberGroups") {
      groups = memberGroups;
    }

    return {
      groups: groups,
      selectedGroupType: ''
    };
}
```

The json template at `src\adaptiveCardExtensions\myM365Groups\groupsListingView\template\GroupsListingViewTemplate.json` displays the groups as follows:

![](/media/2022-01-27-ace-m365-groups-membership/viewcard-m365groups-details.png)

## Deployment

The deployment involves below steps:

**Prepare and deploy the solution package**

Follow below steps to prepare and deploy the solution package to SharePoint tenant app catalog:

1. Prepare the solution package by running the commands

    ```
    gulp bundle --ship
    gulp package-solution --ship
    ```

2. Deploy the package from `sharepoint\solution\primaytextcard-my-m365-groups.sppkg` location to SharePoint tenant app catalog.

**Approve Graph API permissions**

After deploying the solution to tenant app catalog, SharePoint administrator needs to approve the Graph API permissions from SharePoint admin center > Advanced > API access blade.

![](/media/2022-01-27-ace-m365-groups-membership/approve-graph-api-permissions.png)

**Add ACE on SharePoint page**

Edit any SharePoint page, Add the ACE named **My M365 Groups** to the page.

## Summary

SPFx v1.13 supports Adaptive Card Extensions (ACEs) component type which enables developers to build a rich experience for Viva Connections' Dashboards and SharePoint Pages. This article demonstrates to build an ACE component to display the M365 (Unified) groups in a tenant, of which user is owner or member.

## Code Download
The ACE sample is available under PnP Repository for the Microsoft Viva Connections Adaptive Card sample solutions at: [https://github.com/pnp/sp-dev-fx-aces/tree/main/samples/PrimaryTextCard-My-M365-Groups](https://github.com/pnp/sp-dev-fx-aces/tree/main/samples/PrimaryTextCard-My-M365-Groups)

## References

- [Getting started with SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant?WT.mc_id=M365-MVP-5003693?WT.mc_id=M365-MVP-5003693)
- [Build your first Adaptive Card Extension](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/build-first-sharepoint-adaptive-card-extension?WT.mc_id=M365-MVP-5003693)
- [Building for Microsoft teams](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/build-for-teams-overview?WT.mc_id=M365-MVP-5003693)
- [Use Microsoft Graph in your solution](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/get-started/using-microsoft-graph-apis?WT.mc_id=M365-MVP-5003693)
- [Publish SharePoint Framework applications to the Marketplace](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/publish-to-marketplace-overview?WT.mc_id=M365-MVP-5003693)
