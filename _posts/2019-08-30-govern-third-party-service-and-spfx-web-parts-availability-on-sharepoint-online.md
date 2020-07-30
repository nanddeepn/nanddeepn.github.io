---
title: "Govern Third-Party Service and SPFx Web Parts Availability on SharePoint Online"
date: "2019-08-30"
share: true
categories:
  - SharePoint
  - SharePoint Framework
header:
  image: media/2019-08-30-govern-third-party-service-and-spfx-web-parts-availability-on-sharepoint-online/01.png
  teaser: media/2019-08-30-govern-third-party-service-and-spfx-web-parts-availability-on-sharepoint-online/01.png
tags:
  - "2019"
  - August 2019
last_modified_at: 2019-08-30T00:00:00-00:00
---

## Overview

The modern SharePoint is becoming richer with the addition of newer web part capabilities. As a content author, it gives wider options to present the content in meaningful and different ways by utilizing the available web parts. This certainly helps in building engaging content. The wide variety of web parts includes capabilities to display information within SharePoint, as well as going beyond SharePoint and fetch information from third-party services outside of Office 365 like Twitter, YouTube, Kindle, etc. However, as a SharePoint or content administrator, you would like to have control over this and govern the process.

In this article, we will explore how a SharePoint administrator can control the third-party service web parts to be made available to end-users.


## What are third-party service web parts?

These web parts pull the data from services outside of Office 365. The example includes Twitter, YouTube, Amazon Kindle.


## Disable third-party service web parts

These web parts can be disabled by running below PowerShell.

```
Set-SPOTenant -DisabledWebPartIds (guid[])
```

Please note, these web parts can be disabled at the tenant level. We cannot selectively disable them on a particular SharePoint site.

To disable the YouTube web part, use the below command.

```
Set-SPOTenant -DisabledWebPartIds 544dd15b-cf3c-441b-96da-004d5a8cea1d
```

To disable multiple web parts at once, use the below command.

```
Set-SPOTenant -DisabledWebPartIds 544dd15b-cf3c-441b-96da-004d5a8cea1d, f6fdf4f8-4a24-437b-a127-32e66a5dd9b4
```

Below are GUID values for out of box third-party service web parts.

- YouTube (544dd15b-cf3c-441b-96da-004d5a8cea1d)
- Twitter (f6fdf4f8-4a24-437b-a127-32e66a5dd9b4)
- Amazon Kindle (46698648-fcd5-41fc-9526-c7f7b2ace919)


**How to get the web part GUID?**

In order to get the GUID of the web part, firstly place the web part on the page and open the web part maintenance mode by typing **?maintenancemode=true** at the end of page URL.

![](/media/2019-08-30-govern-third-party-service-and-spfx-web-parts-availability-on-sharepoint-online/01.png)

Click **Copy data** to copy the summary of any web part. The Id parameter represents the GUID of the web part. Remove **?maintenancemode=true** from the end of the page URL to leave the maintenance mode.

Use the below command to re-enable all disabled web parts.

```
Set-SPOTenant -DisabledWebPartIds @()
```

To see a list of disabled web parts, use the below command.

```
Get-SPOTenant | Select DisabledWebPartIds
```


## Hide Custom SPFx web parts

To hide custom SPFx web part from the toolbox, set hiddenFromToolbox setting to true inside web part's manifest.json.

```json
{  
  "$schema": "https://dev.office.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",  
  "id": "34f6d7f2-262f-460a-8b65-152d784127cb",  
  "alias": "HelloWorldWebPart",  
  "componentType": "WebPart",  
  
  // The "*" signifies that the version should be taken from the package.json  
  "version": "*",  
  "manifestVersion": 2,  
  
  // If true, the component can only be installed on sites where Custom Script is allowed.  
  // Components that allow authors to embed arbitrary script code should set this to true.  
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f  
  "requiresCustomScript": false,  
  "hiddenFromToolbox": true,  
  
  "preconfiguredEntries": [{  
    "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other  
    "group": { "default": "Other" },  
    "title": { "default": "HelloWorld" },  
    "description": { "default": "HelloWorld description" },  
    "officeFabricIconFontName": "Page",  
    "properties": {  
      "description": "HelloWorld"  
    }  
  }]  
}
```

When hiddenFromToolbox is set to true, the web part will not appear in the toolbox when you edit the page.


## Points to note

1. Only SharePoint administrators can govern this setting.
2. Web parts utilizing third party services (Twitter, YouTube, and Amazon Kindle) can be disabled.
3. SPfx web parts can be disabled by setting hiddenFromToolbox to true inside manifest.json.
4. If disabled, web parts previously added on the page will not be rendered.


## Summary

SharePoint administrator can control the third-party service web parts to be made available to end-users using PowerShell. Web parts utilizing third party services (Twitter, YouTube, and Amazon Kindle) can be disabled using this approach. In a similar way, custom SPFx web parts can be disabled setting hiddenFromToolbox to true inside manifest.json.


## References

- [https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/set-spotenant](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/set-spotenant)
- [https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/basics/hide-web-part-from-toolbox](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/basics/hide-web-part-from-toolbox)

This content was originally posted [here](https://www.c-sharpcorner.com/article/govern-third-party-service-and-spfx-web-parts-availability-on-sharepoint-online/).
