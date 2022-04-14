---
title: "Get Whiteboard details with PowerShell"
date: "2022-04-14"
share: true
header:
  image: media/2022-04-14-whiteboard-powershell/sample-output.png
  teaser: media/2022-04-14-whiteboard-powershell/sample-output.png
categories:
  - Office 365
  - Whiteboard
tags:
  - "2022"
  - April 2022
last_modified_at: 2022-04-14T00:00:00-00:00
---
## Overview

Whiteboard is a digital canvas app in M365 for collaboration and creating engaging experiences during meetings. From an administrative point of view, it is important to understand the usage and adoption of dashboards.

In this article, we will explore the PowerShell support for extracting the Whiteboard information.

## WhiteboardAdmin module

Administration cmdlets for Microsoft Whiteboard are available under the [WhiteboardAdmin](https://www.powershellgallery.com/packages/WhiteboardAdmin/) module.

Install the module by running the below command

```powershell
Install-Module -Name WhiteboardAdmin
```

You may need to update your execution policy by running the below cmdlet.

```powershell
Set-ExecutionPolicy RemoteSigned
```

## Get all whiteboard owners in a tenant

Firstly, it is important to understand the users who own the Whiteboards. The owners are returned per geographic region.

Let us store the regions in an array

```powershell
$supportedGeographies = @('Europe', 'Australia', 'Worldwide')
```

Run the below command to get all Whiteboard owners per geography:

```powershell
foreach ($geography in $supportedGeographies) {
    Get-WhiteboardOwners -Geography $geography
}
```

The owners are returned as GUIDs.

![](/media/2022-04-14-whiteboard-powershell/01.png)


## Get user details

To get the user details from the GUID, install the Microsoft Graph PowerShell SDK by running the below command:

```powershell
Install-Module Microsoft.Graph
```

Use the below command to get the user details.

```powershell
Connect-MgGraph -Scopes 'User.Read.All'
Get-MgUser -UserId e1251b10-1ba4-49e3-b35a-933e3f21772b
```

![](/media/2022-04-14-whiteboard-powershell/02.png)


## Get whiteboard details

To get the whiteboards created by each of the owners, use the below command:

```powershell
Get-Whiteboard -UserId e1251b10-1ba4-49e3-b35a-933e3f21772b
```

![](/media/2022-04-14-whiteboard-powershell/03.png)


## The Final Script

Below PowerShell script will help to get all the Whiteboard owners and the whiteboards owned by them into a tenant.

```powershell
# Import modules
Import-Module WhiteboardAdmin
Import-Module Microsoft.Graph.Users

$dateTime = (Get-Date).toString("dd-MM-yyyy")
$invocation = (Get-Variable MyInvocation).Value
$directoryPath = Split-Path $invocation.MyCommand.Path
$fileName = "WhiteboardReport-" + $dateTime + ".csv"
$logFileName = "WhiteboardReport-" + $dateTime + ".log"
$outputView = $directoryPath + "\" + $fileName
Start-Transcript -Path ".\$logFileName"

try {
	# Connect to Azure AD
	$Msolcred = Get-credential
	Connect-MgGraph -Scopes "User.Read.All"

	# The geography to look for board owners in. Accepted values are: Europe, Australia, or Worldwide (all boards not in australia or europe).
	$supportedGeographies = @("Europe", "Australia", "Worldwide")
	
	# Array to hold Whiteboard owners
	$whiteboardOwners = @()
	
	foreach ($geography in $supportedGeographies) {
		Write-Host "`nGetting Whiteboard owners for geography: $($geography) ..."
		$geographyOwners = Get-WhiteboardOwners -Geography $geography		
		
		foreach ($geographyOwner in $geographyOwners.items) {			
			$exportOwner = New-Object PSObject
			$exportOwner | Add-Member -MemberType NoteProperty -name "Geography" -value $geography
			$exportOwner | Add-Member -MemberType NoteProperty -name "OwnerID" -value $geographyOwner
			
			try {
				$ownerInfo = Get-MgUser -UserId $geographyOwner -ErrorAction Stop
				if ($ownerInfo) {
					$exportOwner | Add-Member -MemberType NoteProperty -name "OwnerUPN" -value $ownerInfo.UserPrincipalName
					$exportOwner | Add-Member -MemberType NoteProperty -name "OwnerDisplayName" -value $ownerInfo.DisplayName
				}
			}
			catch {
				write-host -f Red "Error:" $_.Exception.Message
			}
				
			$whiteboardOwners += $exportOwner
		}
		
		Write-Host "Found $($geographyOwners.items.Count) Whiteboard owners."
	}
	
	# Array to hold Whiteboard details
	$whiteboards = @()
	
	# Get whiteboards from the Microsoft Whiteboard service by owners
	foreach ($whiteboardOwner in $whiteboardOwners) {
		Write-Host "`nGetting Whiteboards for owner: $($whiteboardOwner.OwnerUPN) ..."

		$retryCount = 0
		$maxRetries = 3
		$pauseDuration = 2
		$processed = $false
		
		while ($processed -ne $true){
			try {
				$whiteboardInfo = Get-Whiteboard -UserId $whiteboardOwner.OwnerID
				
				foreach ($whiteboardInstance in $whiteboardInfo) {
					$exportWhiteboard = New-Object PSObject
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Geography" -value $whiteboardOwner.Geography
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Owner ID" -value $whiteboardOwner.OwnerID
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Owner UPN" -value $whiteboardOwner.OwnerUPN
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Owner Name" -value $whiteboardOwner.OwnerDisplayName
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Whiteboard ID" -value $whiteboardInstance.id
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Title" -value $whiteboardInstance.title
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Is Shared" -value $whiteboardInstance.isShared
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Created" -value $whiteboardInstance.createdTime
					$exportWhiteboard | Add-Member -MemberType NoteProperty -name "Modified" -value $whiteboardInstance.lastModifiedTime
					
					$whiteboards += $exportWhiteboard
				}
				
				Write-Host "Found $($whiteboardInfo.Count) Whiteboards owned by: $($whiteboardOwner.OwnerUPN)"
				$processed = $true
			} 
			catch {
				if ($retryCount -ge $maxRetries){
					# not going to retry again
					$processed = $true
					Write-Host 'Not going to retry...'
				} 
				else {
					$retryCount += 1
					Write-Host "Retry attempt $retryCount after a $pauseDuration second pause..."
					Start-Sleep -Seconds $pauseDuration
				}
			}
		}
	}
	
	Write-Host "`nFound $($whiteboards.Count) Whiteboards in a tenant."

	# Export the result Array to CSV file
	$whiteboards | sort "Geography" | Export-CSV -Path $outputView -Force -NoTypeInformation
	
	Write-Host "`nFinished"
}
catch {
    Write-Host -f Red "Error:" $_.Exception.Message
}
finally {
	Stop-Transcript
}
```

The updated script is available under the [PnP Script Sample repository](https://pnp.github.io/script-samples/whiteboard-report-usage/README.html?tabs=whiteboard).


The exported report looks as follows:
![](/media/2022-04-14-whiteboard-powershell/sample-output.png)


## References

- [Microsoft Whiteboard Admin](https://docs.microsoft.com/en-us/powershell/module/whiteboard?WT.mc_id=M365-MVP-5003693)
- [Install the Microsoft Graph PowerShell SDK](https://docs.microsoft.com/en-us/graph/powershell/installation?WT.mc_id=M365-MVP-5003693)
- [WhiteboardAdmin module](https://www.powershellgallery.com/packages/WhiteboardAdmin/)
- [Microsoft Graph module](https://www.powershellgallery.com/packages/Microsoft.Graph/)
