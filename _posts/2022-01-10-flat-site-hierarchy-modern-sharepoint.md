---
title: "Flat Site Hierarchy in Modern SharePoint"
date: "2021-12-23"
share: true
header:
  image: media/2022-01-10-flat-site-hierarchy-modern-sharepoint/02.gif
  teaser: media/2022-01-10-flat-site-hierarchy-modern-sharepoint/02.gif
categories:
  - SharePoint
  - Office 365
tags:
  - "2022"
  - January 2022
last_modified_at: 2022-01-10T00:00:00-00:00
---
## Overview

For engaging, high performant, and robust intranet with SharePoint, well-planned information architecture is the key to success. As we are making our way into Modern SharePoint, it is time to think over a traditional nested hierarchy of sites vs a flat hierarchy structure.

In this article, we will explore the flat structure benefits over traditional nested hierarchy.

## Nested hierarchy structure is the past

In the past, organizations preferred the nested hierarchy structure while designing the information architecture for SharePoint sites. We typically had site collections with several subsites, permission structures, and inherited navigation.

This approach was rigid because once built it was always tough to move the sites from one location to another. A lot of thinking had to be done up-front before we go ahead and create the site structure.

In the below example, moving Sub Site A3 from Site Collection A to Site Collection B requires migration of content and permissions.

![](/media/2022-01-10-flat-site-hierarchy-modern-sharepoint/01.png)

The permission management on sub-sites was always challenging. By default, the subsite inherits the permissions from the parent site. Stop inheriting permissions and maintaining unique permissions was a difficult task.

## Flat Architecture

Modern SharePoint architecture promotes flat hierarchy, where every site is a site collection that represents the unique responsibility. Since we do not have a physical hierarchy, permission management is relatively easy.

One might think, in this case, we will have a huge number of site collections and it will be difficult to relate them, get to the relevant content faster. The obvious answer to this is to plan for the hub sites.

**Hub site**

Hub site helps to connect and organize the flat site collections based on the business needs by providing benefits like:

- Common navigation, branding across connected sites
- Search across the connected sites
- Roll up the content from connected sites to the hub site at one place

The below animation shows, how easy it is to organize sites better with hub sites.

![](/media/2022-01-10-flat-site-hierarchy-modern-sharepoint/02.gif)

Image reference: [https://docs.microsoft.com/en-us/sharepoint/planning-hub-sites](https://docs.microsoft.com/en-us/sharepoint/planning-hub-sites?WT.mc_id=M365-MVP-5003693)

## Flat Architecture Benefits

Since every site is an individual site collection, flat architecture offers various benefits:

**Site movement**

It is easier to disassociate a site from one hub and associate it with another hub.

**Site deletion**

With nested hierarchy deleting a site with sub-sites was not possible. In flat architecture, it is an easy decision.

**Adopt to organization change**

With flat architecture, it is very easy to modify the site hierarchy without the need for any migrations.

**Managing security**

In flat architecture, we no more have to Stop inheriting permissions and deal with complex permission models. Permission can be maintained at each site collection level.

**External sharing**

In a nested hierarchy, external sharing cannot be turned on for a single subsite but had to be on the site collection. In flat architecture, this can be achieved easily without impacting other site collections.

## Summary

A flat hierarchy is a preferred way to go with modern SharePoint. It helps you to build long-term information architecture. With flat hierarchy, you can better manage permissions, branding, and security.

## References

- [Introduction to SharePoint information architecture](https://docs.microsoft.com/en-us/sharepoint/information-architecture-modern-experience?WT.mc_id=M365-MVP-5003693)
- [Planning your SharePoint hub sites](https://docs.microsoft.com/en-us/sharepoint/planning-hub-sites?WT.mc_id=M365-MVP-5003693)
- [Information architecture models and examples](https://docs.microsoft.com/en-us/sharepoint/information-architecture-models-examples?WT.mc_id=M365-MVP-5003693)