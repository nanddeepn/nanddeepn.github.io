---
title: "Using Bing Custom Search with Microsoft Copilot Studio for Generative Answers"
date: "2024-03-23"
share: true
header:
  image: media/2024-03-23-bing-search-copilot/09.png
  teaser: media/2024-03-23-bing-search-copilot/09.png
categories:
  - Microsoft 365
  - Copilot
tags:
  - "2024"
  - March 2024
last_modified_at: 2024-03-23T00:00:00-00:00
---
## Overview

Using the Bing search engine API, you can find public available data. You can either use the default Bing Search or customize your search with specific content using Bing Custom Search. Bing Search makes it easier to find what you need by searching through lots of sources at once.

In this article, we will explore how to set up your own Bing Custom Search and use it to get helpful answers on a specific topic for generative answers with Microsoft Copilot Studio.

## Setup Custom Bing Search

To use the custom Bing search with our Copilot, we need to provision custom Bing Search resource and get the key by following the below steps:

1. Navigate to [https://customsearch.ai/](https://customsearch.ai/).
2. Click **Get Started**. Log-in if prompted.
3. Click **Create new instance**.
4. Name your custom search instance.

  ![](/media/2024-03-23-bing-search-copilot/01.png)

5. Click **Ok**.
6. Enter any URL from the public domain. For this article, I will mention my blog site URL.
7. Try out the search experience in the right-side panel to preview the results.

  ![](/media/2024-03-23-bing-search-copilot/02.png)

8. Click **Publish** to publish the results.

  ![](/media/2024-03-23-bing-search-copilot/03.png)

9. On the successful publish, click **Go to production environment**.

  ![](/media/2024-03-23-bing-search-copilot/04.png)

10. This will load the custom search endpoint details. Note down the **Custom Configuration ID**.

  ![](/media/2024-03-23-bing-search-copilot/05.png)


## Use Custom Bing Search for generative answers

We will now use the custom Bing search for generative answers in Microsoft Copilot Studio. Follow the below steps:

1. Open [https://copilotstudio.microsoft.com/](https://copilotstudio.microsoft.com/).
2. Create a new Copilot named "Bing Custom Search Copilot".
3. From the left navigation, click **Topics**.
4. Under **System** tab, open the **Converstional boosting** topic.

  ![](/media/2024-03-23-bing-search-copilot/06.png)

5. Locate the **Create generative answers** node. It has various data sources like Public website, Custom Bing Search, SharePoint, Azure OpenAI Services on your data.

  ![](/media/2024-03-23-bing-search-copilot/07.png)

6. Under **Search public data**, Select **Search with Bing Custom Search** option.
7. Specify the **Customer Configuration ID** generated in the previous section.

  ![](/media/2024-03-23-bing-search-copilot/08.png)

9. Save the changes.


## Test the Copilot

Follow the below steps to test the Copilot working with custom Bing search.

1. From the left pane, select **Test your copilot**.
2. Type your question related to information indexed in Bing search.

  ![](/media/2024-03-23-bing-search-copilot/09.png)


## Summary

Using the Bing search engine API, you can find public available data. We explored how to configure a Bing Search instance and use it in a Copilot using Microsoft Copilot Studio.

## References

- [Microsoft Copilot Studio](https://www.microsoft.com/en/copilot/microsoft-copilot-studio?WT.mc_id=M365-MVP-5003693)
- [Search public data or use a Bing Custom Search for generative answers](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-generative-answers-bing?WT.mc_id=M365-MVP-5003693)
