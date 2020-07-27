---
title: "Rich Text Control in SharePoint Framework with react-draft-wysiwyg"
date: "2020-03-19"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2020-03-19-rich-text-control-in-spfx/02.gif
  teaser: media/2020-03-19-rich-text-control-in-spfx/02.gif
tags:
  - "2020"
  - March 2020
last_modified_at: 2020-03-19T00:00:00-00:00
---

## Overview

Rich text controls offer rich formatting of text content. The ribbon provides WYSIWYG (What You See Is What You Get) editor experience. SharePoint supports Rich text fields, however providing similar experience from custom components like SharePoint Framework web parts involves some efforts, as we do not have any out of the box or Office UI Fabric control to support this scenario.

In this article, we will explore the approach to implement Rich text control in SPFx using npm package “react-draft-wysiwyg”.

## Develop SPFx Web Part for Rich Text Control

Follow the below steps to develop the SPFx solution.

1. Open a command prompt. Create a directory for the SPFx solution.

    ```
    md spfx-richtextcontrol
    ```

2. Navigate to the directory.

    ```
    cd spfx-richtextcontrol
    ```

3. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2020-03-19-rich-text-control-in-spfx/01.png)

    When prompted:

    - Accept the default **spfx-richtextcontrol** as your solution name, and then select Enter.
    - Select **SharePoint Online only (latest)**, and then select Enter.
    - Select **Use the current folder** as the location for the files.
    - Select **N** to allow the solution to be deployed to all sites immediately.
    - Select **N** on the question if the solution contains unique permissions.
    - Select **WebPart** as the client-side component type to be created.
    - Name the extension as **RichTextControl**.
    - Provide the description as **Rich Text Control for SPFx**.
    - Select the framework to use as **React**.

5. Yeoman generator will perform the scaffolding process to generate the solution. The scaffolding process will take a significant amount of time.
6. Once the scaffolding process is completed, lock down the version of the project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in the code editor of your choice.

```
code .
```

## SharePoint List

Create a supporting SharePoint list named “RichText” to implement the scenario with the below schema:

**Column**|**Type**|**Comments**
Title|Single line of text|OOB column
Description|Multiple lines of text|Enhanced rich text (Rich text with pictures, tables, and hyperlinks)

## NPM Package for Rich Text

