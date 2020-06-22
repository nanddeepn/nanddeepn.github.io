---
title: "Handle Navigation in SharePoint Framework Application Customizer"
date: "2020-04-10"
---

## Overview

The modern SharePoint supports partial loading, which significantly improves the SharePoint page performance. However, this behavior causes some issues while working with application customizer. As the page loads partially, an Application customizer faces a challenge to recognize this. Any functionality depends on the page properties (e.g. read the page url, querystring parameter change, etc.)

In this article, we will have a look at the issue and possible options to overcome this.

## Develop Application Customizer

On the command prompt, run the Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

Create an application customizer with below options:

- Provide solution name as **spfx-applicationextension-handle-navigation**.
- Select **SharePoint Online only (latest)**, and then select Enter.
- Select **Use the current folder** as the location for the files.
- Select **N** to allow the solution to be deployed to all sites immediately.
- Select **N** on the question if the solution contains unique permissions.
- Select **Extension** as the client-side component type to be created.
- Select **Application Customizer** as the extension type to be created.
- Name the extension as **HandleNavigation**.
- Provide the description as **Handle page navigation**.

### Implement React Component

We will create React component to display the page url on application customizer. Follow below steps to add component called “Header.tsx”.

import \* as React from 'react';
import styles from './Header.module.scss';
import { ApplicationCustomizerContext } from "@microsoft/sp-application-base";

export interface IHeaderProps {
    context: ApplicationCustomizerContext;
}

export class Header extends React.Component<IHeaderProps, {}> {
    constructor(props: IHeaderProps) {
        super(props);

        this.state = {
            panelOpen: false
        };
    }

    public componentWillUnmount() {
        console.log('Unmounting the header component.');
    }

    public render(): React.ReactElement<IHeaderProps> {
        return (
            <div className={styles.header}>
                <span>Current page:{this.props.context.pageContext.legacyPageContext.serverRequestPath}</span>
            </div>
        );
    }
}

Load React Component inside application customizer

Update the application customizer at “src\\extensions\\handleNavigation\\HandleNavigationApplicationCustomizer.ts” to load the React component.

Add below imports:

import \* as React from 'react';
import \* as ReactDom from 'react-dom';
import { override } from '@microsoft/decorators';
import { Log } from '@microsoft/sp-core-library';
import { BaseApplicationCustomizer, PlaceholderContent, PlaceholderName } from '@microsoft/sp-application-base';
import \* as strings from 'HandleNavigationApplicationCustomizerStrings';
import { IHeaderProps, Header } from './components/header';

Update the class “HandleNavigationApplicationCustomizer” to load the React component.

export default class HandleNavigationApplicationCustomizer
  extends BaseApplicationCustomizer<IHandleNavigationApplicationCustomizerProperties> {
  private static headerPlaceholder: PlaceholderContent;

  @override
  public onInit(): Promise<void> {
    Log.info(LOG\_SOURCE, \`Initialized ${strings.Title}\`);

    this.render();

    return Promise.resolve();
  }

  public onDispose() {
    if (HandleNavigationApplicationCustomizer.headerPlaceholder && HandleNavigationApplicationCustomizer.headerPlaceholder.domElement) {
      ReactDom.unmountComponentAtNode(HandleNavigationApplicationCustomizer.headerPlaceholder.domElement);
    }
  }

  private render() {
    if (this.context.placeholderProvider.placeholderNames.indexOf(PlaceholderName.Top) !== -1) {
      if (!HandleNavigationApplicationCustomizer.headerPlaceholder || !HandleNavigationApplicationCustomizer.headerPlaceholder.domElement) {
        HandleNavigationApplicationCustomizer.headerPlaceholder = this.context.placeholderProvider.tryCreateContent(PlaceholderName.Top, {
          onDispose: this.onDispose
        });
      }

      this.loadReactComponent();
    }
    else {
      console.log(\`The following placeholder names are available\`, this.context.placeholderProvider.placeholderNames);
    }
  }

  /\*\*
   \* Start the React rendering of your components
   \*/
  private loadReactComponent() {
    if (HandleNavigationApplicationCustomizer.headerPlaceholder && HandleNavigationApplicationCustomizer.headerPlaceholder.domElement) {
      const element: React.ReactElement<IHeaderProps> = React.createElement(Header, {
        context: this.context
      });

      ReactDom.render(element, HandleNavigationApplicationCustomizer.headerPlaceholder.domElement);
    }
    else {
      console.log('DOM element of the header is undefined. Start to re-render.');
      this.render();
    }
  }
}

Deploy an Application Customizer to SharePoint

Deploy the application customizer to SharePoint site by following below steps:

On the command prompt, execute below commands:

1. Generate bundles in the dist folder.

gulp bundle --ship

1. Generate .sppkg file to be deployed in the SharePoint app catalog.

gulp package-solution --ship

1. Deploy the application customizer app to the SharePoint site.

## The Navigation Issue

To understand the issue with navigation lets generate below scenario:

1. Create few pages in SharePoint site.
2. Add “Quick links” web parts on each page with links to other pages.
3. Click on the links and observe the url in application customizer.
4. The URL does not change when we navigate through pages.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/01-navigation-issue.gif)

The reason is, component inside application customizer does not get an update as the page loads partially.

## The Solution (_navigatedEvent)_

As a workaround, we can add _navigatedEvent_ listener inside onInit function which will re-render our component every time we navigate to another page.

This event is fired when the application's top-level page context navigates to a new page.

public onInit(): Promise<void> {
  Log.info(LOG\_SOURCE, \`Initialized ${strings.Title}\`);

  this.context.application.navigatedEvent.add(this, () => {
    this.loadReactComponent();
  });

  this.render();

  return Promise.resolve();
}

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/04/02-navigation-resolution.gif)

## Summary

Modern SharePoint supports partial rendering. The component inside application customizer does not get an update as the page loads partially. Adding _navigatedEvent_ listener inside onInit function re-render our component every time we navigate to another page.

## Code Download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/SPFx/Extensions/spfx-applicationextension-preallocate-space](https://github.com/nanddeepn/code-samples/tree/master/SPFx/Extensions/spfx-applicationextension-preallocate-space)
