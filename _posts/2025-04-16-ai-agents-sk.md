---
title: "Building AI Agents with Semantic Kernel"
date: "2025-04-16"
share: true
header:
  image: media/2025-04-16-ai-agents-sk/01.png
  teaser: media/2025-04-16-ai-agents-sk/01.png
categories:
  - AI
  - Copilot
tags:
  - "2025"
  - April 2025
last_modified_at: 2025-04-16T00:00:00-00:00
---
## Introduction

The rise of Generative AI and Large Language Models (LLMs) has made it easier than ever to build intelligent, context-aware applications. Microsoft’s **Semantic Kernel** is a powerful open-source SDK that helps developers build AI agents by combining natural language capabilities with conventional programming logic. This article explores how you can build your own AI agents using Semantic Kernel in a structured and practical way.

## What is Semantic Kernel?

**Semantic Kernel (SK)** is an orchestration framework that allows developers to mix AI with traditional code. It supports multiple languages like C#, Python, and Java, and helps in building **AI-first applications** where LLMs act as reasoning engines, and your code handles execution.

To help developers build their own Copilot experiences on top of AI plugins, Microsoft have released Semantic Kernel, a lightweight open-source SDK that allows you to orchestrate plugins (i.e., your existing code) with AI. With Semantic Kernel, you can leverage the same AI orchestration patterns that power Microsoft's Copilots in your own apps.  

Today's AI models can easily generate messages and images for users. While this is helpful when building a simple chat app, it is not enough to build fully automated AI agents that can automate business processes and empower users to achieve more. To do so, you would need a framework that can take the responses from these models and use them to call existing code to actually do something productive.


## Key Capabilities:
- Connect to OpenAI, Azure OpenAI, Hugging Face, or local LLMs.
- Run AI prompts (called **semantic functions**).
- Combine semantic functions with **native code**.
- Create workflows and **autonomous agents** that can plan and act.

## Semantic Kernel makes AI development extensible

Semantic Kernel (SK) is an open-source SDK developed by Microsoft to help developers build AI-powered agents that can use language models, memory, logic, and tools/plugins to accomplish real-world tasks.

Think of it as the orchestrator that brings together:
- Language models (e.g., GPT-4, Azure OpenAI)
- External tools and services (via plugins)
- Memory (for context and history)
- Your existing business logic (code or APIs)

![](/media/2025-04-16-ai-agents-sk/01.png)

1. Models and Memory (the "brain")
This is where language models (LLMs like OpenAI GPT, Azure OpenAI, Hugging Face, etc.) and memory stores (like vector databases or local storage) live.
These enable:
- Understanding and generation of human-like language.
- Long-term memory of past interactions or context.

2. Connectors
Connectors are the interface layer between Semantic Kernel and external AI services (models) or memory storage. They abstract away the specific implementation (e.g., OpenAI vs Azure OpenAI) so you can swap AI providers without changing your entire codebase. This is what gives you the freedom to switch providers, like from OpenAI to Hugging Face.

3. Semantic Kernel Core
This is the heart of the architecture — it orchestrates how AI models, memory, and plugins work together.
It decides:
- Which model to call.
- What plugins or tools to trigger.
- How memory is read or updated.
- It’s what lets you compose your AI agent behavior using simple logic.

4. Plugins
These are the tools your AI agent uses to interact with your apps or services. Examples: A plugin to read from SharePoint, create calendar events, send emails, or make database calls.
In the analogy: They are the "arms and hands" of your AI app — enabling real-world actions.

5. Triggers and Actions
This is where external events can trigger AI workflows, and where AI can take actions in response.
Think of:
- A Power Automate flow kicking off a Semantic Kernel function.
- A webhook triggering a custom SK agent.
- This makes SK reactive and able to execute full workflows.

Analogy: AI Agent = Brain + Memory + Hands + Orchestrator

Layer |	Acts as... | Enables...
------|------------|----------
Models & Memory	| Brain	| Thinking and remembering
Plugins | Hands | Acting on the world
Connectors | Nerves | Connecting brain to memory & senses
Semantic Kernel | Orchestrator | Coordinates everything
Triggers/Actions | Events | External signals to start/stop actions



## Build Your Own Agent in .NET

In the command prompt, type

```
dotnet new console -o HelloSK 
```

After running the above, navigate into that directory 

```
cd HelloSK 
```

Install the Semantic Kernel SDK 

