---
title: "Handle Teams Channel Creation with Graph Notifications using Azure Functions"
date: "2023-04-08"
share: true
header:
  image: media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/architecture.png
  teaser: media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/architecture.png
categories:
  - Microsoft Teams
  - Microsoft Graph
  - Azure Functions
tags:
  - "2023"
  - April 2023
last_modified_at: 2023-04-08T00:00:00-00:00
---

## Overview

As a business scenario, we some times needs to hook up to a event when a new Teams channel is happened (for e.g. channel created, deleted, or updated). Graph notifications helps to respond instantly for these scenarios.

In this article, we are going explore on handling the Graph change notifications when Teams channel event has occurred.


## Pull vs Push mechanism

There are two scenarios while approaching this business scenario, i.e., pull and push. Each has its own pros and cons as follows:

**Pull from Audit logs**

All of the user and admin activities in M365 (including Teams channel) is recorded in Audit logs. One can pull the information from Audit logs as needed by running a timer job. While you can decide your own schedule to pull the audit logs, this does not offer a real-time event handling. Also, some times retrieving audit logs (search-unifiedauditlog) may timeout.

**Push mechanism with Graph notifications**

Graph notifications allows you to respond to the event as it occurs. Although this sounds straight forward, you can subscribe to limited Graph resources listed [here](https://learn.microsoft.com/en-us/graph/api/resources/webhooks?WT.mc_id=M365-MVP-5003693). Also, you need to take care of updating the subscription for their renewal.

In this article, we will explore the Graph notifications option.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/architecture.png)

1. Azure Function gets an access token for Microsoft Graph by using Azure AD.
2. Azure Function creates a Microsoft Graph subscription and updates it every hour to track changes.
3. When a Microsoft Graph change notification is processed, the message is handled by Azure Function.

## Azure AD App Registration

As per the [documentation](https://learn.microsoft.com/en-us/graph/teams-changenotifications-team-and-channel?WT.mc_id=M365-MVP-5003693), `/teams/getAllChannels` resource only supports application permission.

| **Supported resource** | **Delegated (work or school account)** | **Delegated (personal Microsoft account)** | **Application** |
| --- | --- | --- | --- |
| [channel](https://learn.microsoft.com/en-us/graph/api/resources/channel?WT.mc_id=M365-MVP-5003693) (/teams/getAllChannels – all channels in an organization) | Not supported | Not supported | Channel.ReadBasic.All, ChannelSettings.Read.All |

We will start by registering an app in Azure AD with application permission of Channel.ReadBasic.All. Note down the Application (client) ID, Directory (tenant) ID, and client secret.


## Create Function App

Create a function app to handle the Graph notifications.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/01.png)

**Create a function**

Create a function as `HTTP trigger`.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/02.png)


## Create a Subscription

We will now create a subscription, i.e., a listener application to receive change notifications when the requested type of changes occurs to the specified resource in Microsoft Graph (i.e. Teams channel created).

We will make use of Postman to create the subscription.

**Step1: Get Access token**

Use the below code to get the access token

```
POST /{{tenant}}/oauth2/v2.0/token HTTP/1.1 
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id={{client_id}}
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret={{client_secret}}
&grant_type=client_credentials
```

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/03.png)

From the response, note down the `access_token`.

**Step 2: Register the subscription**

Set Authorization:

1. Under **Authorization**, select the type as **Bearer Token**.
2. Paste the `access_token` from previous step as the Token.

    ![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/04.png)

**Create subscription**

Make below POST request to create the subscription.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/05.png)

You will most likely receive an error: Subscription validation request failed. Response must exactly match validationToken query parameter.


## Update Function App

The notificationUrl (i.e., Azure function - we will create in next step) must be capable of responding to the validation request as mentioned [here](https://learn.microsoft.com/en-us/graph/webhooks?tabs=http#notification-endpoint-validation&WT.mc_id=M365-MVP-5003693).

Update the function (e.g. ProcessGraphNotification) as follows:

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Response for Subscription Notification Validation. Respond back with validationToken. 
if ($Request.Query.validationToken) {
    Write-Host "RESPONSE: Sending status code 'OK' and validationToken to Subscription Notification Validation Request" 
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
            StatusCode = [HttpStatusCode]::OK
            Body       = $Request.Query.validationToken
        })
}
```

Use the Postman request again to create a subscription. It should be successful now.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/06.png)

**PowerShell approach**

If you prefer the PowerShell way, then you may use the below script (instead of Postman query).

```powershell
<#
.DESCRIPTION
Regsiter the Graph notification for Teams channel created.

.OUTPUTS
Graph notification registration details.
#>

param (
    [parameter(Mandatory = $true)]$ClientID,
    [parameter(Mandatory = $true)]$ClientSecret,
    [parameter(Mandatory = $true)]$TenantID,
    [parameter(Mandatory = $true)]$NotificationURL
)

