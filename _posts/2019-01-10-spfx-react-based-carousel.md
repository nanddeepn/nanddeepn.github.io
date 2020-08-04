---
title: "SharePoint Framework - React based Carousel"
date: "2019-01-10"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-01-10-spfx-react-based-carousel/04.png
  teaser: media/2019-01-10-spfx-react-based-carousel/04.png
tags:
  - "2019"
  - January 2019
last_modified_at: 2019-01-10T00:00:00-00:00
---

## Overview

SharePoint portals have many widgets or technically web parts displaying content to the users. They are either out of the box web parts configured to show the content or custom developed web parts. Carousel is one of such commonly used web part. The Carousel web part often scrolls the images in an infinite loop and allow users to scroll though. Often Carousels are used to rotate the news or announcements on a home page of SharePoint portal. The modern SharePoint does not offer any Carousel web part out of the box, however we can use available npm package for implementing Carousel.

In this article, we will explore npm package to help represent the Carousel in SPFx webpart. We will use React JS in this example.


## SharePoint Framework Version and NPM Packages Installed

For this article, I am using SharePoint Framework Version 1.7.1

```
npm view @microsoft/generator-sharepoint
```

![](/media/2019-01-10-spfx-react-based-carousel/01.png)

To see the installed npm packages, use below command:

```
npm list-g --depth 0
```


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-react-carousel
    ```

2. Navigate to above created directory.

    ```
    cd spfx-react-carousel
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-01-10-spfx-react-based-carousel/02.png)

    - **Solution Name:** Hit enter to have default name (spfx-react-carousel in this case) or type in any other name for your solution.
        - Selected choice: Hit enter
    - **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
        - Selected choice: SharePoint Online only (latest)
    - **Place of files:** We may choose to use the current folder or create a subfolder for our solution.
        - Selected choice: Use the current folder
    - **Deployment option:** We may choose to allow the tenant admin the choice of being able to deploy the solution to all sites immediately without running any feature deployment or adding apps in sites.
        - Selected choice: N (install on each site explicitly)
    - **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
        - Selected choice: WebPart
    - **Web part name:** Hit enter to select the default name or type in any other name.
        - Selected choice: ReactCarousel
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: React based Carousel
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: React

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## npm Packages

**Carousel**

