---
title: "Setting Dynamic Options in Microsoft Copilot Studio"
date: "2025-01-22"
share: true
header:
  image: media/2025-01-22-copilot-studio-dynamic-options/13.png
  teaser: media/2025-01-22-copilot-studio-dynamic-options/13.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2025"
  - January 2025
last_modified_at: 2025-01-22T00:00:00-00:00
---
## Overview

Microsoft Copilot Studio is a powerful platform that allows everyone to create customized AI experiences for their business needs. One of its handy features is the ability to set dynamic dropdown options. This means you can provide dropdown menus in your Copilot-powered applications that update automatically based on user input, data changes, or external sources.

In this article, we will explore how to use this feature effectively.

## What Are Dynamic Dropdowns?

Dynamic dropdowns are menus where the available options are not fixed. Instead, they are generated in real time, depending on the context or the data provided. For example:

- In a CRM system: A dropdown could show customer names pulled from your database.
- In a task management tool: The dropdown could list tasks assigned to a specific team member.
- In an e-commerce app: It could display product categories or stock availability.

This flexibility makes your Copilot solutions more intelligent and tailored to user needs.

## Set Up Dynamic Dropdowns in Copilot Studio

Follow the below steps:

### Set up an agent

1. Open Microsoft Copilot Studio.
2. Create **New agent**.
3. Add SharePoint site as a knowledge source. The SharePoint site contains a document library with `travel brochures`.

### Add a custom topic  

1. Add a topic named Hotel Reservations with below phrases:

    - book a hotel room
    - hotel reservation
    - reserve a room at the hotel
    - check hotel availability
    - make a reservation at the hotel
    - hotel booking
    - room reservation at the hotel
    - find available rooms at the hotel

    ![](/media/2025-01-22-copilot-studio-dynamic-options/01.png)

2. Add a **Generative Answers** node.

    - Data Sources: Set the data source as our SharePoint site.
    - Input: Add input as - `Suggest me hotel names in Dubai as comma separated`.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/02.png)

3. Under **Advanced**, create a string variable to save the bot response.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/03.png)

### Add a flow

1. Create a flow by adding a node as follows:

    ![](/media/2025-01-22-copilot-studio-dynamic-options/04.png)

2. Add a **Text input parameter** named **HotelList** to pass the Generative answers node output.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/05.png)

3. Initialize string variable CleanHotelString with formula as `first(split(triggerBody()?\['text'\],'\['))`.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/06.png)

4. Split the variable as comma separated as `split(variables('CleanHotelString'), ',')`.

5. Initialize variable CleanedHotelNamesArray as an array.

6. Use Apply to each to clean the hotel name string and add to array.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/07.png)

7. Return CleanedHotelNamesArray as an output to Copilot.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/08.png)

8. Save the flow as - Get Hotel Listings for Booking and publish.

### Continue with custom topic

Navigate back to the Copilot Studio to follow below steps:

1. Add the flow node.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/09.png)

2. The node will be displayed as follows

    ![](/media/2025-01-22-copilot-studio-dynamic-options/10.png)

3. Use Parse value to parse the string of Hotels to table. Click Edit schema to update the schema.

    ![](/media/2025-01-22-copilot-studio-dynamic-options/11.png)

4. Use the Question and Message node as follows to show the list of options to the user and show the confirmation

    ![](/media/2025-01-22-copilot-studio-dynamic-options/12.png)

## Test the Copilot

Test the copilot by prompting about the hotel reservation.

![](/media/2025-01-22-copilot-studio-dynamic-options/13.png)

## Benefits of Dynamic Dropdowns

- **Improved User Experience:** Users only see relevant and up-to-date options, saving time and reducing errors.
- **Increased Efficiency:** No need for manual updates. Dropdowns update automatically as data changes.
- **Enhanced Customization:** Tailor the dropdown options to individual users, roles, or scenarios.
- **Seamless Integration:** Dynamic dropdowns can pull data from almost any source integrated with.

## Best Practices for Dynamic Dropdowns

- **Keep it Simple:** Avoid overcrowding the dropdown with too many options.
- **Ensure Data Accuracy:** Connect to reliable and up-to-date data sources.
- **Test Extensively:** Ensure the dropdown works well under different scenarios and with various user inputs.
- **Handle Errors Gracefully:** If the data source is unavailable, provide a fallback or an error message.

## Summary

Dynamic dropdowns in Microsoft Copilot Studio empower us to create smart, user-friendly applications. By automatically updating options based on data, they save time, improve accuracy, and provide a seamless experience for users.
