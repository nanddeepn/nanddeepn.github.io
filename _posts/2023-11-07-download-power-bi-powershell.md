---
title: "Downloading Power BI Table Data with PowerShell"
date: "2023-10-17"
share: true
header:
  image: media/2023-11-07-download-power-bi-powershell/01.png
  teaser: media/2023-11-07-download-power-bi-powershell/01.png
categories:
  - Power BI
  - PowerShell
tags:
  - "2023"
  - November 2023
last_modified_at: 2023-11-07T00:00:00-00:00
---
## Overview

Power BI is a powerful business analytics tool that allows users to visualize and share insights from their data. While Power BI provides a user-friendly interface for creating reports and dashboards, there are times when you may need to automate the process of downloading data from Power BI tables. PowerShell, with its versatility and scripting capabilities, can be a handy tool for this task. 

In this article, we will explore how to download Power BI table data using PowerShell.


## Getting Started

Before we dive into the PowerShell script, make sure you have the following prerequisites:

- Power BI Account: You need to have a Power BI account with the necessary permissions to access the workspace and tables you want to download data from.

- Power BI Workspace access: Ensure that you have an access to Power BI workspace where the tables you want to retrieve data from are located.


## Installing the Power BI Module

To interact with the Power BI service through PowerShell, you will need to install the Power BI module. Open PowerShell as an administrator and run the following command:

```powershell
Install-Module -Name MicrosoftPowerBIMgmt
```

This command will install the Power BI Management module, which includes the cmdlets needed for working with Power BI.

## PowerShell Script

Now, let's create a PowerShell script that uses the Invoke-PowerBIRestMethod cmdlet to download data from a Power BI table. Here is a simple example:

```powershell
Login-PowerBI

$token = Get-PowerBIAccessToken -AsString
$apiEndpoint = "https://api.powerbi.com/v1.0/myorg/"

function Get-PowerBIWorkspaces {
    $requestUrl = $apiEndpoint + "groups"
    $response = Invoke-RestMethod -Uri $requestUrl -Method Get -Headers @{Authorization = "$token" }    
    $workspaces = $response.value | Select-Object id, name
    return $workspaces
}

function Get-PowerBIDatasets {
    param(
        [parameter(Mandatory = $true)][String] $WorkspaceId
    )

    $requestUrl = $apiEndpoint + "groups/$WorkspaceId/datasets"
    $response = Invoke-RestMethod -Uri $requestUrl -Method Get -Headers @{Authorization = "$token" }
    $datasets = $response.value | Select-Object id, name
    return $datasets
}

function Get-PowerBITables {
    param (
        [parameter(Mandatory = $true)][String] $WorkspaceId,
        [parameter(Mandatory = $true)][String] $DatasetId
    )
    
    $requestUrl = $apiEndpoint + "groups/$WorkspaceId/datasets/$DatasetId/datasources"
    Write-Host $requestUrl -ForegroundColor Blue

    $response = Invoke-RestMethod -Uri $requestUrl -Method Get -Headers @{Authorization = "$token" }
    Write-Host $response.value | Select-Object id -ForegroundColor Green
    return $response.value | Select-Object name
}

function Get-PowerBITableRows {
    param (
        [parameter(Mandatory = $true)][String] $WorkspaceId,
        [parameter(Mandatory = $true)][String] $DatasetId,
        [parameter(Mandatory = $true)][String] $TableName
    )
    
    $requestUrl = $apiEndpoint + "groups/$WorkspaceId/datasets/$DatasetId/tables/$TableName/rows"
    $response = Invoke-RestMethod -Uri $requestUrl -Method Get -Headers @{Authorization = "$token" }    
    return $response.value
}

# Get all workspaces
$workspaces = Get-PowerBIWorkspaces
ForEach ($workspace in $workspaces) {
    Write-Host "Workspace: $($workspace.name) - $($workspace.id)" -ForegroundColor Green

    $datasets = Get-PowerBIDatasets -WorkspaceId $workspace.id
    ForEach ($dataset in $datasets) {
        Write-Host "Dataset: $($dataset.name) - $($dataset.id)" -ForegroundColor Yellow

        $tables = Get-PowerBITables -WorkspaceId $workspace.id -DatasetId $dataset.id
        ForEach ($table in $tables) {
            Write-Host "Table: $($table.name)" -ForegroundColor Cyan

            $rows = Get-PowerBITableRows -WorkspaceId $workspace.id -DatasetId $dataset.id -TableName $table.name
            Write-Host $rows | Format-Table

            # Save data to a CSV file (optional)
            $response.value | Export-Csv -Path "$($table.name).csv" -NoTypeInformation
        }
    }
}
```

