---
title: "Using Azure OpenAI APIs in Bots with Teams Toolkit"
date: "2023-06-19"
share: true
header:
  image: media/2023-06-19-az-oai-teams-toolkit-bot/04.png
  teaser: media/2023-06-19-az-oai-teams-toolkit-bot/04.png
categories:
  - MS Teams
  - OpenAI
tags:
  - "2023"
  - June 2023
last_modified_at: 2023-06-19T00:00:00-00:00
---
## Overview

Teams Toolkit helps developers develop apps for Microsoft Teams. Adding AI capabilities to your Teams app can be easily done with Azure OpenAI.

In this article, we will explore adding Azure OpenAI API capabilities to bots created with Teams Toolkit with easy-to-follow and step-by-step instructions.


## Teams Toolkit Overview

Teams Toolkit is available as an extension for Visual Studio Code and Visual Studio 2022 to help developers create, debug, and deploy Microsoft Teams apps.

In Visual Studio Code, it can be installed as an extension. Follow the below steps to install:

1. Open Visual Studio Code.
2. Click **View** > **Extensions**.
3. Find and install **Teams Toolkit**.

    ![](/media/2023-05-07-openap-apis-teams-toolkit-bot/01.png)


## Create a Bot solution

Follow the steps below to create a bot solution with Teams Toolkit.

1. Open Visual Studio Code.
2. From the left menu, click Teams Toolkit.
3. Choose to **Create a new app**.
4. Select an option: **Create a new Teams app**.

    ![](/media/2023-05-07-openap-apis-teams-toolkit-bot/02.png)

5. Select **Command bot** to create.

    ![](/media/2023-05-07-openap-apis-teams-toolkit-bot/03.png)

6. Choose a programming language as **TypeScript**.
7. Select a folder location to scaffold the project.
8. Enter the **Application Name** as **OpenAIChatBot**.

Once the project is scaffolded, you can select **Run and Debug** from the left menu. Choose the run profile and test the bot solution.

![](/media/2023-05-07-openap-apis-teams-toolkit-bot/04.png)


## Azure OpenAI npm package

We will use the Azure OpenAI npm package available at [https://www.npmjs.com/package/@azure/openai](https://www.npmjs.com/package/@azure/openai) in this solution. The OpenAI Node.js library provides convenient access to the OpenAI API from Node.js applications.

Install the package by running the below command:

```powershell
npm install @azure/openai
```


## Azure OpenAI

Follow the below steps to set up Azure OpenAI:

![](/media/2023-06-19-az-oai-teams-toolkit-bot/01.png)

1. Create an Azure OpenAI service in the Azure portal.
2. In the Azure AI Studio, From the left navigation under **Management** , click **Models**.
3. Search and select the **text-davinci-003** model.
4. Click **deploy**.

The model will now be available under the **Deployments** section. Select the **text-davinci-003** model and click **Open in Playground**.

![](/media/2023-06-19-az-oai-teams-toolkit-bot/02.png)

In the Completions playground, click View code and note down the Endpoint and Key.

![](/media/2023-06-19-az-oai-teams-toolkit-bot/03.png)

We will store the Endpoint and Key in our solution as a configuration in the bot\src\internal\config.ts file.

```typescript
const config = {
  botId: process.env.BOT_ID,
  botPassword: process.env.BOT_PASSWORD,
  endpoint: 'https://XXXX-openai.openai.azure.com/',
  azureApiKey: 'XXXX'
};

export default config;
```


## Command Handler implementation in Bot

Start by adding below import to `teamsBot.ts`.

```typescript
import { OpenAIClient, AzureKeyCredential } from "@azure/openai";
```

Implement the `welcome` command handler as follows:

```typescript
    ...
    case "welcome": {
          // Call Azure OpenAI to get the assessment questions
          const prompt = [`Generate a poem on Microsoft Teams toolkit`];

          // You will need to set these environment variables or edit the following values
          const endpoint = config.endpoint;

          const azureApiKey = config.azureApiKey;

          const client = new OpenAIClient(endpoint, new AzureKeyCredential(azureApiKey));
          const deploymentId = "text-davinci-003";

          const result = await client.getCompletions(deploymentId, prompt, { maxTokens: 4000 });
          var choiceText = "";
          for (const choice of result.choices) {
            choiceText = choice.text;
          }

          this.openAIOutputObj.openAIOutput = choiceText;

          // Fix missing quotation marks on keys in JSON
          choiceText = choiceText.replace(/(['"])?([a-zA-Z0-9_]+)(['"])?:([^\/])/g, '"$2":$4');
          const card = AdaptiveCards.declare<OpenAIDataInterface>(rawWelcomeCard).render(this.openAIOutputObj);
          await context.sendActivity({ attachments: [CardFactory.adaptiveCard(card)] });
          break;
        }
    ...
```


## Test the bot

From the Visual Studio Code, select **Run and Debug** from the left menu. Choose the run profile and test the bot solution.

![](/media/2023-06-19-az-oai-teams-toolkit-bot/04.png)


## Summary

Adding AI capabilities to your Teams app can be easily done with Azure OpenAI. This can be easily extended to bots with Teams Toolkit to provide engaging experience for the end-users.


## Code Download

The code developed during this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/MSTeams/teams-toolkit/azure-openai-commandbot/AzureOpenAIBot).


## References

- [Teams Toolkit Overview](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/teams-toolkit-fundamentals?WT.mc_id=M365-MVP-5003693)
- [Install Teams Toolkit](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/install-teams-toolkit?WT.mc_id=M365-MVP-5003693)
- [Command bot in Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/bots/how-to/conversations/command-bot-in-teams?WT.mc_id=M365-MVP-5003693)
