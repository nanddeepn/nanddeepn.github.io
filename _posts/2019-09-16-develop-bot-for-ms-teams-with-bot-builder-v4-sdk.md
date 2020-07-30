---
title: "Develop Bots for MS Teams with Bot Builder SDK v4"
date: "2019-09-16"
share: true
categories:
  - MS Teams
  - Bot
header:
  image: media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/15.png
  teaser: media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/15.png
tags:
  - "2019"
  - September 2019
last_modified_at: 2019-09-16T00:00:00-00:00
---

## Overview

MS Teams supports integration with various apps and services. Bot is one of them. Both v3 and v4 bots built on the Bot Framework work well with MS Teams. All new development should be targeting the Bot Builder v4 SDK. Bots can be developed using .NET or Node.js.

In this article, we will develop a bot framework bot and extend it to MS Teams. Please follow my previous article Getting Started with MS Teams Development to setup your development environment.


### Visual Studio Bot Template

The Microsoft Bot Framework v4 Templates for Visual Studio can be downloaded from here ([https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4)). It is available as a VSIX package. It helps to build new conversational AI bots using the Microsoft Bot Framework v4. Both v3 and v4 bots built on the Bot Framework work well with MS teams.

Once installed, the Bot Framework project creation option will be available for new project creation from Visual Studio.


## Develop an Echo Bot

We will create a simple echo Bot in MS Teams. Follow below steps to start developing a Bot.

1. Open **Visual Studio 2017** as an administrator.
2. Click **File** > **New** > **Project**.
3. Under **Visual C#**, select **Bot Framework** > **Echo Bot (Bot Framework v4)**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/01.png)

4. Click **OK**.
5. Build the solution. This will download all referenced NuGet packages.
6. Open **Project properties**.
7. Under **Debug**, note down the **App URL**.

![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/02.png)


## Run the ngrok

Ngrok is a free program that exposes a process running on your localhost to the public internet. Run the ngrok secure tunnel application by executing below steps.

1. On the command prompt, navigate to ngrok.exe location.
2. Execute below command.

```ngrok.exe http [port] -host-header=localhost:[port]```

Replace ```[port]``` with the port mentioned in the App URL of solution.

E.g.:

```ngrok.exe http 3978 -host-header=localhost:3978```

![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/03.png)


## Register Bot in MS Azure

1. Open MS Azure portal ([https://portal.azure.com](https://portal.azure.com))
2. Click **Create a resource**.
3. Search for **Bot** in the marketplace.
4. Select **Bot Channels Registration**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/04.png)

5. Click **Create**.
6. Fill in the form details.
7. For **Messaging endpoint**, specify below value.

    ```https://[ngrok-address].ngrok.io/api/Messages```

    Replace token ```[ngrok-address]``` with forwarding address from ngrok window. (e.g. https://4541d6ad.ngrok.io)

8. Choose **Auto create App ID and password**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/05.png)

9. Click **Create**.
10. Navigate to the resource, when created.
11. Under **Bot Management** section, click **Channels**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/06.png)

12. Click **MS Teams** icon under featured channel.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/07.png)

13. Click **Save**.
14. Agree to the **Terms of Service**.


## Get the Bot Channel Registration Id and secret

1. Under **Bot Management** section, click **Settings**.
2. Note down **Microsoft App ID**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/08.png)

3. Click **Manage** link, next to Microsoft App ID.
4. Under **Client secrets**, click **New client secret**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/09.png)

5. The dialog window will appear. Add description and expiration.
6. Click **Add**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/10.png)

7. Copy the secret string.
8. In the Visual Studio, add MicrosoftAppId and MicrosoftAppPassword under appsettings.json.

![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/11.png)


## Package Bot from Visual Studio

We will add manifest file and related resources that will be compressed as zip file and added to MS Teams.

1. In Visual Studio, add folder **Manifest**.
2. Upload icons for color and outline.
3. Add file manifest.json with below content.

```json
{  
  "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.3/MicrosoftTeams.schema.json",  
  "manifestVersion": "1.3",  
  "version": "1.0.0",  
  "id": "[microsoft-app-id]",  
  "packageName": "io.ngrok.[ngrok-address]",  
  "developer": {  
    "name": "Office Developer",  
    "websiteUrl": "https://[ngrok-address].ngrok.io",  
    "privacyUrl": "https://[ngrok-address].ngrok.io/privacy",  
    "termsOfUseUrl": "https://[ngrok-address].ngrok.io/termsofuse"  
  },  
  "icons": {  
    "color": "bot-icon-color-192x192.png",  
    "outline": "bot-icon-outline-32x32.png"  
  },  
  "name": {  
    "short": "office-content-management-bot ",  
    "full": "Office Content Management Bot"  
  },  
  "description": {  
    "short": "Office Content Management",  
    "full": "Sample bot for the Office Content Management"  
  },  
  "accentColor": "#AB00B0",  
  "bots": [  
    {  
      "botId": "[microsoft-app-id]",  
      "supportsFiles": true,  
      "scopes": [  
        "personal",  
        "team"  
      ]  
    }  
  ],  
  "composeExtensions": [],  
  "permissions": [  
    "identity",  
    "messageTeamMembers"  
  ],  
  "validDomains": []  
}
```

