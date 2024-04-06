---
title: "Build SharePoint Copilot with Microsoft Copilot Studio"
date: "2024-04-06"
share: true
header:
  image: media/2024-04-06-sp-copilot-studio/16.png
  teaser: media/2024-04-06-sp-copilot-studio/16.png
categories:
  - Microsoft 365
  - Copilot
  - SharePoint
tags:
  - "2024"
  - April 2024
last_modified_at: 2024-04-06T00:00:00-00:00
---
## Overview

Over the past 2 decades, SharePoint has been serving as a main building block for businesses working together and managing information. The role of SharePoint for generating answers in the Microsoft Copilot Studio is not just supportive but foundational.

This article is all about how we use SharePoint to find answers in Microsoft Copilot Studio. Doing this well means organizing content smartly and knowing how SharePoint and Copilot Studio's advanced AI work together.


## Why SharePoint as data source?

SharePoint's integration as a data source in Microsoft Copilot Studio is a game-changer for organizations. It helps them use all their knowledge better and make decisions faster. SharePoint has been the preferred content management solution for various Organization that helps to collate the information at one place. Copilot Studio can use this information to give accurate answers.

From project documentation and reports to internal communications and collaborative content, SharePoint houses diverse data. By indexing this data, Copilot Studio can perform advanced searches across the SharePoint ecosystem, ensuring that the answers generated are not only accurate but also reflect the latest updates and insights.

Moreover, SharePoint's robust security and compliance features ensure that sensitive data remains protected while being utilized by Copilot Studio. This is particularly important for enterprises that operate in regulated industries or handle confidential information.

In essence, SharePoint as a data source empowers Microsoft Copilot Studio to act as an intelligent assistant, capable of providing informed answers and suggestions.


## Sample SharePoint Content

For the demonstration purpose of this article, I have set up a SharePoint communication site named "Margie Travels" for a travel agency. The standard out of box "Documents" library is populated with brochures of places where they provide the travel arrangements.

![](/media/2024-04-06-sp-copilot-studio/01.png)

Each brochure represents a city travel and hotel accommodation information.


## Create a Copilot in the Microsoft Copilot Studio

Follow the below steps to set up a Copilot:

1. Navigate to Microsoft Copilot Studio at [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/).
2. Create a new Copilot named "Travel Copilot".

    You may enter the SharePoint site URL in the below highlighted textbox while you set up the Copilot.

    ![](/media/2024-04-06-sp-copilot-studio/02.png)

    There are 2 options while you set up the SharePoint website:

3. Specify the **SharePoint Site URL** and the Copilot will surface the information from all document libraries.
4. Specify the URL to document library and Copilot will surface the information only from the specific document library. I will leave this textbox empty as of now and we will configure the SharePoint source later.

5. Click **Create**.


## Set SharePoint Site as source for Generative AI

Follow the below steps to enable the Generative AI and specify SharePoint as data source for the Copilot:

1. From the left menu, under **Settings**, click **Generative AI**.
2. Under **Use AI features in your copilot**, enable **Boost conversational coverage with generative answers**.
3. In the Instructions box provide information about how the copilot should behave or respond to users. For example: _You are a travel assistant that helps people find information about various cities, travel, hotels, and accommodation. Only answer questions related to travel topics. If you are not sure of the answer, tell to look for more information at SharePoint site._

    ![](/media/2024-04-06-sp-copilot-studio/03.png)

4. On the same page, scroll up to specify the SharePoint site or URL to SharePoint document library. Note, the URL cannot be more than two levels deep.

    ![](/media/2024-04-06-sp-copilot-studio/04.png)

5. Click **Save**.
6. Ask a travel related question to the Copilot and it can answer from SharePoint.

    ![](/media/2024-04-06-sp-copilot-studio/05.png)

If you set the tracking on, you will observe the answer is generated from a topic called "Conversational boosting".


## Conversational boosting topic

