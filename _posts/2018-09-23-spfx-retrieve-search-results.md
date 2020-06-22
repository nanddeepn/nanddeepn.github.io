---
title: "SharePoint Framework – Retrieve Search Results"
date: "2018-09-23"
---

## Overview

Search is an integral part of SharePoint over the years. Search helps us to get the security trimmed results across the tenant. Unfortunately modern SharePoint sites does not provide the search related web parts by default.

In this article, we will explore to query the Search REST API and get the results. We will use React JS in this example.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-react-search

1. Navigate to above created directory.

cd spfx-react-search

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-220.png)

- **Solution Name:** Hit enter to have default name (spfx-react-search in this case) or type in any other name for your solution.
    - Selected choice: Hit enter
- **Target for component:** Here we can select the target environment where we are planning to deploy the client webpart i.e. SharePoint Online or SharePoint OnPremise (SharePoint 2016 onwards).
    - Selected choice: SharePoint Online only (latest)
- **Place of files:** We may choose to use the same folder or create a subfolder for our solution.
    - Selected choice: Same folder
- **Deployment option:** Selecting Y will allow the app to deployed instantly to all sites and will be accessible everywhere.
    - Selected choice: N (install on each site explicitly)
- **Type of client-side component to create:** We can choose to create client side webpart or an extension. Choose webpart option.
    - Selected choice: WebPart
- **Web part name:** Hit enter to select the default name or type in any other name.
    - Selected choice: SearchResultsViewer
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Retrieve search results using REST API
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

 

Model for Search Results

We will define an interface for representing the SharePoint search results.

1. Add an interface (ISPSearchResult.ts) representing the SharePoint search result.

export interface ISPSearchResult {  
    Title: string;  
    Description: string;  
    Url: string  
}

1. React JS acts on the state change. Let us add state to our solution (ISearchResultsViewerState.ts)

import {ISPSearchResult} from './ISPSearchResult';  
  
export interface ISearchResultsViewerState {  
    status: string;  
    searchText: string;  
    items: ISPSearchResult\[\];  
}

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-221.png)

1. Configure SearchResultsViewer.tsx for this state

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-222.png)

## Add Controls to WebPart

1. Open SearchResultsViewer.tsx under “\\src\\webparts\\searchResultsViewer\\components\\” folder.
2. Modify Render method to include the required controls.

**Text field:**

// Import Textfield component   
import { TextField } from 'office-ui-fabric-react/lib/TextField';  
import './TextField.Examples.scss';  
  
<TextField     
    required={true}     
    name="txtSearchText"     
    placeholder="Search..."    
    value={this.state.searchText}    
    onChanged={e => this.setState({ searchText: e })}    
/>

**Button:**

// Import Button component    
import { IButtonProps, DefaultButton } from 'office-ui-fabric-react/lib/Button';  
  
<DefaultButton    
     data-automation-id="search"    
     target="\_blank"    
     title="Search"    
     onClick={this.\_searchClicked}    
     >    
     Search    
</DefaultButton>  
  
private \_searchClicked(): void {  
}

**List:**

// Import List component  
import { FocusZone, FocusZoneDirection } from 'office-ui-fabric-react/lib/FocusZone';  
import { List } from 'office-ui-fabric-react/lib/List';  
  
// Import Link component  
import { Link } from 'office-ui-fabric-react/lib/Link';  
  
<FocusZone direction={FocusZoneDirection.vertical}>  
    <div className="ms-ListGhostingExample-container" data-is-scrollable={true}>  
         <List items={this.state.items} onRenderCell={this.\_onRenderCell} />  
    </div>  
</FocusZone>  
  
private \_onRenderCell(item: ISPSearchResult, index: number, isScrolling: boolean): JSX.Element {  
    return (  
      <div className="ms-ListGhostingExample-itemCell" data-is-focusable={true}>  
        <div className="ms-ListGhostingExample-itemContent">  
          <div className="ms-ListGhostingExample-itemName">  
            <Link href={item.Url}>{item.Title}</Link>            
          </div>  
          <div className="ms-ListGhostingExample-itemName">{item.Description}</div>  
          <p></p>  
        </div>  
      </div>  
    );  
  }

