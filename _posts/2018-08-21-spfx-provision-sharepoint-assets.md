---
title: "SharePoint Framework - Provision SharePoint Assets"
date: "2018-08-21"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2018-08-21-spfx-provision-sharepoint-assets/06.png
  teaser: media/2018-08-21-spfx-provision-sharepoint-assets/06.png
tags:
  - "2018"
  - August 2018
last_modified_at: 2018-08-21T00:00:00-00:00
---

## Overview

SharePoint components like web parts normally interact with the SharePoint assets like lists, libraries, content types, etc. In the full trust solutions, we deploy these SharePoint assets along with web part in a single deployable package (.wsp).

Similarly, SharePoint Framework (SPFx) client side web parts do interact with the underlying lists and libraries in SharePoint site. These SharePoint assets needs to be provisioned along with the client side solution package. SharePoint framework toolchain allows to package and deploy SharePoint assets with client side solution package.

In this article, we will explore how we can package together the SharePoint assets needed for SPFx web parts.


## SharePoint Assets

SharePoint assets is a generic term referred to basic building blocks of SharePoint that constitutes fields, content types, list instances.


**Fields (Site Columns):**

A field is metadata that describes property or attribute of object we want to represent. For example, an Employee can be represented with ID, Name, Department, etc. Each field has a specific type such as text, number, Boolean, etc.

Below is an example of field representing number:

```xml
<Field ID="{060E50AC-E9C1-4D3C-B1F9-DE0BCAC300F6}"  
           Name="SPFxAmount"  
           DisplayName="Amount"  
           Type="Currency"  
           Decimals="2"  
           Min="0"  
           Required="FALSE"  
           Group="SPFx Columns" />
```


**Content types**

Content type is reusable collection of site columns. Below is an example of content type that uses site column:

```xml
<ContentType ID="0x010042D0C1C200A14B6887742B6344675C8B"   
           Name="Cost Center"   
           Group="SPFx Content Types"   
           Description="Content types from SPFx">  
       <FieldRefs>  
           <FieldRef ID="{060E50AC-E9C1-4D3C-B1F9-DE0BCAC300F6}" />   
       </FieldRefs>  
</ContentType>
```


**List instance**

List instance is pre-defined SharePoint list with well-known identifier. We can add, update, and delete items from list.

```xml
<ListInstance   
           FeatureId="00bfea71-de22-43b2-a848-c05709900100"  
           Title="SPFx List"   
           Description="SPFx List"  
           TemplateType="100"  
           Url="Lists/SPFxList">  
</ListInstance>
```


**List instance with custom schema**

We can create our own schema to define fields, content types, and views for list instance. Use CustomSchema attribute in ListInstance element to reference custom schema.

```xml
<ListInstance   
           CustomSchema="schema.xml"  
           FeatureId="00bfea71-de22-43b2-a848-c05709900100"  
           Title="SPFx List"   
           Description="SPFx List"  
           TemplateType="100"  
           Url="Lists/SPFxList">  
</ListInstance>
```


## Create SPFx Solution

1. Open command prompt. Create a directory for SPFx solution.

    ```
    md spfx-provisionspassets
    ```

2. Navigate to above created directory.

    ```
    cd spfx-provisionspassets
    ```

3. Run Yeoman SharePoint Generator to create the solution.

    ```
    yo @microsoft/sharepoint
    ```

4. Yeoman generator will present you with the wizard by asking questions about the solution to be created.

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/01.png)

    - **Solution Name:** Hit enter to have default name (spfx-provisionspassets in this case) or type in any other name for your solution.
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
        - Selected choice: ProvisionSPAssets
    - **Web part description:** Hit enter to select the default description or type in any other value.
        - Selected choice: Provision SharePoint Assets with SPFx
    - **Framework to use:** Select any JavaScript framework to develop the component. Available choices are (No JavaScript Framework, React, and Knockout)
        - Selected choice: No JavaScript Framework

5. Yeoman generator will perform scaffolding process to generate the solution. The scaffolding process will take significant amount of time.
6. Once the scaffolding process is completed, lock down the version of project dependencies by running below command.

    ```
    npm shrinkwrap
    ```

7. On the command prompt type below command to open the solution in code editor of your choice.

    ```
    code .
    ```


## Add SharePoint Assets to Solution

1. Create a folder hierarchy as "sharepoint\assets".

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/02.png)

2. Add a file elements.xml under "sharepoint\assets" folder.