- Replace token ```[microsoft-app-id]``` with MicrosoftAppId.
- Replace token ```[ngrok-address]``` with forwarding address from ngrok window. (e.g. https://4541d6ad.ngrok.io)


## Compress Manifest folder

1. In the Solution explorer, right click on the project and choose to unload.
2. Again, right click on project and choose to edit.
3. Since AfterBuild and AfterPublish targets are defined in the import <Project Sdk="Microsoft.NET.Sdk.Web"> it is expanded to include Sdk.props file before your project's content and Sdk.targets after them, the targets defined in the project will be overwritten by the targets defined by the Sdk / "common targets".
4. Update the file content as below.

    ```xml
    <Project>   
      <Import Project="Sdk.props" Sdk="Microsoft.NET.Sdk.Web" />    
      <PropertyGroup>    
        <TargetFramework>netcoreapp2.1</TargetFramework>    
      </PropertyGroup>    
        
      <ItemGroup>    
        <PackageReference Include="Microsoft.AspNetCore.App" />    
        <PackageReference Include="Microsoft.Bot.Builder.Integration.AspNet.Core" Version="4.4.3" />    
      </ItemGroup>    
        
        <ItemGroup>    
        <Content Update="appsettings.json">    
          <CopyToOutputDirectory>Always</CopyToOutputDirectory>    
        </Content>    
      </ItemGroup>    
    </Project>
    ```

5. Add below target at the end of file.

    ```xml
    <Import Project="Sdk.targets" Sdk="Microsoft.NET.Sdk.Web" />    
    <Target Name="AfterBuild">    
      <ZipDir InputBaseDirectory="manifest"    
              OutputFileName="$(OutputPath)\$(MSBuildProjectName).zip"    
              OverwriteExistingFile="true"    
              IncludeBaseDirectory="false" />    
    </Target>
    ```

6. Add below task element at the end of file.

    ```xml
    <UsingTask TaskName="ZipDir" TaskFactory="CodeTaskFactory"    
            AssemblyFile="$(MSBuildToolsPath)\Microsoft.Build.Tasks.v4.0.dll">    
      <ParameterGroup>    
        <InputBaseDirectory ParameterType="System.String" Required="true" />    
        <OutputFileName ParameterType="System.String" Required="true" />    
        <OverwriteExistingFile ParameterType="System.Boolean" Required="false" />    
        <IncludeBaseDirectory ParameterType="System.Boolean" Required="false" />    
      </ParameterGroup>    
      <Task>    
        <Reference Include="System.IO.Compression" />    
        <Reference Include="System.IO.Compression.FileSystem" />    
        <Using Namespace="System.IO.Compression" />    
        <Code Type="Fragment" Language="cs">    
          <![CDATA[   
        if (File.Exists(OutputFileName))   
        {   
          if (!OverwriteExistingFile)   
          {   
            return false;   
          }   
          File.Delete(OutputFileName);   
        }   
        ZipFile.CreateFromDirectory   
        (   
          InputBaseDirectory, OutputFileName,   
          CompressionLevel.Optimal, IncludeBaseDirectory   
        );   
      ]]>    
        </Code>    
      </Task>    
    </UsingTask>
    ```

7. In the solution explorer, right click and reload the project.
8. Build the solution. The AfterBuild target will run and compressed .zip file will be available under bin folder.
9. Run the Visual Studio solution by pressing F5 key.


## Upload app into Microsoft Teams

1. Open MS Teams.
2. Right click on any existing team, click **Manage team**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/12.png)

3. Click **Apps** tab.
4. Click **Upload a custom app**.

    ![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/13.png)

5. Upload the zip file generated inside bin folder of Visual Studio solution.
6. The bot app will be available to use.

![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/14.png)


## Test the Bot in MS Teams

1. Open **MS Teams**.
2. From left menu, click **Chat**.
3. Our Bot (named OfficeContentManagement) should be listed out.
4. Send the messages to Bot, it should echo back.

![](/media/2019-09-16-develop-bot-for-ms-teams-with-bot-builder-v4-sdk/15.png)

In order to run the bot inside Microsoft Teams:

- The bot must be accessible from the internet.
- The bot must be registered with the bot connector.
- The AppId and AppPassword from the bot framework registration page have to be recorded in the project's web.config.
- The bot must be added to Microsoft Teams.


## Summary

MS Teams supports integration with various apps and services including Bot. Both v3 and v4 bots built on the Bot Framework work well with MS Teams. Visual Studio offers ready templates for Microsoft Bot Framework v4. Ngrok exposes a process running on your localhost to the public internet.

This content was originally posted [here](https://www.c-sharpcorner.com/article/develop-bot-for-ms-teams-with-bot-builder-v4-sdk/).
