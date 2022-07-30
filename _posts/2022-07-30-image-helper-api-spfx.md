---
title: "Using Image Helper API in SPFx"
date: "2022-07-30"
share: true
header:
  image: media/2022-07-30-image-helper-api-spfx/01.png
  teaser: media/2022-07-30-image-helper-api-spfx/01.png
categories:
  + SharePoint Framework
tags:
  + "2022"
  + July 2022
last_modified_at: 2022-07-30T00:00:00-00:00
---

## Overview

Image Helper API helps to generate an optimized URL to the images stored in SharePoint. This feature is generally available in SharePoint Framework v1.15. It was introduced in the earlier release (SPFx v1.14) as a developer preview feature.

In this article, we will explore the advantages of Image Helper API and how to use it.

## Image Helper API

The `ImageHelper` is available as a static class in the `@microsoft/sp-image-helper` namespace. It helps to generate optimized URLs for the images in SharePoint.

`ImageHelper.convertToImageUrl()` method accepts argument of type IImageHelperRequest with below parameters:

* sourceUrl (required): Path of an image stored in SharePoint
* width (optional): Width to resize image to. SharePoint supports resolution for 200, 400, 960, 1600, and 2560. Any value specified apart from this will get converted to the nearest largest resolution.
* height (optional): Height to resize an image to. If not specified, the aspect ratio will be maintained by SharePoint.

## How to use Image Helper API in the SPFx solution?

As this feature is generally available in SPFx v1.15, we do not need to install additional packages.

Add below import

```typescript
import { ImageHelper, IImageHelperRequest } from '@microsoft/sp-image-helper';
```

Use the functionality as follows

```typescript
const resizedImage = ImageHelper.convertToImageUrl(
    {
        sourceUrl: originalImageUrl,
        width: 200
    } as IImageHelperRequest
);
```

I am working on using this functionality in one of my upcoming ACE solution. The resized image URL looks as follows:

![](/media/2022-07-30-image-helper-api-spfx/01.png)

## Summary

Image Helper API helps to generate an optimized URL to the images stored in SharePoint. This feature is generally available in SharePoint Framework v1.15.

## References

- [Image Helper API](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/image-helper-api?WT.mc_id=M365-MVP-5003693)
- [SharePoint Framework v1.15 release notes](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/release-1.15?WT.mc_id=M365-MVP-5003693)
