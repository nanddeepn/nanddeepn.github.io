---
title: "SharePoint Framework – React based Tree view"
date: "2018-10-05"
---

## Overview

Webparts developed in SharePoint makes use of various controls to depict the functionality on UI. Office 365 UI Fabric offers majority of UI controls that can be used with SharePoint Framework client web parts. However there are certain UI controls which are not available to use. Tree view is one of example of such UI control.

In this article, we will explore tree view control. We will use React JS in this example.

## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

md spfx-react-treeview

1. Navigate to above created directory.

cd spfx-react-treeview

1. Run Yeoman SharePoint Generator to create the solution.

yo @microsoft/sharepoint

1. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-226.png)

- **Solution Name:** Hit enter to have default name (spfx-react-treeview in this case) or type in any other name for your solution.
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
    - Selected choice: TreeViewExample
- **Web part description:** Hit enter to select the default description or type in any other value.
    - Selected choice: Tree view control with React
- **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
    - Selected choice: React

1. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
2. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

npm shrinkwrap

1. On the command prompt type below command to open the solution in code editor of your choice.

code .

Tree View Control

At the time of writing this article tree view control is not available in Office 365 UI Fabric controls. We will explore the tree view control called as react-super-treeview ([https://www.npmjs.com/package/react-super-treeview](https://www.npmjs.com/package/react-super-treeview))

Use below command to install the tree view control.

npm install react-super-treeview --save

The --save option enables NPM to include the packages to dependencies section of package.json file.

## Code the webpart

1. Open TreeViewExample.tsx file under “\\src\\webparts\\treeViewExample\\components\\” folder.
2. Import the tree view control.

import SuperTreeview from 'react-super-treeview';

1. Define a tree structure in a state.

this.state = {  
  data: \[  
      {  
          id: 1,  
          name: 'Parent A'  
      },  
      {  
          id: 2,  
          name: 'Parent B',  
          isExpanded: true,  
          isChecked: true,  
          children: \[  
              {  
                  id: 21,  
                  name: 'Child 1',  
                  isExpanded: true,  
                  children: \[  
                      {  
                          id: 5,  
                          name: "Grand Child",  
                          isExpanded: true  
                      }  
                  \]  
              },  
              {  
                  id: 22,  
                  name: 'Child 2'  
              },  
              {  
                  id: 23,  
                  name: 'Child 3'  
              },  
              {  
                  id: 24,  
                  name: 'Child 4'  
              }  
          \]  
      }  
  \]  
};

1. Modify the render method to render the tree view.

public render(): React.ReactElement<ITreeViewExampleProps> {  
  return (  
    // RENDER THE COMPONENT  
    <div className={ styles.treeViewExample }>    
      <div className={ styles.container }>    
        <div className={ styles.row }>    
          <div className={ styles.column }>    
              
            <SuperTreeview  
              data={ this.state.data }  
              noChildrenAvailableMessage= ''  
              isDeletable= {(node, depth) => {  
                return false;  
              }}  
              onUpdateCb={(updatedData) => {  
                this.setState({data: updatedData});  
                let selectedNodeName: string = sessionStorage.getItem(sessionStorageSelectedNodeKey);  
  
                resetNodes(updatedData);  
          
                function resetNodes(nodes){  
                    nodes.forEach((node)=>{  
                        if (node.name !== selectedNodeName) {  
                          node.isChecked = false;  
                        }  
                          
                        if(node.children){  
                          resetNodes(node.children);  
                        }  
                    });  
                }  
  
                sessionStorage.removeItem(sessionStorageSelectedNodeKey);  
              }}  
              onCheckToggleCb={(nodes, depth)=>{  
                  sessionStorage.setItem(sessionStorageSelectedNodeKey, nodes\[0\].name);  
            }}  
            />          
          </div>    
        </div>    
      </div>    
    </div>    
  );  
}

Test the WebPart

1. On the command prompt, type “gulp serve”
2. Open SharePoint site
3. Navigate to /\_layouts/15/workbench.aspx
4. Add the webpart to page.
5. Verify the tree view

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-227.png)

## Troubleshooting

In some cases SharePoint workbench (https://\[tenant\].sharepoint.com/\_layouts/15/workbench.aspx) shows below error although “gulp serve” is running.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-228.png)

Open below url in the next tab of browser. Accept the warning message.

https://localhost:4321/temp/manifests.js

## Summary

Office 365 UI fabric does not provide all the controls, however we can utilize any third party controls to meet the business needs.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-react-based-tree-view/).