**react-draft-wysiwyg** ([https://www.npmjs.com/package/react-draft-wysiwyg](https://www.npmjs.com/package/react-draft-wysiwyg))

This npm package supports editor built using ReactJS and extensible DraftJS libraries. DraftJS is a JavaScript rich text editor framework.

Use the below command to install the package.

```
npm install --save react-draft-wysiwyg draft-js
npm install --save draftjs-to-html
```

Before we start using the library, let's understand a few terms:

- **EditorState**: is the top-level state object for the editor.
- **draftToHtml**: converts DraftJS Editor content to plain HTML.
- **htmlToDraft**: converts plain HTML to DraftJS Editor content.

**@pnp/sp** ([https://www.npmjs.com/package/@pnp/sp](https://www.npmjs.com/package/@pnp/sp))
SharePoint Patterns and Practices client side library for SharePoint operations.
```
npm install --save @pnp/sp
```

Follow the below steps to configure our web part to use PnP JS V2:

1. Open web part file “src\\webparts\\richTextControl\\RichTextControlWebPart.ts”.
2. Add below import.

    ```typescript
    import { sp } from "@pnp/sp/presets/all";
    ```

3. Implement onInit method to initialize the context.
```typescript
protected onInit(): Promise<void> {
  return super.onInit().then(\_ => {
    sp.setup({
      spfxContext: this.context
    });
  });
}
```

## Define State

Define a state to represent an item in the “RichText” SharePoint list.

```typescript
export interface IRichTextControlState {
    ID: number;
    Title: string;
    Description?: any;
    editorState?: any;
}
```

## Use the Rich Text in SPFx Solution

Follow the below steps to use rich text:

1. Open file "src\\webparts\\richTextControl\\components\\RichTextControl.tsx".
2. Include below imports:

```typescript
import { EditorState, convertToRaw, ContentState } from 'draft-js';
import { Editor } from 'react-draft-wysiwyg';
import draftToHtml from 'draftjs-to-html';
import htmlToDraft from 'html-to-draftjs';
import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css';

import { sp } from "@pnp/sp/presets/all";
```


**Implement New Item Operation**

We will implement a scenario to add rich text to a SharePoint list.

1. Add the controls to render method to accept title and description from end user.

    ```typescript
    public render(): React.ReactElement<IRichTextControlProps> {
      const { Title, editorState } = this.state;

      return (
        <div className={styles.richTextControl}>
          <div className={styles.container}>
            <div className={styles.row}>
              <div className={styles.column}>
                <TextField
                  label="Title"
                  required
                  value={Title}
                  onChange={this.onTitleChange}
                />
              </div>
            </div>

            <div className={styles.row}>
              <div className={styles.column}>
                <Label>Description</Label>
                <Editor
                  editorState={editorState}
                  onEditorStateChange={this.onEditorStateChange}
                />
              </div>
            </div>

            <div className={styles.row}>
              <div className={styles.column}>
                <PrimaryButton
                  onClick={this.onSave}
                  style={{ marginBottom: '15px', marginRight: '8px', float: 'right' }}>
                  Save
                </PrimaryButton>
              </div>
            </div>
          </div>
        </div>
      );
    }
    ```

2. Set or update the state as user types in the input.

    ```typescript
    private onTitleChange = (ev: any, newText: string): void => {
      this.setState({ Title: newText });
    }

    private onEditorStateChange(editorState) {
      this.setState({
        editorState
      });
    }
    ```

2. On the **Save** button click, add an item to the SharePoint list.

```typescript
private async onSave() {
    await sp.web.lists.getByTitle('RichText').items.add({
      Title: this.state.Title,
      Description: draftToHtml(convertToRaw(this.state.editorState.getCurrentContent()))
    });
}
```


**Implement Read Item Operation**

We will implement a scenario to read rich text from the SharePoint list. To keep it simple, we will read the first item from the list.

1. In the Render method, add a button to read the item.

    ```typescript
    <PrimaryButton
      onClick={this.onRead}
      style={{ marginBottom: '15px', marginRight: '8px', float: 'right' }}>
      Read
    </PrimaryButton>
    ```

2. Implement the _onRead_ method to read the first item from the SharePoint list.

```typescript
private async onRead() {
    const richTextItem = await sp.web.lists.getByTitle('RichText').items.getById(1)
    .select("ID", "Title", "Description")
    .get();

    let editorState: EditorState;

    // Get Description and covert to RichText Control
    const html = richTextItem.Description;
    const contentBlock = htmlToDraft(html);

    if (contentBlock) {
      const contentState = ContentState.createFromBlockArray(contentBlock.contentBlocks);
      editorState = EditorState.createWithContent(contentState);
    }

    this.setState({
      ID: richTextItem.ID,
      Title: richTextItem.Title,
      Description: richTextItem.Description,
      editorState: editorState
    });
}
```


**Implement Update Item Operation**

We will implement a scenario to update the first item with rich text from the SharePoint list. To keep it simple, we will update the first item from the list.

1. In the _Render_ method, add a button to update the item.

    ```typescript
    <PrimaryButton
      onClick={this.onUpdate}
      style={{ marginBottom: '15px', marginRight: '8px', float: 'right' }}>
      Update
    </PrimaryButton>
    ```

2. Implement the _onUpdate_ method to update the first item in the SharePoint list.

```typescript
private async onUpdate() {
  let newItem: any = {
    Title: this.state.Title,
    Description: draftToHtml(convertToRaw(this.state.editorState.getCurrentContent()))
  }

  await sp.web.lists.getByTitle('RichText').items
    .getById(1)
    .update(newItem);
}
```

## Rich Text Control in action
![](/media/2020-03-19-rich-text-control-in-spfx/02.gif)


## Summary

In this article, we explored the approach to implement Rich text control in SPFx using npm package “react-draft-wysiwyg” to support rich formatting of text content in SharePoint Framework web parts.

## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-richtextcontrol](https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-richtextcontrol)
