---
title: "Batch Graph APIs requests in SPFx Solution"
date: "2022-05-16"
share: true
header:
  image: media/2022-05-16-batch-graph-requests-spfx/02.png
  teaser: media/2022-05-16-batch-graph-requests-spfx/02.png
categories:
  - SharePoint
  - SharePoint Framework
tags:
  - "2022"
  - May 2022
last_modified_at: 2022-05-16T00:00:00-00:00
---
## Overview

SharePoint Framework supports developing solutions for modern SharePoint, Microsoft Teams, Viva Connections - Adaptive Card Extensions. One of the common scenarios is to consume the Graph APIs in SPFx solution.

In this article, we will explore one of such common scenarios of adding multiple owners to Microsoft 365 Group using batch request in Graph API.

## SPFx Solution

Once the SPFx solution is scaffolded using `yo @microsoft/sharepoint` command, let us add the PnP People Picker control with below command:

```powershell
npm install @pnp/spfx-controls-react --save --save-exact
```

The PnP people picker control allows to select one or more users from SharePoint group or site.

Import the control to your React component as follows:

```typescript
import { PeoplePicker, PrincipalType } from '@pnp/spfx-controls-react/lib/PeoplePicker';
```

Add the People Picker control to the render method as follows:

```typescript
<PeoplePicker
    context={this.props.context}
    titleText={strings.AddOwners}
    groupName={""} // Leave this blank in case you want to filter from all users    
    showtooltip={true}
    required={true}
    ensureUser={true}
    personSelectionLimit={5}
    onChange={this._getPeoplePickerItems}
    showHiddenInUI={false}
    principalTypes={[PrincipalType.User]}
    resolveDelay={1000} />
```

The People Picker control now allows selecting up to 5 users (as specified in the `personSelectionLimit` property).

![](/media/2022-05-16-batch-graph-requests-spfx/01.png)

The `onChange` property can get the selected People in the PeoplePicker.

```typescript
private _getPeoplePickerItems(items: any[]) {
    this.setState({ selectedOwners: items });
}
```

We can have a button click event to process the selected users as follows:

```typescript
<PrimaryButton onClick={this._addOwners} text={strings.OK} />

    .
    .
    .

private async _addOwners() {
    for (let i = 0; i < this.state.selectedOwners.length; i++) {
        await addOwners(groupId, this.state.selectedOwners).then(response => {
            console.log('Owners added to the group');
        });
    }

    this.props.onDissmissPanel(true);
}
```

## Graph API

Below Graph API add user to M365 group with delegated or application permission of `Group.ReadWrite.All` or `Directory.ReadWrite.All`.

```json
POST https://graph.microsoft.com/v1.0/groups/{id}/owners/$ref
Content-type: application/json

{
    '@odata.id': 'https://graph.microsoft.com/v1.0/users/{id}'
}
```

The limitation of this approach is that we can specify only one user at a time to get added as owner to the M365 group.

To overcome this limitation, we can batch the multiple requests together to get added multiple users as owner to M365 group in a single request.

JSON batching allows to combine multiple requests (up to 20) in a single JSON object. The JSON can be constructed as follows:

```json
POST https://graph.microsoft.com/v1.0/$batch
Accept: application/json
Content-Type: application/json

{
    'requests': [
        {
            'id': '1',
            'method': 'GET',
            'url': '/me'
        },
        {
            'id': '2',
            'method': 'GET',
            'url': '/me/joinedTeams'
        },
        {
            'id': '3',
            'method': 'GET',
            'url': '/groups/{group-id}/members?$count=true'
        }
    ]
}
```

The above JSON object helps to get below information from multiple sources:

1. My profile
2. My joined teams
3. Direct members of Group

![](/media/2022-05-16-batch-graph-requests-spfx/02.png)

We will use the same approach to combine multiple requests to add user as an owner to M365 group as follows:

```typescript
public addOwners(groupId: string, userIds: any[]): Promise<any> {
    let jsonOwners: string = "";

    for (let i = 0; i < userIds.length; i++) {
      let jsonEntry = `{
          "id": "${i + 1}",
          "method": "POST",
          "url": "/groups/${groupId}/owners/$ref",
          "body": {
              "@odata.id": "https://graph.microsoft.com/v1.0/users/${userIds[i].secondaryText}"
          },
          "headers": {
              "Content-Type": "application/json"
          }
      },`;

      jsonOwners += jsonEntry;
    }

    // remove last comma
    jsonOwners = jsonOwners.replace(/,\s*$/, "");

    return new Promise<void>((resolve, reject) => {
      this.context.msGraphClientFactory
        .getClient()
        .then((client: MSGraphClient) => {
          client
            .api(`/$batch`)
            .post(`{ "requests": [ ${jsonOwners} ]}`)
            .then((addOwnerResponse) => {
              if (addOwnerResponse.responses.length === userIds.length) {
                resolve();
              }
              else {
                throw new Error(`Error occured while adding owner to the Group`);
              }
            });
        });
    });
}
```

## References

- [PnP People Picker control](https://pnp.github.io/sp-dev-fx-controls-react/controls/PeoplePicker/)
- [Graph API - Add owners to the Group](https://docs.microsoft.com/en-us/graph/api/group-post-owners?WT.mc_id=M365-MVP-5003693)
- [Combine multiple requests in one HTTP call using JSON batching](https://docs.microsoft.com/en-us/graph/json-batching?WT.mc_id=M365-MVP-5003693)
