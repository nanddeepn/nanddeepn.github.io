---
title: "Build Leave Approval System with SharePoint and Power Platform"
date: "2020-11-22"
share: true
header:
  image: media/2020-11-22-leave-approval-with-spo-and-power-platform/11.gif
  teaser: media/2020-11-22-leave-approval-with-spo-and-power-platform/11.gif
categories:
  - SharePoint
  - Power Apps
  - Power Automate
tags:
  - "2020"
  - November 2020
last_modified_at: 2020-11-22T00:00:00-00:00
---

## Overview

SharePoint is well known for content management. With the power of Power Platform, we can extend the experience to build the automated systems.

In this article, we are going to explore one of such automation for leave approval.

## Scenario

The scenario is very straight forward:

- Employees can log their leaves through Power App
- The Power App app is built on top of the SharePoint Calendar list
- Power Automate will handle manager approval for the leave

## SharePoint List

We will start by creating a Calendar list named "Leave Calendar".

![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/01.png)

Let us add a custom column to the Calendar list as defined below:

| **Name** | Leave Type | Approval Status |
| --- | --- | --- |
| **Type** | Choice | Choice |
| **Choices** | Casual LeaveCompensatory OffsEarned LeaveSick LeaveOther | PendingApprovedRejected |
| **Display choices using** | Drop-Down Menu | Drop-Down Menu |

## Build Power Apps app

In the next step, we will build a Power App app that can be used by remote users to apply for leave.

**Create the app**

Follow the below steps to build the Power Apps app:

1. Navigate to the Power Apps portal ([https://make.powerapps.com](https://make.powerapps.com/))
2. From the left menu, click **Create**.
3. Under **Start from data** , select **SharePoint**.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/02.png)

4. Add a connection to the SharePoint list.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/03.png)

**Update the screens**

We will update the default browse, default, and edit screen by following the below steps:

1. Open **EditScreen1** and delete Approval Status, Location, Category, and Attachments cards from the screen.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/04.png)

2. Similarly, open **DetailScreen1** and delete Location, Category, and Attachments cards from the screen.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/05.png)

3. On the BrowseScreen1, Add an icon with the below formula:

    ```
    Switch(ThisItem.'Approval Status'.Value,
    "Pending",
    Icon.EmojiNeutral,
    "Approved",
    Icon.EmojiSmile,
    Icon.EmojiSad)
    ```

4. Add a circle icon in the background of the above icon with the below formula for fill:

    ```
    Switch(ThisItem.'Approval Status'.Value,
    "Pending",
    Yellow,
    "Approved",
    Green,
    Red)
    ```

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/06.png)

5. From the ribbon, click **File**.
6. From the left menu, click **Settings**.
7. Specify Name, Icon, and background color for the app.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/07.png)

8. From the left menu, click **Save**.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/08.png)

9. Once saved, you will be able to share the app with others.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/09.png)

10. From the next changes onwards, you will be able to see the Publish option.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/10.png)

Once published, the Leave management Power Apps app will function as shown below:

![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/11.gif)


## Build Power Automate Flow

As the last part to leave approval, we will build a power automate flow to send the leave request for manager approval.

Follow the below steps to build the Power automate flow:

1. Open Power Automate portal.
2. Search for approval templates.
3. Choose **Request manager approval for leave requests**.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/12.png)

4. Update the Site address and List name in the ready template.

    ![](/media/2020-11-22-leave-approval-with-spo-and-power-platform/13.png)


## Summary

SharePoint along with Power Platform can be used to automate our business processes. In this article, we explored the creation of a leave approval system using SharePoint integration with Power Platform. Power Apps supports the creation of apps for mobile and web. Power Automate helps to automate the business processes (e.g. approvals) by designing flows.


## References

- [Power apps gallery conditional formatting](https://wonderlaura.com/2020/07/23/power-apps-gallery-conditional-formatting/) by _Laura Rogers_
- [Power Apps](https://powerapps.microsoft.com/)
- [Power Automate](https://flow.microsoft.com/)