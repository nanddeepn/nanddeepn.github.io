---
title: "Building Multilingual Copilots with Microsoft Copilot Studio"
date: "2024-03-10"
share: true
header:
  image: media/2024-03-10-multilingual-copilots-copilot-studio/12.png
  teaser: media/2024-03-10-multilingual-copilots-copilot-studio/12.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - March 2024
last_modified_at: 2024-03-10T00:00:00-00:00
---
## Overview

Multilingual copilots are like assistants that can talk to users in various languages, all within one assistant. They can figure out the language you want to use by checking your internet browser settings and then respond in that language. This makes the conversation more personal and interesting for users.

In this article, we will explore the multilingual copilots configured from the Microsoft Copilot Studio.

## Set up a topic for Copilot

Follow the below steps to set up a Copilot topic:

1. Navigate to Microsoft Copilot Studio at [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/).
2. From the **Settings** menu, click **General settings**.
3. Toggle the **Build and iterate on your topics by describing what you want** option to **On**.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/01.png)

4. Create a new Copilot named "Multilingual Copilot".
5. From the left menu, select **Topics & Plugins** \> **Create**\> **Topic** \> **Create from description with Copilot**.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/02.png)

6. Name the topic as **Order placement.**
7. In the **Create a topic to...** textbox, type **Let the user select items from the menu and place the order**.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/03.png)

8. Click **Create**.
9. The new topic will be generated with trigger phrases.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/04.png)


## Set up a Copilot to work with multiple languages

Follow the below steps to add language support to Copilot:

1. From the left menu, click **Settings** \> **Languages**.
2. Click **Add languages**.
3. Select the languages to add.
4. Click **Add Languages**.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/05.png)


## Manage localization

All Copilot topic and content editing must be done in the Copilot's primary language. Follow the below instructions to translate the text for the Copilot's secondary language. After localizing and uploading the copilot strings, you can see the translated content in the authoring canvas by switching the language in the test copilot.

1. For a language, click the **Update localizations** icon.
2. Click **Download localization file as JSON** to download the current localization file for that language.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/06.png)

3. Update the Copilot's messages in English with the selected secondary language.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/07.png)

4. On the **Update localizations** page, select **Browse**, and upload the translated file.
5. Click **Update localizations**.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/08.png)


## Authoring for multilingual copilots

When authoring, you can configure the copilot to change the current spoken language in the middle of a conversation.

To change the copilot's current language, you can set the User.Language variable value to one of your copilot's secondary languages. This selection changes the language spoken by your copilot immediately.

Follow the below steps:

1. Open the "Order placement" topic.
2. Add a **Variable management** \> **Set a variable value** node.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/09.png)

3. Select `User.Language` variable under the **System** tab. Set the value to localized language (for e.g., Hindi).

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/10.png)

4. Click **Save**.


## Test the multilingual Copilot with a secondary language

Follow the below steps to test the multilingual capabilities of the Copilot:

1. From the left navigation, click **Test your copilot**.
2. Select the secondary language.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/11.png)

3. The copilot will reload using the selected secondary language. The authoring canvas should show the text in the secondary language.
4. Enter a message in the secondary language to test the copilot.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/12.png)


## Test the multilingual Copilot with language variable

Follow the below steps to test the multilingual Copilot by setting the language variable.

1. From the left navigation, click **Test your copilot**.
2. Select the language as English.
3. Use the same prompts from the previous test.
4. After the **Set variable value** node execution, the language automatically changes to Hindi, as we have configured the topic to change the language using the `User.Language` variable.

    ![](/media/2024-03-10-multilingual-copilots-copilot-studio/13.png)


**How does Copilot act with languages that haven't been set up?**

If the language in your web browser is not set up in Copilot, Copilot will always use its main language (the one it was originally made in). You can't change Copilot's main language once it's created.

**How does Copilot act when there are no translations available for certain languages?**

If the Copilot author makes a change in the main language but doesn't add translations, Copilot will display the changes without translations. This can make it confusing with two languages. Always update translations after changing things in the main language to avoid confusion.


## Summary

In this article, we have explored how to set up multiple languages, update the local versions, and test them in Copilot using Microsoft Copilot Studio.


## References

- [Configure and create multilingual copilots](https://learn.microsoft.com/en-us/microsoft-copilot-studio/multilingual?WT.mc_id=M365-MVP-5003693)
- [Supported languages](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-language-support?WT.mc_id=M365-MVP-5003693)