Conversational boosting topic enhances the Copilot responses with contextual answers from websites, SharePoint sites, documents, etc.

Although everything seems to work perfectly, if you open up the topic, you will notice an error at the top in the Topic checker.

![](/media/2024-04-06-sp-copilot-studio/06.png)

The error talks about the AuthenticationInputNotEnabled (authentication input not enabled).


Let us fix this by enabling the authentication with below steps:

1. Go to **Settings** \> **Security**.
2. Click **Authentication**.
3. Select **Authenticate manually**.

    ![](/media/2024-04-06-sp-copilot-studio/07.png)

    - **No Authentication**: When your copilot doesn't require your users to sign in when interacting with the copilot.
    - **Only for Teams and Power Apps:** Automatically sets up Microsoft Entra ID authentication for Teams without the need for any manual configuration.
    - **Authenticate manually**: configure any Microsoft Entra ID v1, Microsoft Entra ID, or OAuth2-compatible identity provider with this option.

    **Require users to sign in** options determines whether a user needs to sign in before interacting with the copilot.


## Set up Microsoft Entra App registration

Follow the below steps to set up Microsoft Entra App registration and refer the values back to the Copilot authentication.

1. Navigate to the [Azure portal](https://portal.azure.com/).
2. From the left menu, click **App registrations**.
3. You can create a new App registration or can use existing app registration created for your Copilot as well.

    ![](/media/2024-04-06-sp-copilot-studio/08.png)

I will use the existing App registration to make further changes.


**Add Redirect URI**

Add the Redirect URI (for e.g. https://token.botframework.com/.auth/web/redirect) shown in the copilot authentication panel.

![](/media/2024-04-06-sp-copilot-studio/09.png)


**API permissions**

Add the below Graph API permissions as delegated.

![](/media/2024-04-06-sp-copilot-studio/10.png)


**Generate Client secret**

Navigate to Certificates & secrets and generate a new client secret.

![](/media/2024-04-06-sp-copilot-studio/11.png)


**Update the Copilot configuration**

Once the Microsoft Entra App registration is set up, reflect the values to Copilot configuration.

![](/media/2024-04-06-sp-copilot-studio/12.png)

As suggested in the dialog, make sure to publish the copilot. Navigate back to the Conversational boosting topic and make sure no validation error is present.

As we had selected the option **Require users to sign in** in the Copilot configuration, user needs to sign at least once.

![](/media/2024-04-06-sp-copilot-studio/13.png)

Click the Login button, in the new windows it will generate a code. Paste the code as chat message to start a discussion with your Copilot.

![](/media/2024-04-06-sp-copilot-studio/14.png)


## Publish the Copilot

Once you are happy with the Copilot, navigate to **Settings** \> **Channels** and publish the Copilot to your desired channel.

![](/media/2024-04-06-sp-copilot-studio/15.png)


## Travel Copilot plugin experience in M365 Copilot

Type your travel related query into M365 Copilot and it will pick up our travel copilot to answer it as follows.

![](/media/2024-04-06-sp-copilot-studio/16.png)


## Summary

The role of SharePoint for generating answers in the Microsoft Copilot Studio is not just supportive but foundational. SharePoint as a data source empowers Microsoft Copilot Studio to act as an intelligent assistant, capable of providing informed answers and suggestions.


## References

- [Configure user authentication in Microsoft Copilot Studio](
https://learn.microsoft.com/en-us/microsoft-copilot-studio/configuration-end-user-authentication?WT.mc_id=M365-MVP-5003693)
- [Configure user authentication with Microsoft Entra ID](
https://learn.microsoft.com/en-us/microsoft-copilot-studio/configuration-authentication-azure-ad?WT.mc_id=M365-MVP-5003693)
- [Use content on SharePoint or OneDrive for Business for generative answers](
https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-generative-answers-sharepoint-onedrive?WT.mc_id=M365-MVP-5003693)
