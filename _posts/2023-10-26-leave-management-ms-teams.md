---
title: "Use Case: Leave Management within Microsoft Teams Team"
date: "2023-10-17"
share: true
header:
  image: media/2023-10-26-leave-management-ms-teams/04.png
  teaser: media/2023-10-26-leave-management-ms-teams/04.png
categories:
  - MS Teams
tags:
  - "2023"
  - October 2023
last_modified_at: 2023-10-26T00:00:00-00:00
---
## Overview

Recently, I was working on the use case for leave management within the Microsoft Teams team. The solution was developed using Microsoft Teams, SharePoint, and Power Automate.

In this article, I will discuss the use case, the issues faced, and the resolution.


## The Use Case

As a team inside Microsoft Teams team, we wanted to develop a leave management solution.

**Leave Calendar**

The leaves are maintained on the calendar list named "Leave Calendar" on the SharePoint site connected to the Microsoft Teams team.

**Approval process**

When a new leave request is created to the leave calendar, a Power Automate flow attached to the item created event would trigger and send the leave for the approval process.

**The problem statement**

Once the leave is approved, it should be shown on the calendar (inside Outlook).


## Is this a straightforward ask?

It might look to be a straightforward one, but it was a bit tricky as it involved connecting two pieces together i.e., connecting the SharePoint calendar with the Outlook calendar.

Let's take a simple example by creating a leave request in the SharePoint calendar.

![](/media/2023-10-26-leave-management-ms-teams/01.png)

Although it was approved, it was not visible on the Outlook calendar.

![](/media/2023-10-26-leave-management-ms-teams/02.png)

And to some extent, yes, it is an expected behavior because SharePoint calendar and Outlook calendar (Shared mailbox) are not connected.


## The Solution

In the Power Automate flow, use the "Create a group event (v2)" activity.

![](/media/2023-10-26-leave-management-ms-teams/03.png)

The formulas used are as follows:

Start Time:

```
formatDateTime(triggerOutputs()?['body/EventDate'],'yyyy-MM-ddTHH:mm:ss')
```

End Time:

```
formatDateTime(triggerOutputs()?['body/EndDate'],'yyyy-MM-ddTHH:mm:ss')
```

The events (i.e., leave requests) should not appear in the Shared mailbox.

![](/media/2023-10-26-leave-management-ms-teams/04.png)


## Summary

The "Create a group event" activity in Power Automate can be used effectively to create the events in Shared mailboxes.
