---
title: "Dynamically Load SPFx Library Components"
date: "2019-10-06"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-10-06-dynamically-load-spfx-library-components/03.png
  teaser: media/2019-10-06-dynamically-load-spfx-library-components/03.png
tags:
  - "2019"
  - October 2019
last_modified_at: 2019-10-06T00:00:00-00:00
---

## Overview

Library components are generally available with SPFx v1.9.1 release. The library component helps to build independently versioned and deployed code. It then can be consumed by various SPFx web parts or extensions.

In this article, we will explore how to dynamically load the library component and the benefits of dynamic loading.


## Why load the Library component dynamically?

Here are a couple of points to set the stage.

1. You import the library component to your SPFx web part or extension, but the call to library component methods never happens.
2. When building SPFx components, any library you add to your project will be downloaded on the page you use the web part or extension on (irrespective of it is being called or not).

Importing the library component dynamically in the SPFx solution helps to handle above situations gracefully.


## Implement Library Component

Follow the below steps to create a library component named spfx-library.

1. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

2. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-10-06-dynamically-load-spfx-library-components/01.png)

3. To avoid localhost port conflicts on gulp serve, open config\\serve.json and set the port to any number other than default port 4321. For more information - [SharePoint Library Components – Simultaneous Parallel Development](http://warner.digital/sharepoint-library-components-simultaneous-parallel-development/)

    ```json
    {  
      "$schema": "https://developer.microsoft.com/json-schemas/core-build/serve.schema.json",  
      "port": 4322,  
      "https": true  
    }
    ```

4. On the command prompt, type below command to create a local npm link to the library.

    ```
    npm link
    ```

5. To run the library component (without opening a browser).

    ```
    gulp serve --nobrowser
    ```


## Implement Consumer Web Part

Let's implement a web part named spfx-library-consumer to consume the above library component.

1. Run the Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

2. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2019-10-06-dynamically-load-spfx-library-components/02.png)

3. On the command prompt, symlink a library component by running below command.

    ```
    npm link spfx-library
    ```

4. Type below command to open the solution in the code editor of your choice.

    ```
    code .
    ```

5. Open file "src\webparts\libraryConsumer\components\LibraryConsumer.tsx".
6. Import the library component.

    ```typescript
    import * as myLibrary from 'spfx-library';
    ```

7. Refer library inside render method.

    ```typescript
    public render(): React.ReactElement<ILibraryConsumerProps> {  
      const myInstance = new myLibrary.CommonLibraryLibrary();  
      
      return (  
        <div className={ styles.libraryConsumer }>  
          <div className={ styles.container }>  
            <div className={ styles.row }>  
              <div className={ styles.column }>  
                <span className={ styles.title }>Welcome to SharePoint!</span>  
                <p className={ styles.subTitle }>Customize SharePoint experiences using Web Parts.</p>  
                <p>My library: {myInstance.name()}</p>  
                <p className={ styles.description }>{escape(this.props.description)}</p>  
                <a href="https://aka.ms/spfx" className={ styles.button }>  
                  <span className={ styles.label }>Learn more</span>  
                </a>  
              </div>  
            </div>  
          </div>  
        </div>  
      );  
    }
    ```

8. On the command prompt, run the web part solution by executing the below command.

    ```
    gulp serve
    ```

9. The web part should consume the methods from the library component.

    ![](/media/2019-10-06-dynamically-load-spfx-library-components/03.png)

10. Open developer dashboard (F12) in the browser to see how the components are loaded.

![](/media/2019-10-06-dynamically-load-spfx-library-components/04.png)

This shows the library component is loaded by default.


## Load the Library Dynamically

Consider an example, where we would like to call a method from the library component on click of a button. In this situation, the library does not require to be loaded by default, rather it should be available only when needed (on click of button, in this scenario).

Let us analyze our library loading on the calling component.

Analyze the original import statement (as below).

```typescript
import * as myLibrary from 'spfx-library';
```

Let us try to be very specific about what we need from the library component. The CommonLibraryLibrary class contains the methods in library component. The above import statement will reduce to below.

```typescript
import CommonLibraryLibrary from 'spfx-library';
```

The old friend - webpackChunkName is helpful here, which will make sure anything in that tsx/ts file will be wrapped up in its own .js file, loaded when this line of code executes.

```typescript
import * as React from 'react';  
import styles from './LibraryConsumer.module.scss';  
import { ILibraryConsumerProps } from './ILibraryConsumerProps';  
import { escape } from '@microsoft/sp-lodash-subset';  
// import * as myLibrary from 'spfx-library';  
  
// Import Button component    
import { IButtonProps, DefaultButton } from 'office-ui-fabric-react/lib/Button';  
import { autobind } from 'office-ui-fabric-react';  
  
export default class LibraryConsumer extends React.Component<ILibraryConsumerProps, {}> {  
  public render(): React.ReactElement<ILibraryConsumerProps> {  
    // const myInstance = new myLibrary.CommonLibraryLibrary();  
  
    return (  
      <div className={ styles.libraryConsumer }>  
        <div className={ styles.container }>  
          <div className={ styles.row }>  
            <div className={ styles.column }>  
              <span className={ styles.title }>Welcome to SharePoint!</span>  
              <p className={ styles.subTitle }>Customize SharePoint experiences using Web Parts.</p>  
              {/* <p>My library: {myInstance.name()}</p> */}  
              <p className={ styles.description }>{escape(this.props.description)}</p>                
  
              <DefaultButton    
                  data-automation-id="search"    
                  target="_blank"    
                  title="Search"    
                  onClick={this._loadLibrary}    
                  >    
                  Load Library Component    
              </DefaultButton>  
            </div>  
          </div>  
        </div>  
      </div>  
    );  
  }  
  
  @autobind  
  private async _loadLibrary() {  
    const dynamicLibImport = await import(  
      /* webpackChunkName: 'CommonLibraryLibrary' */  
      'spfx-library'  
    );  
  
    const myInstance = new dynamicLibImport.CommonLibraryLibrary();  
    alert(myInstance.name());  
  }  
}
```


## Test the library component dynamic loading

1. On the command prompt, type "gulp serve" to run the web part.
2. In the browser, open the developer dashboard (F12) to observe the component loading.

    ![](/media/2019-10-06-dynamically-load-spfx-library-components/05.png)

3. The page will not load the library component on the initial load.
4. Click the button "Load Library Component". Observe the developer dashboard.

    ![](/media/2019-10-06-dynamically-load-spfx-library-components/06.png)

5. On the click of a button, the library component loads dynamically and executes the lines of code.


## Summary

The library component helps to build independently versioned and deployed code. webpackChunkName is helpful to make sure anything in that tsx/ts file will be wrapped up in its own .js file, loaded when this line of code executes. Importing the library component dynamically in SPFx solution helps to optimize the overall performance.

This content was originally posted [here](https://www.c-sharpcorner.com/article/dynamically-load-spfx-library-components/).