$expiryInHours = 1

$clientStateValue = New-Guid

$notificationExpiry = (Get-Date).addHours($expiryInHours).ToUniversalTime()
$utcExpiry = Get-Date $notificationExpiry -Format yyyy-MM-ddThh:mm:ss.0000000Z

# Get Access Token
$accessToken = (Invoke-RestMethod -uri "https://login.microsoftonline.com/$($TenantID)/oauth2/v2.0/token" `
-Method Post `
-Headers @{"Content-Type" = "application/x-www-form-urlencoded" } `
-Body "grant_type=client_credentials&client_id=$($ClientID)&scope=https://graph.microsoft.com/.default&client_secret=$($ClientSecret)").access_token


# Azure AD Users change notification subscription configuration
$createSubscriptionBody = @{
    changeType = "created"
    notificationUrl = $NotificationURL
    resource = "/teams/getAllChannels"
    clientState = $clientStateValue.Guid
    expirationDateTime = "$($utcExpiry)"
    latestSupportedTlsVersion = "v1_2"
}

# Create Notification Subscription
$newTeamsChannelNotificationSubscription = Invoke-RestMethod -Method Post `
    -Uri "https://graph.microsoft.com/v1.0/subscriptions" `
    -Body ($createSubscriptionBody | ConvertTo-Json) `
    -Headers @{Authorization = "Bearer $($accessToken)"; "content-type" = "application/json" }

$newTeamsChannelNotificationSubscription
```

**Update Azure Function Application settings**

Note down the Id and clientState and update them to the Azure Function Application settings as follows:

| **Azure Function Application Setting** | **Value** |
| --- | --- |
| GRAPH_NOTIFICATION_CHANNEL_CREATED_SUBSCRIPTION_ID | ID |
| GRAPH_NOTIFICATION_CHANNEL_CREATED_SUBSCRIPTION_CLIENTSTATE | clientState |

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/07.png)

We will use these values while subscription renewal and validating the client state.


## Renew Subscription

Maximum length of subscription for Teams channel resource type is 60 minutes (1 hour) as mentioned [here](https://learn.microsoft.com/en-us/graph/api/resources/subscription?WT.mc_id=M365-MVP-5003693).

Let us create an Azure function as follows to trigger every hour.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/08.png)

Use below code to renew the subscription.

```powershell
# Input bindings are passed in via param block.
param($Timer)

# Get the current universal time in the default string format.
$currentUTCtime = (Get-Date).ToUniversalTime()

# The 'IsPastDue' property is 'true' when the current function invocation is later than scheduled.
if ($Timer.IsPastDue) {
    Write-Host "PowerShell timer is running late!"
}

$ClientID = ""
$ClientSecret = ""
$TenantID = ""

# Get Access Token
$accessToken = (Invoke-RestMethod -uri "https://login.microsoftonline.com/$($TenantID)/oauth2/v2.0/token" `
        -Method Post `
        -Headers @{"Content-Type" = "application/x-www-form-urlencoded" } `
        -Body "grant_type=client_credentials&client_id=$($ClientID)&scope=https://graph.microsoft.com/.default&client_secret=$($ClientSecret)").access_token

# Get Subscription
$notificationSubscription = $null

try {
	$notificationSubscription = Invoke-RestMethod -method GET `
		-Uri "https://graph.microsoft.com/v1.0/subscriptions/$($env:GRAPH_NOTIFICATION_CHANNEL_CREATED_SUBSCRIPTION_ID)" `
		-Headers @{Authorization = "Bearer $($accessToken)"; "content-type" = "application/json"}
}
catch {	
}

if ($notificationSubscription) {        
    # Update Subscription 
    $expiryHours = 1
    $notificationExpiry = (get-date).addHours($expiryHours).ToUniversalTime() 
    $utcExpiry = Get-Date $notificationExpiry -Format yyyy-MM-ddTHH:mm:ss.0000000Z

    $updateSubscriptionBody = @{
        expirationDateTime = $utcExpiry
    }

    $extendNotificationSubscription = Invoke-RestMethod -method PATCH `
        -Uri "https://graph.microsoft.com/v1.0/subscriptions/$($env:GRAPH_NOTIFICATION_CHANNEL_CREATED_SUBSCRIPTION_ID)" `
        -body ($updateSubscriptionBody | convertTo-json) `
        -Headers @{Authorization = "Bearer $($accessToken)"; "content-type" = "application/json"}

    Write-Host "New Subscription Expiry: $($extendNotificationSubscription.expirationDateTime)"
    Write-Host "Subscription ClientState: $($extendNotificationSubscription.clientState)"
}
else {
    Write-Host "Notification not found"
}

# Write an information log with the current time.
Write-Host "PowerShell timer trigger function ran! TIME: $currentUTCtime"

```

Finally, update the function (e.g. ProcessGraphNotification) as follows to update the channel description of newly created Teams channel:

```powershell
using namespace System.Net

# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Response for Subscription Notification Validation. Respond back with validationToken. 
if ($Request.Query.validationToken) {
    Write-Host "RESPONSE: Sending status code 'OK' and validationToken to Subscription Notification Validation Request" 
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
            StatusCode = [HttpStatusCode]::OK
            Body       = $Request.Query.validationToken
        })
}