```xml
<?xml version="1.0" encoding="utf-8"?>  
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">  
  
   <Field ID="{060E50AC-E9C1-4D3C-B1F9-DE0BCAC300F6}"  
           Name="SPFxAmount"  
           DisplayName="Amount"  
           Type="Currency"  
           Decimals="2"  
           Min="0"  
           Required="FALSE"  
           Group="SPFx Columns" />  
  
   <Field ID="{943E7530-5E2B-4C02-8259-CCD93A9ECB18}"  
           Name="SPFxCostCenter"  
           DisplayName="Cost Center"  
           Type="Choice"  
           Required="FALSE"  
           Group="SPFx Columns">  
       <CHOICES>  
       <CHOICE>Administration</CHOICE>  
       <CHOICE>Information</CHOICE>  
       <CHOICE>Facilities</CHOICE>  
       <CHOICE>Operations</CHOICE>  
       <CHOICE>Sales</CHOICE>  
       <CHOICE>Marketing</CHOICE>  
       </CHOICES>  
   </Field>  
  
   <ContentType ID="0x010042D0C1C200A14B6887742B6344675C8B"   
           Name="Cost Center"   
           Group="SPFx Content Types"   
           Description="Sample content types from web part solution">  
       <FieldRefs>  
           <FieldRef ID="{060E50AC-E9C1-4D3C-B1F9-DE0BCAC300F6}" />   
           <FieldRef ID="{943E7530-5E2B-4C02-8259-CCD93A9ECB18}" />  
       </FieldRefs>  
   </ContentType>   
  
   <ListInstance   
           CustomSchema="schema.xml"  
           FeatureId="00bfea71-de22-43b2-a848-c05709900100"  
           Title="SPFx List"   
           Description="SPFx List"  
           TemplateType="100"  
           Url="Lists/SPFxList">  
   </ListInstance>  
  
</Elements>
```

We are provisioning 2 fields, content type and list instance with custom schema. FeatureId in the ListInstance represents the ID of feature which contains list definition. The featureId mentioned in the xml represents the ID for custom list definition.


## Custom Schema

Add schema.xml file as our custom schema.

```xml
<List xmlns:ows="Microsoft SharePoint" Title="Basic List" EnableContentTypes="TRUE" FolderCreation="FALSE" Direction="$Resources:Direction;" Url="Lists/Basic List" BaseType="0" xmlns="http://schemas.microsoft.com/sharepoint/">  
 <MetaData>  
   <ContentTypes>  
     <ContentTypeRef ID="0x010042D0C1C200A14B6887742B6344675C8B" />  
   </ContentTypes>  
   <Fields></Fields>  
   <Views>  
     <View BaseViewID="1" Type="HTML" WebPartZoneID="Main" DisplayName="$Resources:core,objectiv_schema_mwsidcamlidC24;" DefaultView="TRUE" MobileView="TRUE" MobileDefaultView="TRUE" SetupPath="pages\viewpage.aspx" ImageUrl="/_layouts/images/generic.png" Url="AllItems.aspx">  
       <XslLink Default="TRUE">main.xsl</XslLink>  
       <JSLink>clienttemplates.js</JSLink>  
       <RowLimit Paged="TRUE">30</RowLimit>  
       <Toolbar Type="Standard" />  
       <ViewFields>  
         <FieldRef Name="LinkTitle"></FieldRef>  
         <FieldRef Name="SPFxAmount"></FieldRef>  
         <FieldRef Name="SPFxCostCenter"></FieldRef>  
       </ViewFields>  
       <Query>  
         <OrderBy>  
           <FieldRef Name="ID" />  
         </OrderBy>  
       </Query>  
     </View>  
   </Views>  
   <Forms>  
     <Form Type="DisplayForm" Url="DispForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />  
     <Form Type="EditForm" Url="EditForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />  
     <Form Type="NewForm" Url="NewForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />  
   </Forms>  
 </MetaData>  
</List>
```


## Package Assets as part of Solution

We have created the assets and custom schema. We need to package these files as part of solution.

1. Open package-solution.json file under config folder.
2. Include feature framework definition for solution package.

```json
{  
  "$schema": "https://developer.microsoft.com/json-schemas/spfx-build/package-solution.schema.json",  
  "solution": {  
    "name": "spfx-provisionspassets-client-side-solution",  
    "id": "ea551656-9f74-4107-b049-e296e475932d",  
    "version": "1.0.0.0",  
    "includeClientSideAssets": true,  
    "features": [{  
      "title": "asset-deployment-webpart-client-side-solution",  
      "description": "asset-deployment-webpart-client-side-solution",  
      "id": "523fe887-ced5-4036-b564-8dad5c6c6e24",     // Specify unique GUID  
      "version": "1.0.0.0",  
      "assets": {  
        "elementManifests": [  
          "elements.xml"  
        ],  
        "elementFiles":[  
          "schema.xml"  
        ]  
      }  
    }]  
  },  
  "paths": {  
    "zippedPackage": "solution/spfx-provisionspassets.sppkg"  
  }  
}
```

Specify unique GUID for feature ID.
 

## Deploy and Test

1. Package your client-side solution by running below command.

    ```
    gulp bundle
    ```

2. Create solution package by running below command.

    ```
    gulp package-solution
    ```

    This command will create package (.sppkg) inside sharepoint/solution folder.

3. Deploy the package to app catalog

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/03.png)

4. Click **Deploy**.
5. Open SharePoint site, click **Add an app**.
6. Install the app.

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/04.png)

7. When installation finishes, refresh the page. The site should have “SPFx List” provisioned.

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/05.png)

8. Open **SPFx List**, it should have our content type with site columns inside it.

    ![](/media/2018-08-21-spfx-provision-sharepoint-assets/06.png)


## Summary

SharePoint assets can be provisioned using SPFx. We can define the needed structure to provision on SharePoint site, which can be utilized by SPFx web parts.

This content was originally posted [here](https://www.c-sharpcorner.com/article/sharepoint-framework-provision-sharepoint-assets/).
