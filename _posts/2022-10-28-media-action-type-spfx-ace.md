---
title: "Using media Action Types in SPFx ACEs"
date: "2022-10-28"
share: true
header:
  image: media/2022-10-28-media-action-type-spfx-ace/02.png
  teaser: media/2022-10-28-media-action-type-spfx-ace/02.png
categories:
  - SharePoint Framework
tags:
  - "2022"
  - August 2022
last_modified_at: 2022-10-28T00:00:00-00:00
---
## Overview

SPFx v1.15 has introduced new action types for media to use in Adaptive Card Extensions to select Images from your native device.

In this article, we will get introduced to using these new media action types.


## Scaffold SPFx ACE solution

Let us start by scaffolding the SPFx ACE solution to get familiar with new media action type.

![](/media/2022-10-28-media-action-type-spfx-ace/01.png)


## Install packages

Install the PnP/PnPJS package using below command

```powershell
npm install @pnp/sp --save
```

This package will help us to upload a file to SharePoint library.


## Create a File Upload Service

Create a folder/file structure at `/src/services/FileUploadService.ts` for file upload operation.

```typescript
import { AdaptiveCardExtensionContext } from "@microsoft/sp-adaptive-card-extension-base";
import { spfi, SPFx, SPFI } from "@pnp/sp";
import "@pnp/sp/webs";
import "@pnp/sp/files";
import "@pnp/sp/folders";

export class FileUploadService {
    private _sp: SPFI;
    public context: AdaptiveCardExtensionContext;

    public setup(context: AdaptiveCardExtensionContext): void {
        if (!this._sp && typeof context !== "undefined") {
            this._sp = spfi().using(SPFx(context));
            this.context = context;
        }
    }

    public async UploadFile(fileName: string, content: string): Promise<void> {
        const base64Response = await fetch(content);
        const myblob = await base64Response.blob();

        await this._sp.web
            .getFolderByServerRelativePath("Shared Documents")
            .files
            .addChunked(fileName, myblob, undefined, true);
    }
}

const FileService: FileUploadService = new FileUploadService();
export default FileService;
```

## Define quick view

Update the Quick View template in a file `src\adaptiveCardExtensions\mediaActionType\quickView\QuickView.ts` as follows:

```typescript
public get template(): ISPFxAdaptiveCard {
    return {
        body:[
            {
                "type": "TextBlock",
                "weight": "Bolder",
                "text": "${filesUploaded}"
            }
        ],
        actions: [
            {
                id: 'upload image',
                title: 'Upload an image',
                type: 'VivaAction.SelectMedia',
                parameters: {
                    mediaType: MediaType.Image,
                    allowMultipleCapture: false
                }
            }
        ]
    }
}
```

In the `onAction` method, handle the file upload event to get the file uploaded to the SharePoint.

```typescript
public onAction(action: IActionArguments): void {
    if (action.type === 'VivaAction.SelectMedia') {
        // media is an array of attachment objects which contain the content and filename
        action.media.map(async attachment => {
            await FileUploadService.UploadFile(attachment.fileName, attachment.content);
        });
    }
  }
```

The ACE looks as follows:

![](/media/2022-10-28-media-action-type-spfx-ace/02.png)


## Summary

The media action type is useful in Adaptive Card Extensions for the frontline workers to upload the images on the go.


## Code Download

The SPFx code developed for this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/SPFx/ACEs/media-action-type).


## References

- [Create an Adaptive Card Extension with select media action](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/viva/get-started/actions/media-upload/mediauploadtutorial?WT.mc_id=M365-MVP-5003693)
- [@pnp/sp/files](https://pnp.github.io/pnpjs/sp/files/#adding-files)