1. On the command prompt type “gulp serve” to see the controls on webpart.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-223.png)

## Implement service to retrieve search results

1. Add a folder “services” to the solution.
2. To the “services” folder, add a file SearchService.ts
3. We will use the REST API to get the search results.

import { SPHttpClient, SPHttpClientResponse } from '@microsoft/sp-http';  
import { IWebPartContext } from '@microsoft/sp-webpart-base';  
import { ISPSearchResult } from '../components/ISPSearchResult';  
import { ISearchResults, ICells, ICellValue, ISearchResponse } from './ISearchService';  
import { escape } from '@microsoft/sp-lodash-subset';  
  
export default class SearchService {  
    constructor(private \_context: IWebPartContext) {  
    }  
  
    public getSearchResults(query: string): Promise<ISPSearchResult\[\]> {  
          
        let url: string = this.\_context.pageContext.web.absoluteUrl + "/\_api/search/query?querytext='" + query + "'";  
          
        return new Promise<ISPSearchResult\[\]>((resolve, reject) => {  
            // Do an Ajax call to receive the search results  
            this.\_getSearchData(url).then((res: ISearchResults) => {  
                let searchResp: ISPSearchResult\[\] = \[\];  
  
                // Check if there was an error  
                if (typeof res\["odata.error"\] !== "undefined") {  
                    if (typeof res\["odata.error"\]\["message"\] !== "undefined") {  
                        Promise.reject(res\["odata.error"\]\["message"\].value);  
                        return;  
                    }  
                }  
  
                if (!this.\_isNull(res)) {  
                    const fields: string = "Title,Path,Description";  
  
                    // Retrieve all the table rows  
                    if (typeof res.PrimaryQueryResult.RelevantResults.Table !== 'undefined') {  
                        if (typeof res.PrimaryQueryResult.RelevantResults.Table.Rows !== 'undefined') {  
                            searchResp = this.\_setSearchResults(res.PrimaryQueryResult.RelevantResults.Table.Rows, fields);  
                        }  
                    }  
                }  
  
                // Return the retrieved result set  
                resolve(searchResp);  
            });  
        });  
    }  
  
     /\*\* 
     \* Retrieve the results from the search API 
     \* 
     \* @param url 
     \*/  
    private \_getSearchData(url: string): Promise<ISearchResults> {  
        return this.\_context.spHttpClient.get(url, SPHttpClient.configurations.v1, {  
            headers: {  
                'odata-version': '3.0'  
            }  
        }).then((res: SPHttpClientResponse) => {  
            return res.json();  
        }).catch(error => {  
            return Promise.reject(JSON.stringify(error));  
        });  
    }  
  
    /\*\* 
     \* Set the current set of search results 
     \* 
     \* @param crntResults 
     \* @param fields 
     \*/  
    private \_setSearchResults(crntResults: ICells\[\], fields: string): any\[\] {  
        const temp: any\[\] = \[\];  
  
        if (crntResults.length > 0) {  
            const flds: string\[\] = fields.toLowerCase().split(',');  
  
            crntResults.forEach((result) => {  
                // Create a temp value  
                var val: Object = {}  
  
                result.Cells.forEach((cell: ICellValue) => {  
                    if (flds.indexOf(cell.Key.toLowerCase()) !== -1) {  
                        // Add key and value to temp value  
                        val\[cell.Key\] = cell.Value;  
                    }  
                });  
  
                // Push this to the temp array  
                temp.push(val);  
            });  
        }  
  
        return temp;  
    }  
  
    /\*\* 
     \* Check if the value is null or undefined 
     \* 
     \* @param value 
     \*/  
    private \_isNull(value: any): boolean {  
        return value === null || typeof value === "undefined";  
    }  
}

 

Test the WebPart

1. On the command prompt, type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Type the search text and verify the search results.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-224.png)

## Troubleshooting

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-225.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

## Summary

Modern SharePoint does not have web part for displaying search results. However using the Search REST API the results can be achieved.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-retrieve-search-results/).
