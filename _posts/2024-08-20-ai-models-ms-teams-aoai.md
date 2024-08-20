---
title: "Integrating AI Models into Microsoft Teams with Azure OpenAI"
date: "2024-08-20"
share: true
header:
  image: media/2024-08-20-ai-models-ms-teams-aoai/12.png
  teaser: media/2024-08-20-ai-models-ms-teams-aoai/12.png
categories:
  - Microsoft Teams
  - MS Teams
tags:
  - "2024"
  - August 2024
last_modified_at: 2024-08-20T00:00:00-00:00
---
### **Integrating AI Models into Microsoft Teams with Azure OpenAI**

## Introduction

Microsoft Teams, a popular collaboration platform, can be significantly augmented by integrating it with AI models from Azure OpenAI. This integration allows organizations to deploy intelligent bots and applications directly into Teams, transforming the way users interact with the platform and boosting overall efficiency.

In this article, we will explore how we can integrate AI models into Microsoft Teams applications (especially Bots) with Azure OpenAI.

## Understanding the Integration

Azure OpenAI provides powerful AI models capable of natural language understanding and generation. By integrating these models with Microsoft Teams, businesses can create sophisticated bots that offer advanced conversational capabilities. These bots can assist with a wide range of tasks, from automating routine inquiries to providing data-driven insights, all within the Teams environment.

## Prerequisites

Before integrating AI models into Teams, ensure you have the following:

- **Access to Azure OpenAI Service**: Sign up for Azure OpenAI and set up your environment. Obtain your API keys and configure your AI models.
- **Microsoft Teams Developer Account**: Register for a Microsoft Teams developer account and set up the necessary permissions for app development.
- Latest version of Visual Studio Code
- The latest version of Teams Toolkit (VS Code extension)
- Node.js (version 16 or 18)

## Best Practices for AI Integration

- **User-Centric Design**: Ensure that your bot is designed with the end-user in mind. It should provide intuitive and useful interactions, making it easy for users to achieve their goals.
- **Data Security**: Handle all data securely and in compliance with privacy regulations. Ensure that user interactions are protected and that sensitive information is managed appropriately.
- **Continuous Improvement**: Regularly update and improve your bot based on user feedback and performance metrics. Adapt to changing needs and ensure the bot evolves with your organization’s requirements.

## Bring your own data in Azure OpenAI

**Set up Azure OpenAI**

1. Create an Azure OpenAI resource.
2. Set up Azure AI Studio by creating a Hub and a project inside it.


**Deploy GPT model**

Deploy GPT-4 model (or any other GPT model you like).

![](/media/2024-08-20-ai-models-ms-teams-aoai/01.png)


**Bring your own data**

We will implement the RAG (Retrieval Augmented Generation) pattern to bring our own data to enhance the model’s responses by adding external knowledge.

Follow below steps:

1. Under **Project playground**, click **Chat**.
2. Click **Add your data (preview)**.
3. Click **Add a data source**.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/02.png)

4. Select any listed data source to ground the generated results with your data.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/03.png)

5. For this article, I will use the **Upload files (preview)** option.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/04.png)

6. I have uploaded a PDF file with hotel information in Dubai.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/05.png)

## Bringing Custom AI Copilots to Microsoft Teams

Microsoft has introduced a new feature in public preview that allows users to integrate custom AI copilots from Azure OpenAI Studio directly into Microsoft Teams. This enhancement aims to enable organizations to create tailored AI-driven experiences within Teams, leveraging the power of Azure's OpenAI models.

Follow the below steps:

1. In the Azure OpenAI Studio, in the chat set your "system message" to define behavior of the model. From the **Deploy to** menu, select **A new Teams app (preview)**.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/06.png)

2. Specify the Teams app name and click **Download** to download the solution.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/07.png)

3. Get the API Key of Azure AI Search from the **Settings** > **Keys** menu.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/08.png)

4. In the solution, navigate to "src\prompts\chat\config.json" file and replace the key.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/09.png)

5. On the terminal of Visual Studio Code, run `az login`.


**Assign Cognitive Services OpenAI User role**

Follow below steps:

1. Open Azure OpenAI.
2. From the left menu, click **Access control (IAM)**.
3. Click **Add** > **Add role assignment**.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/10.png)

4. Add the account you want to use.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/11.png)


**Test the Teams app**

Follow the below steps to run the Teams app:

1. Navigate back to the VS Code.
2. Make sure that you are logged-in to the M365 and Azure accounts in the Teams Toolkit extension.
3. Click **Run and Debug** from the left menu.
4. Type your prompt to test the app.

    ![](/media/2024-08-20-ai-models-ms-teams-aoai/12.png)


## Key Points

1. **Custom AI Copilots**: Users can now deploy custom AI models, designed and fine-tuned in Azure OpenAI Studio, into Microsoft Teams. This feature allows for the creation of bespoke AI assistants that can address specific organizational needs.
2. **Deployment Process**:
    - **Create and Configure**: Design your custom AI model in Azure OpenAI Studio, focusing on your unique requirements and data.
    - **Integration with Teams**: Use the new integration tools to deploy your custom AI copilot into Teams. This process involves configuring the bot and ensuring it can interact seamlessly within the Teams environment.
3. **Enhanced Capabilities**:
    - **Tailored Experiences**: Custom AI copilots can be designed to handle specific tasks, provide specialized information, or interact in unique ways, enhancing productivity and user experience in Teams.
    - **Advanced Interactions**: Leverage the advanced language understanding and generation capabilities of OpenAI models to create more engaging and effective interactions within Teams.
4. **Public Preview**:
    - The feature is currently available in public preview, allowing organizations to test and provide feedback on its functionality before it becomes generally available.

By integrating these custom AI copilots, organizations can significantly enhance their Teams environment, making it a more powerful and tailored tool for collaboration and productivity.

## Summary

Integrating Azure OpenAI models into Microsoft Teams can revolutionize how organizations interact within the platform, offering enhanced functionality and smarter automation. By following the outlined steps and best practices, businesses can deploy sophisticated AI-driven bots that improve communication, streamline workflows, and drive productivity. As AI technology continues to advance, the potential for these integrations will expand, offering even more opportunities for innovation and efficiency in the workplace.

## References

- [Using your data with Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search%2Cteams#tabpanel_2_teams?WT.mc_id=M365-MVP-5003693)

- [Bring your custom engine copilot from Azure OpenAI Studio to Microsoft Teams](https://devblogs.microsoft.com/microsoft365dev/bring-your-custom-engine-copilot-from-azure-openai-studio-to-microsoft-teams-now-in-public-preview/?WT.mc_id=M365-MVP-5003693)
