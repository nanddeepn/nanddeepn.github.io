---
title: "Function calling in Azure OpenAI"
date: "2024-06-16"
share: true
header:
  image: media/2024-06-16-aoai-function-calling/04.png
  teaser: media/2024-06-16-aoai-function-calling/04.png
categories:
  - OpenAI
tags:
  - "2024"
  - June 2024
last_modified_at: 2024-06-16T00:00:00-00:00
---
## Overview

The GPT models in Azure OpenAI are powerful at generating content. The last training date of LLM models sometimes poses a challenge because it limits the model's ability to provide information on events or developments that occurred after that date, potentially leading to outdated or incomplete responses. Function calling helps in these scenarios by enabling the language model to access real-time information and perform up-to-date operations that go beyond its static training data.

In this article, we will have an overview of function calling, technical implementation, and example scenarios.

## Overview of Function Calling

Function calling in OpenAI's language models bridges the gap between static training data and dynamic, real-time information, significantly expanding the model's utility and responsiveness.

Function calling in OpenAI's language models allows the models to interface with external tools and APIs, enhancing their ability to provide accurate and contextually relevant responses by performing specific actions based on user inputs.

**Purpose:**

- Enhance the capabilities of language models by enabling them to call predefined functions.
- Improve the accuracy and relevance of responses by leveraging external data sources and tools.

**How It Works?**

- **Function Definition:** Developers define functions that the model can call. These functions are typically used to perform tasks such as retrieving data, performing calculations, or interfacing with other APIs.
- **Model Invocation:** When interacting with the language model, users can provide inputs that trigger these functions. The model identifies when a function call is appropriate based on the input and context.
- **Function Execution:** The model passes the necessary parameters to the function, which then executes and returns the results.
- **Response Integration:** The results from the function are integrated into the model's response to the user, providing enriched and more accurate information.

**Benefits:**

- **Accuracy:** By using external functions, the model can provide precise information that may not be within its training data.
- **Efficiency:** Functions can perform complex calculations or data retrievals quickly, enhancing the model's response time.
- **Versatility:** Allows the model to perform a wider range of tasks, from simple queries to complex operations.

**Applications:**

- **Data Retrieval:** Fetching current data such as weather information, stock prices, news updates, etc.
- **Calculations:** Performing mathematical computations, data analysis, or generating reports.
- **Automation:** Interfacing with automation tools for tasks like scheduling, reminders, or controlling smart home devices.
- **Integration with Services:** Connecting with third-party APIs for services like translation, payment processing, or e-commerce operations.

**Implementation Example:**

Let's consider an example where a language model needs to provide current weather information:

- **Function Definition:** A function called get_weather is defined to retrieve weather data from a weather API.
- **User Input:** A user asks, "What's the weather like in New York City today?"
- Model Processing: The model recognizes the need to call the get_weather function with the parameter "New York City".
- **Function Call:** The get_weather function is called, retrieves the current weather data, and returns it.
- **Response:** The model integrates this data into its response and tells the user, "The weather in New York City today is 75°F with partly cloudy skies."

## Using function calling with Azure OpenAI Service

The latest versions of GPT-35-Turbo (e.g., gpt-35-turbo-16k) and GPT-4 are now better at using functions. They can figure out when and how to use a function based on what you ask. If you include one or more functions in your request, the model will decide if any should be used based on the context. If it decides a function is needed, it will respond with a JSON object containing the arguments for the function.

This means the models can naturally create API calls and structure data outputs using the functions you specify. However, while the models can generate these calls, you are responsible for executing them, so you stay in control.

Here's a simple breakdown of how to work with functions:

- Call the chat completions API with your functions and the user's input.
- Use the model’s response to call your API or function.
- Call the chat completions API again, including the response from your function, to get a final answer.

## Function calling in action

Let us implement an example of a function calling as “What is the stock price of &lt;Company&gt;?”

Here is how ChatGPT responds:

![](/media/2024-06-16-aoai-function-calling/01.png)

Let us start by creating a console application in Visual Studio 2022.

![](/media/2024-06-16-aoai-function-calling/02.png)

**Add NuGet package**

Add the reference of Azure.AI.OpenAI package to generate content from OpenAI models by following below steps:

1. In the Visual Studio 2022, click **Tools** > **NuGet Package Manager** > **Package Manager Console**.
2. Run the below command:

    ```
    dotnet add package Azure.AI.OpenAI --version 1.0.0-beta.7
    ```

**Implement function definition**

Add a file `GetStockPriceFunction.cs` to implement the stock price function definition.