This PowerShell script uses the Invoke-RestMethod cmdlet to make a GET request to the Power BI API endpoint. The response is then processed to extract and display information about each table within the dataset.

> Note: The above script will only run for Power BI push datasets.


## Power BI push datasets

Power BI push datasets are a type of dataset in Power BI that enables real-time data streaming and analytics. Unlike traditional datasets, where data is imported or queried periodically, push datasets allow external applications to push data directly into Power BI for immediate analysis and visualization. This capability is particularly useful in scenarios where you need to monitor and analyze live or near-real-time data.


## Get Table data for non-push datasets

For non-push datasets in Power BI, which typically include datasets where data is imported or connected without real-time streaming, you can use DAX (Data Analysis Expressions) to retrieve and analyze data. In Power BI Desktop or Power BI Service, you can create measures, calculated columns, and tables using DAX expressions to get the desired data. 

## DAX (Data Analysis Expressions)

In Power BI, you can use the DAX (Data Analysis Expressions) language to create queries for tables and manipulate data. DAX is primarily used in calculated columns, measures, and calculated tables, but you can also use it to query data directly. To create a DAX query for a table, you can use the EVALUATE statement in DAX Studio or in Power BI Desktop.

Here's an example of how to create a simple DAX query for a table:

**Option 1: Using DAX Studio**

1. Download and install it from the official website: [DAX Studio](https://daxstudio.org/).
2. Open DAX Studio and connect to your Power BI Desktop file or Power BI service workspace.
3. In the Query tab, you can write a DAX query to retrieve data from a specific table. For example:

  ```
  EVALUATE
  'YourTableName'
  ```

  Replace 'YourTableName' with the actual name of your table.

4. Click the "Run" button to execute the DAX query. The results will be displayed in the bottom pane.

  ![](/media/2023-11-07-download-power-bi-powershell/01.png)


**Option 2: Using Power BI Desktop (DAX Calculated Table) **

1. Open your Power BI Desktop file.
2. Navigate to the Data view by clicking on the "Data" icon in the left sidebar.
3. In the Modeling tab, click on "New Table" to create a new calculated table.
4. In the formula bar, write a DAX query to define the calculated table. For example:

  ```
  YourCalculatedTable =
  EVALUATE
  'YourTableName'
  ```

  Replace 'YourTableName' with the actual name of your table.

> Note: These examples demonstrate how to use the EVALUATE statement to query a table in DAX. You can customize the DAX query based on your specific requirements, including filtering, sorting, and aggregating data within the EVALUATE statement. DAX provides a powerful and flexible language for working with data in Power BI.


## PowerShell to get Table data for non-push datasets

Use the below PowerShell to get Table data for non-push datasets:

```powershell
[CmdletBinding()]
param (
    [parameter(Mandatory)][string] $DatasetID,
    [parameter(Mandatory)][string] $TableName
)

Login-PowerBI

$requestUrl = "datasets/$DatasetID/executeQueries"

$requestBody = @"
{ 
    "queries": [
        {
            "query": "EVALUATE($TableName)",
            "includeNulls": true
        }
    ]    
}
"@

$result = Invoke-PowerBIRestMethod -Url $requestUrl -Method POST -Body $requestBody

$parsed = $result | ConvertFrom-Json
$parsed.results[0].tables[0].rows | Export-Csv .\$TableName.csv -NoTypeInformation -Encoding UTF8
```

## Summary

PowerShell, combined with the Power BI Management module, provides a convenient way to automate the process of downloading data from Power BI tables. This script serves as a starting point, and you can customize it based on your specific requirements, such as filtering data, scheduling the script as a task, or integrating it into a larger automation workflow.