```
dotnet add package Microsoft.SemanticKernel
```

To use Configuration in our application

```
dotnet add package Microsoft.Extensions.Configuration --version 8.0.0 
```

Use the below code in `Program.cs`

```cs
using Microsoft.Extensions.Configuration;
using Microsoft.SemanticKernel;

var config = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .Build();

var builder = Kernel.CreateBuilder();

// Create plugin to call Azure OpenAI
builder.Services.AddAzureOpenAIChatCompletion(
    config["DeploymentModel"],
    config["AzOpenAIEndpoint"],
    config["AzOpenAIKey"]
    );

var kernel = builder.Build();

var result = await kernel.InvokePromptAsync("Tell me a joke on cat");
Console.WriteLine(result);
Console.Read();
```

The `appsettings.json` will be as follows:

```json
{
  "DeploymentModel": "gpt-4o",
  "AzOpenAIEndpoint": "https://<resource>.openai.azure.com/",
  "AzOpenAIKey": "XXXXXXXXXX"
}
```

Type `dotnet run` on the command prompt to run the application.

## Maintaining Chat History

```cs
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;

var config = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .Build();

var builder = Kernel.CreateBuilder();

builder.Services.AddAzureOpenAIChatCompletion(
    config["DeploymentModel"],
    config["AzOpenAIEndpoint"],
    config["AzOpenAIKey"]
);

var kernel = builder.Build();
var chatService = kernel.GetRequiredService<IChatCompletionService>();

var chatHistory = new ChatHistory();

// Set the behavior of the bot
chatHistory.AddSystemMessage(@"
You are a friendly hotel food ordering assistant.
Help guests order food from today's menu.
Menu:
- Butter Chicken
- Paneer Tikka
- Veg Biryani
- Dal Tadka
- Naan
- Jeera Rice
- Sprite

Greet the user, show the menu on request, and confirm orders when placed.
");

Console.WriteLine("Welcome to Hotel FoodBot! (type 'exit' to quit)");

while (true)
{
    Console.Write("\nYou: ");
    var userInput = Console.ReadLine();

    if (string.Equals(userInput, "exit", StringComparison.OrdinalIgnoreCase))
        break;

    chatHistory.AddUserMessage(userInput);

    var reply = await chatService.GetChatMessageContentAsync(chatHistory);
    chatHistory.AddAssistantMessage(reply.Content);

    Console.WriteLine($"\nBot: {reply.Content}");
}
```

## Plugin

Create a utility plugin `UtilityPlugin.cs` as follows:

```cs
public class UtilityPlugin
{
    [KernelFunction, Description("Get the current time")]
    public string GetTime() => DateTime.Now.ToString("hh:mm tt");

    [KernelFunction, Description("Say hello to someone")]
    public string SayHello(string name) => $"Hello, {name}!";
}
```

The plugin can be consumed as follows:

```cs
using HelloSemanticKernel;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.SemanticKernel;

var config = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .Build();

var builder = Kernel.CreateBuilder();

builder.Services.AddAzureOpenAIChatCompletion(
    config["DeploymentModel"],
    config["AzOpenAIEndpoint"],
    config["AzOpenAIKey"]
);

var kernel = builder.Build();

// Register plugin using KernelPluginFactory
var plugin = KernelPluginFactory.CreateFromObject(new UtilityPlugin(), "utils");
kernel.Plugins.Add(plugin);

// Call the plugin function
var timeResult = await kernel.InvokeAsync("utils", "GetTime");
Console.WriteLine($"Time: {timeResult.GetValue<string>()}");

var helloResult = await kernel.InvokeAsync("utils", "SayHello", new() { ["name"] = "Nanddeep" });
Console.WriteLine(helloResult.GetValue<string>());
```

## Real-World Use Cases

- Customer Support Agent: Combines FAQs (semantic), ticket updates (native), and memory for previous interactions.
- Personal Assistant: Reads emails, sets up meetings, and summarizes content using Microsoft Graph + Semantic Kernel.
- Sales Copilot: Generates proposals, looks up pricing, and summarizes client meetings using skills and memory.

## Conclusion

Semantic Kernel empowers developers to build intelligent agents that can plan, reason, and act. With seamless integration of AI prompts, skills, and memory, you can build powerful copilots and automation tools tailored to your needs. Whether you're enhancing business productivity or building a customer-facing bot, Semantic Kernel provides the flexibility and power to turn your ideas into reality.
