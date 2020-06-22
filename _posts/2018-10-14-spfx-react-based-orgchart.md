---
title: "SharePoint Framework – React based OrgChart from SharePoint list"
date: "2018-10-14"
---

## Overview

SharePoint Framework client web parts are targeted to develop the business scenarios. Office 365 UI fabric component offers seamless integration with Office 365 and offers wide range of UI components. However it does not offer Organization chart kind of controls yet. In these scenarios we can make use of open source npm packages offerings.

In this article, we will explore organization chart control. We will use React JS to develop the example.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-react-orgchart

1. Navigate to above created directory.

cd spfx-react-orgchart

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-233.png)

- **Solution Name:** Hit enter to have default name (spfx-react-orgchart in this case) or type in any other name for your solution.
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
    - Selected choice: OrgChartViewer
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Organization chart with React
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

 

Development Scenario

In this article, we will make use of SharePoint to store the hierarchical information for chart and render it on SPFx web part.

A SharePoint list (named OrgChart) is used to store the hierarchical data. The schema of list is as below:

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-234.png)

The Parent column is a lookup on same OrgChart list’s Title column.

Let’s add some test data to our list.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-235.png)

## NPM Packages

**Organization Chart Control**

At the time of writing this article organization chart control is not available in Office 365 UI Fabric controls. We will explore the org chart control called as react-orgchart ([https://www.npmjs.com/package/react-orgchart](https://www.npmjs.com/package/react-orgchart))

Use below command to install the org chart control.

npm install react-orgchart --save

The --save option enables NPM to include the packages to dependencies section of package.json file.

 

**Array to Tree**

The array-to-tree npm package helps to convert plain array of nodes (with pointers to parent nodes) to a nested data structure. ([https://www.npmjs.com/package/array-to-tree](https://www.npmjs.com/package/array-to-tree))

Use below command to install the array-to-tree npm package:

npm install array-to-tree --save

 

Code the webpart

1. Open OrgChartViewer.tsx file under “\\src\\webparts\\orgChartViewer\\components\\” folder to import the org chart control.

import OrgChart from 'react-orgchart';

1. Open OrgChartViewer.module.scss file under “\\src\\webparts\\orgChartViewer\\components\\” folder to import the CSS.

@import '~react-orgchart/index.css';

1. Modify the render method to render the tree view.

public render(): React.ReactElement<IOrgChartViewerProps> {  
  return (  
    <div className={ styles.orgChartViewer }>  
      <div className={ styles.container }>  
        <div className={ styles.row }>  
          <div className={ styles.column }>  
  
            <OrgChart tree={this.state.orgChartItems} NodeComponent={this.MyNodeComponent} />  
  
          </div>  
        </div>  
      </div>  
    </div>  
  );  
}  
  
private MyNodeComponent = ({ node }) => {  
  if (node.url) {  
    return (  
      <div className="initechNode">  
        <a href={ node.url.Url } className={styles.link} >{ node.title }</a>          
      </div>  
    );      
  }  
  else {  
    return (  
      <div className="initechNode">{ node.title }</div>  
    );      
  }      
}

1. Implement method to read items from SharePoint list - OrgChart.

private readOrgChartItems(): Promise<IOrgChartItem\[\]> {  
  return new Promise<IOrgChartItem\[\]>((resolve: (itemId: IOrgChartItem\[\]) => void, reject: (error: any) => void): void => {  
    this.props.spHttpClient.get(\`${this.props.siteUrl}/\_api/web/lists/getbytitle('${this.props.listName}')/items?$select=Title,Id,Url,Parent/Id,Parent/Title&$expand=Parent/Id&$orderby=Parent/Id asc\`,  
    SPHttpClient.configurations.v1,  
    {  
      headers: {  
        'Accept': 'application/json;odata=nometadata',  
        'odata-version': ''  
      }  
    })  
    .then((response: SPHttpClientResponse): Promise<{ value: IOrgChartItem\[\] }> => {  
      return response.json();  
    })  
    .then((response: { value: IOrgChartItem\[\] }): void => {  
      resolve(response.value);  
    }, (error: any): void => {  
      reject(error);  
    });  
  });      
}

1. Implement method to process the items and convert to org chart.

private processOrgChartItems(): void {  
  this.readOrgChartItems()  
    .then((orgChartItems: IOrgChartItem\[\]): void => {  
  
      let orgChartNodes: Array<ChartItem> = \[\];  
      var count: number;  
      for (count = 0; count < orgChartItems.length; count++) {  
        orgChartNodes.push(new ChartItem(orgChartItems\[count\].Id, orgChartItems\[count\].Title, orgChartItems\[count\].Url, orgChartItems\[count\].Parent ? orgChartItems\[count\].Parent.Id : undefined));  
      }  
  
      var arrayToTree: any = require('array-to-tree');  
      var orgChartHierarchyNodes: any = arrayToTree(orgChartNodes);  
      var output: any = JSON.stringify(orgChartHierarchyNodes\[0\]);  
  
      this.setState({  
        orgChartItems: JSON.parse(output)  
      });  
    });  
}

 

Test the WebPart

1. On the command prompt, type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Edit the webpart and add list name (i.e. OrgChart) to web part property

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-236.png)

1. The web part should display the data from SharePoint list in an organization chart

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-237.png)

1. Click on the nodes with url to see test the page navigation.

 

## Troubleshooting

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-238.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

## Summary

We can utilize open source npm packages in SharePoint framework to easily develop complex controls like organization chart control.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-react-based-orgchart-from-sharepoint-list/).