We will use the npm package called as react-responsive-carousel ([https://www.npmjs.com/package/react-responsive-carousel](https://www.npmjs.com/package/react-responsive-carousel))

Use below command to install the Carousel:

```
npm install react-responsive-carousel --save
```

The ```--save``` option enables NPM to include the packages to dependencies section of package.json file.


## Code the webpart

1. Open ReactCarousel.tsx file under "\src\webparts\reactCarousel\components\" folder.
2. Import the Carousel control.

    ```typescript
    import { Carousel } from 'react-responsive-carousel';
    ```

3. Include the styles for Carousel.

    ```typescript
    import "react-responsive-carousel/lib/styles/carousel.min.css";
    ```


## Define State

1. Create a new file IReactCarouselState.ts under "\src\webparts\reactCarousel\components\" folder.

    ```typescript
    export interface IReactCarouselState {    
        imageURLs: string[];    
    }
    ```

2. Update our component "\src\webparts\reactCarousel\components\ReactCarousel.tsx" to use the state.

    ```typescript
    export default class ReactCarousel extends React.Component<IReactCarouselProps, IReactCarouselState> {    
      public constructor(props: IReactCarouselProps, state: IReactCarouselState) {    
        super(props);    
        
        this.state = {    
          imageURLs: []    
        };    
      }    
    }
    ```
 

## Implement Service

Let us implement service to fetch the image urls to display in carousel

1. Create **services** folder under **src** folder.
2. Add a file *IDataService.ts* under **services** folder.

    ```typescript
    export interface IDataService {    
        getImages: (listName?: string) => Promise<any>;    
    }
    ```

3. Add a file *ImageService.ts* under **services** folder.

    ```typescript
    import { ServiceScope, ServiceKey } from "@microsoft/sp-core-library";    
    import { IDataService } from './IDataService';    
    import { SPHttpClient, SPHttpClientResponse } from '@microsoft/sp-http';    
    import { PageContext } from '@microsoft/sp-page-context';    
    import { ICarouselImage } from './ICarouselImage';    
        
    export class ImageService implements IDataService {    
        public static readonly serviceKey: ServiceKey<IDataService> = ServiceKey.create<IDataService>('carousel:data-service', ImageService);    
        private _spHttpClient: SPHttpClient;    
        private _pageContext: PageContext;    
        private _currentWebUrl: string;    
        
        constructor(serviceScope: ServiceScope) {    
            serviceScope.whenFinished(() => {    
                // Configure the required dependencies      
                this._spHttpClient = serviceScope.consume(SPHttpClient.serviceKey);    
                this._pageContext = serviceScope.consume(PageContext.serviceKey);    
                this._currentWebUrl = this._pageContext.web.absoluteUrl;    
            });    
        }    
        
        public getImages(listName?: string): Promise<string[]> {    
            var images: string[] = [];    
            return new Promise<string[]>((resolve: (itemId: string[]) => void, reject: (error: any) => void): void => {    
                this.readImages(listName)    
                    .then((carouselItems: ICarouselImage[]): void => {    
                        var i: number = 0;    
                        for (i = 0; i < carouselItems.length; i++) {    
                            images.push(this._currentWebUrl + carouselItems[i].FileRef);    
                        }    
                        resolve(images);    
                    });    
            });    
        }    
        
        private readImages(listName: string): Promise<ICarouselImage[]> {    
            return new Promise<ICarouselImage[]>((resolve: (itemId: ICarouselImage[]) => void, reject: (error: any) => void): void => {    
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
            });    
        }    
    }
    ```


## Update Web Part class to consume Service

1. Open ReactCarousel.tsx file under "\src\webparts\reactCarousel\components\" folder.
2. Update the class to consume the implemented service.

    ```typescript
    import * as React from 'react';    
    import styles from './ReactCarousel.module.scss';    
    import { IReactCarouselProps } from './IReactCarouselProps';    
    import { escape } from '@microsoft/sp-lodash-subset';    
    import { Carousel } from 'react-responsive-carousel';    
    import "react-responsive-carousel/lib/styles/carousel.min.css";    
    import { IReactCarouselState } from './IReactCarouselState';    
        
    import { ServiceScope } from '@microsoft/sp-core-library';    
    import { ImageService } from '../../../services/ImageService';    
    import { IDataService } from '../../../services/IDataService';    
        
    export default class ReactCarousel extends React.Component<IReactCarouselProps, IReactCarouselState> {    
      private dataCenterServiceInstance: IDataService;    
        
      public constructor(props: IReactCarouselProps, state: IReactCarouselState) {    
        super(props);    
        
        this.state = {    
          imageURLs: []    
        };    
        
        let serviceScope: ServiceScope = this.props.serviceScope;    
        this.dataCenterServiceInstance = serviceScope.consume(ImageService.serviceKey);    
        
        this.dataCenterServiceInstance.getImages('Site Collection Images').then((carouselItems: any) => {    
          this.setState({    
            imageURLs: carouselItems    
          });    
        });    
      }    
        
      public render(): React.ReactElement<IReactCarouselProps> {    
        return (    
          <div className={styles.reactCarousel}>    
            <div className={styles.container}>    
              <div className={styles.row}>    
                <div className={styles.column}>    
                  <span className={styles.title}>Welcome to SharePoint!</span>    
                  <p className={styles.subTitle}>React based Carousel</p>    
                  <p className={styles.description}>{escape(this.props.description)}</p>    
        
                  <Carousel showThumbs={false} >    
                    {this.state.imageURLs.map((imageList) => {    
                      return (<div>    
                        <img src={imageList} />    
                      </div>)    
                    })}    
                  </Carousel>    
        
                </div>    
              </div>    
            </div>    
          </div>    
        );    
      }    
    }    
    ```
 

## Test the WebPart

1. On the command prompt, type **gulp serve**.
2. Open SharePoint site
3. Navigate to /_layouts/15/workbench.aspx
4. Locate and add the webpart to page.

    ![](/media/2019-01-10-spfx-react-based-carousel/03.png)

5. Verify the Carousel is scrolling through images

    ![](/media/2019-01-10-spfx-react-based-carousel/04.png)


## Summary

Carousel is widely used functionality in SharePoint world. Modern SharePoint does not provide Carousel as ready web part to use, however we can utilize any third party controls to meet the business needs.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-react-based-carousel2/).