# Convert notification details to PSObject
$objNotification = ($Request.RawBody | ConvertFrom-JSON).value
Write-Host $objNotification
# https://www.youtube.com/watch?v=J0Xyfqs_gYA

if ($objNotification.clientState -eq "$($env:GRAPH_NOTIFICATION_CHANNEL_CREATED_SUBSCRIPTION_CLIENTSTATE)") {
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
            StatusCode = [HttpStatusCode]::BadRequest
        })
}
else {
    Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
            StatusCode = [HttpStatusCode]::Accepted
        })
}

# Output Notification to host
Write-Host "NOTIFICATION: clientState: $($objNotification.clientState)"
Write-Host "NOTIFICATION: changeResource: $($objNotification.resource)"
Write-Host "NOTIFICATION: changeType: $($objNotification.changeType)"

# The resource information ($objNotification.resource) will be as follows
# # "teams('c892d52b-954d-4348-a269-6cf3a7339306')/channels('19:3f5002df7ea3404aa8f8@thread.tacv2')"
# Use regular expression to extract the TeamID and ChannelID
$teamsPattern = "teams\('(.+?)'\)"
$channelsPattern = "channels\('(.+?)'\)"

$teamID = [regex]::Match($objNotification.resource, $teamsPattern).Groups[1].Value
$channelID = [regex]::Match($objNotification.resource, $channelsPattern).Groups[1].Value

Write-Host "Team ID: $($teamID)"
Write-Host "Channel ID: $($channelID)"

$clientID = $env:CLIENT_ID
$clientSecret = $env:CLIENT_SECRET
$tenantID = $env:TENANT_ID

# Get Access Token
$accessToken = (Invoke-RestMethod -uri "https://login.microsoftonline.com/$tenantID/oauth2/v2.0/token" `
            -Method Post `
            -Headers @{"Content-Type" = 'application/x-www-form-urlencoded'} `
            -Body "grant_type=client_credentials&client_id=$($clientID)&client_secret=$($clientSecret)&scope=https://graph.microsoft.com/.default").access_token

# Update channel description
Invoke-RestMethod -Method PATCH `
    -Uri "https://graph.microsoft.com/v1.0/teams/$($teamID)/channels/$($channelID)" `
    -Headers @{Authorization = "Bearer $($accessToken)"; "content-type" = "application/json"} `
    -Body '{"description": "This channel is processed by graph notifications."}'

Write-Host "Finished"
```

## Test the scenario

Create a new channel in any Teams team in the tenant. Watch the output from the http trigger. You should see the TeamID and ChannelID of the newly created channel.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/09.png)

The Graph notification subscription will be renewed by Azure function with timer trigger.

![](/media/2023-04-08-handle-teams-channel-graph-notifications-az-functions/10.png)


## Summary

In this article, we explored handling the Graph change notifications when Teams channel event (e.g., created) has occurred using Azure functions.


## Code Download

The code developed during this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/MSGraph/Teams%20Channel%20Creation%20with%20Graph%20Notifications%20using%20Azure%20Functions).


## References

- [Use the Microsoft Graph API to get change notifications](https://learn.microsoft.com/en-us/graph/api/resources/webhooks?WT.mc_id=M365-MVP-5003693)
- [Create subscription](https://learn.microsoft.com/en-us/graph/api/subscription-post-subscriptions?WT.mc_id=M365-MVP-5003693)
- [Update subscription](https://learn.microsoft.com/en-us/graph/api/subscription-update?WT.mc_id=M365-MVP-5003693)
- [Get change notifications for teams and channels using Microsoft Graph](https://learn.microsoft.com/en-us/graph/teams-changenotifications-team-and-channel?WT.mc_id=M365-MVP-5003693)
- [Use Postman with the Microsoft Graph connectors API](https://learn.microsoft.com/en-us/graph/connecting-external-content-connectors-api-postman?WT.mc_id=M365-MVP-5003693)
- [Real-time presence with Microsoft 365, Azure, and Power Platform](https://learn.microsoft.com/en-us/azure/architecture/solution-ideas/articles/presence-microsoft-365-power-platform?WT.mc_id=M365-MVP-5003693)