```cs
using Azure.AI.OpenAI;
using System.Text.Json;

namespace az_openai_function_calling
{
    /// <summary>
    /// Function to get stock price
    /// </summary>
    public class GetStockPriceFunction
    {
        static public string Name = "get_stock_price";

        /// <summary>
        /// Function definition
        /// </summary>
        static public FunctionDefinition GetFunctionDefinition()
        {
            return new FunctionDefinition()
            {
                Name = Name,
                Description = "Get the stock price of company",
                Parameters = BinaryData.FromObjectAsJson(
                new
                {
                    Type = "object",
                    Properties = new
                    {
                        Company = new
                        {
                            Type = "string",
                            Description = "The company, e.g. Contoso",
                        }
                    },
                    Required = new[] { "company" },
                },
                new JsonSerializerOptions() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase }),
            };
        }

        /// <summary>
        /// Function implementation
        /// </summary>
        /// <param name="location"></param>
        /// <param name="unit"></param>
        /// <returns></returns>
        static public StockPrice GetStockPrice(string company)
        {
            Random r = new Random();
            int range = 100;
            double randomStockPrice = r.NextDouble() * range;

            return new StockPrice() { Price = randomStockPrice, Currency = "USD" };
        }
    }

    /// <summary>
    /// Function argument
    /// </summary>
    public class StockPriceInput
    {
        public string Company { get; set; } = string.Empty;
    }

    /// <summary>
    /// Return type
    /// </summary>
    public class StockPrice
    {
        public double Price { get; set; }
        public string Currency { get; set; } = "USD";
    }
}
```

**Make LLM function aware**

Now, we will make LLM aware of the function definition. Make the below changes in the `program.cs` file.

```cs
// Create Azure OpenAI client
OpenAIClient client = new(openAIUri, new AzureKeyCredential(openAIApiKey));
ChatCompletionsOptions chatCompletionsOptions = new ChatCompletionsOptions();

// Read user input
Console.WriteLine("System >  I can help you find stock price of a given company");
Console.Write("User > ");
string question = Console.ReadLine()?.Trim();
chatCompletionsOptions.Messages.Add(new(ChatRole.User, question));

// Make LLM function aware
FunctionDefinition getStockPriceFunctionDefinition = GetStockPriceFunction.GetFunctionDefinition();
chatCompletionsOptions.Functions.Add(getStockPriceFunctionDefinition);

// Call the Completion in a loop to determine if the finish reason is "function" or "stop."
ChatCompletions response = await client.GetChatCompletionsAsync(model, chatCompletionsOptions);
ChatChoice responseChoice = response.Choices[0];

// Loop until the finish reason is not "function."
while (responseChoice.FinishReason == CompletionsFinishReason.FunctionCall)
{
    // Add message to history.
    chatCompletionsOptions.Messages.Add(responseChoice.Message);

    if (responseChoice.Message.FunctionCall.Name == GetStockPriceFunction.Name)
    {
        string unvalidatedArguments = responseChoice.Message.FunctionCall.Arguments;
        StockPriceInput input = JsonSerializer.Deserialize<StockPriceInput>(unvalidatedArguments, new JsonSerializerOptions() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase })!;

        var functionResultData = GetStockPriceFunction.GetStockPrice(input.Company);
        var functionResponseMessage = new ChatMessage(
            ChatRole.Function,
            JsonSerializer.Serialize(
                functionResultData,
                new JsonSerializerOptions() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase }
        ));

        functionResponseMessage.Name = GetStockPriceFunction.Name;
        chatCompletionsOptions.Messages.Add(functionResponseMessage);
    }

    // Call LLM again to generate the response.
    response = await client.GetChatCompletionsAsync(model, chatCompletionsOptions);
    responseChoice = response.Choices[0];
}

Console.ForegroundColor = ConsoleColor.Green;
Console.WriteLine(responseChoice.Message.Content);
```

## The final output

Run the application by pressing F5, and ask the question: What is the stock price of Contoso?

Azure OpenAI should use the function calling to answer the question using GetStockPriceFunction as follows:

![](/media/2024-06-16-aoai-function-calling/03.png)

## Summary

Function calling in OpenAI's language models allows the models to interface with external tools and APIs, enhancing their ability to provide accurate and contextually relevant responses by performing specific actions based on user inputs.


## References

- [Function calling is now available in Azure OpenAI Service](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/function-calling-is-now-available-in-azure-openai-service/ba-p/3879241?WT.mc_id=M365-MVP-5003693)
- [Azure OpenAI client library for .NET](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?WT.mc_id=M365-MVP-5003693)
- [C#: Azure Open AI and Function Calling](https://dev.to/kenakamu/c-azure-open-ai-and-function-calling-h7j) by [Kenichiro Nakamura](https://dev.to/kenakamu)


## Code Download

The code developed during this article can be found at: [https://github.com/nanddeepn/code-samples/tree/master/AI/az-openai-function-calling-cs](https://github.com/nanddeepn/code-samples/tree/master/AI/az-openai-function-calling-cs)