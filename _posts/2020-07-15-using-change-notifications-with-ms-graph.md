---
title: "Using Change Notifications with Microsoft Graph"
date: "2020-07-15"
share: true
header:
  image: media/2020-07-15-using-change-notifications-with-ms-graph/01.png
  teaser: media/2020-07-15-using-change-notifications-with-ms-graph/01.png
categories:
  - MS Graph
tags:
  - "2020"
  - July 2020
last_modified_at: 2020-07-15T00:00:00-00:00
---

## Overview

MS Graph is a single end point for all M365 services, i.e. [https://graph.microsoft.com](https://graph.microsoft.com/). With MS Graph, we can use a single access token for authentication to multiple services.

In this article, we will explore how to work with change notifications (webhooks) &amp; track changes (delta query) in the Microsoft Graph.


## Change notifications

Change notifications feature enable applications to be alerted when data is created or changed in Microsoft Graph. When a particular entity is created, updated, or changed, Microsoft Graph submits an HTTP POST to the specified custom endpoint. The custom endpoint listens to these messages and acts on it.

![](/media/2020-07-15-using-change-notifications-with-ms-graph/01.png)

You can get notifications on messages, events, contacts, users, groups, conversations, OneDrive files, and alerts.


## Implement the scenario

We have to follow below steps to implement this scenario:

- Create the app
- Subscribe for notifications
- Receive notifications

From your application, you can create a subscription with MS Graph. The subscription has information about the entities you want to get notified and address of custom web API. For this to work, we will need accessible HTTPS secured endpoint to receive the notifications. For the developer experience, we can use Ngrok. MS Graph will be able to submit HTTP POST requests to the Ngrok URL

## Azure AD app registration

Create an Azure AD app registration by following below steps:

1. Navigate to the Azure Active Directory admin center ([https://aad.portal.azure.com/](https://aad.portal.azure.com/))
2. From left navigation, select **Azure Active Directory**.
3. Under **Manage** , select **App registrations**.
4. Click **New registration**.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/02.png)

5. For new application, set the values as below:

    - Name: GraphNotificationDemo
    - Supported account types: Accounts in any organizational directory and personal Microsoft accounts
    - Redirect URI: Web > http://localhost

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/03.png)

6. Click **Register**.
7. Note down the **Application (client) ID** and **Directory (tenant) ID** for later use.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/04.png)


## Add a client secret

1. Select **Manage** > **Certificates &amp; secrets**.
2. Select **New client secret**.
3. Enter a **Description** , select **Expires** duration.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/05.png)

4. Click **Add**.
5. Note down the client secret value for future reference.


## API Permissions

1. Select **Manage** > **API Permissions**.
2. Select **Add a permission** and select **Microsoft Graph**.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/06.png)

3. Select **Application Permission**.
4. Select the permission **User.Read.All**.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/07.png)

5. Click **Add permissions**.
6. Select **Grant admin consent for [tenant name]**, then select **Yes** to consent this application.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/08.png)


## Ngrok

Ngrok allows calls from the internet to be directed to your application running locally without needing to create firewall rules.

Install Ngrok from [https://ngrok.com/](https://ngrok.com/)

On the command prompt, Run below command:

```
ngrok http 5000
```

![](/media/2020-07-15-using-change-notifications-with-ms-graph/09.png)

Note down the forwarding address for future reference.


## Create an ASP.NET Core web API project

1. On the command prompt, type below command to create a new .NET Core WebApi app.

    ```
    dotnet new webapi -o changenotifications
    ```

2. Add below packages to the project.

    ```
    cd changenotifications
    dotnet add package Microsoft.Identity.Client
    dotnet add package Microsoft.Graph
    ```

3. Run the project to test everything works.

    ```
    dotnet build && dotnet run
    ```

4. Open the application in Visual Studio Code.

    ```
    code .
    ```


## Create change notification subscriptions

We can create a change notification subscription by submitting an HTTP POST request to the subscriptions endpoint: https://graph.microsoft.com/v1.0/subscriptions.

Below is an example that creates a subscription to receive change notifications on the https://graph.microsoft.com/v1.0/users endpoint when users are updated:

```
POST https://graph.microsoft.com/v1.0/subscriptions HTTP/1.1
Authorization: bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFDRWZl<snip>
Content-Type: application/json; charset=utf-8
Host: graph.microsoft.com
Content-Length: 199

{
  "changeType": "updated",
  "clientState": "SecretClientState",
  "notificationUrl": "https://1a3f84c2.ngrok.io/api/notifications",
  "resource": "/users",
  "expirationDateTime": "2020-03-11T04:30:28.2257768+00:00"
}
```

- notificationUrl: Where notifications should be sent
- changeType: The type of change to trigger the notification
- expirationDateTime: Timestamp for the expiration of the subscription.
- clientState: Refers to your registered endpoint


## Update the ASP.NET Core web API project

In the Startup.cs file and comment out the following line to disable SSL redirection.

```csharp
//app.UseHttpsRedirection();
```


**Add model classes**

The application uses several new model classes for (de)serialization of messages to/from the Microsoft Graph.

Create new folder Models, add below files to it:

- Notification.cs
- ResourceData.cs
- MyConfig.cs

Open the Startup.cs file. Locate the method ConfigureServices() method and replace it with the following code:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_3_0);
  var config = new MyConfig();
  Configuration.Bind("MyConfig", config);
  services.AddSingleton(config);
}
```

Open the appsettings.json file and replace the content with the following JSON.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "MyConfig":
  {
    "AppId": "<APP ID>",
    "AppSecret": "<APP SECRET>",
    "TenantId": "<TENANT ID>",
    "Ngrok": "<NGROK URL>"
  }
}
```

Replace the variables with the values noted earlier.


## Add notification controller

The application requires a new controller to process the subscription and notification.

In the Controllers folder, add new controller NotificationsController.cs

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using changenotifications.Models;
using Newtonsoft.Json;
using System.Net;
using System.Threading;
using Microsoft.Graph;
using Microsoft.Identity.Client;
using System.Net.Http.Headers;

namespace changenotifications.Controllers
{
  [Route("api/[controller]")]
  [ApiController]
  public class NotificationsController : ControllerBase
  {
    private readonly MyConfig config;

    // Renew notifications
    private static Dictionary<string, Subscription> Subscriptions = new Dictionary<string, Subscription>();
    private static Timer subscriptionTimer = null;

    public NotificationsController(MyConfig config)
    {
      this.config = config;
    }

    [HttpGet]
    public async Task<ActionResult<string>> Get()
    {
        var graphServiceClient = GetGraphClient();

        var sub = new Microsoft.Graph.Subscription();
        sub.ChangeType = "updated";
        sub.NotificationUrl = config.Ngrok + "/api/notifications";
        sub.Resource = "/users";
        sub.ExpirationDateTime = DateTime.UtcNow.AddMinutes(5);
        sub.ClientState = "SecretClientState";

        var newSubscription = await graphServiceClient
            .Subscriptions
            .Request()
            .AddAsync(sub);

        // Renew notifications (every 15 seconds)
        Subscriptions[newSubscription.Id] = newSubscription;

        if (subscriptionTimer == null)
        {
            subscriptionTimer = new Timer(CheckSubscriptions, null, 5000, 15000);
        }

        return $"Subscribed. Id: {newSubscription.Id}, Expiration: {newSubscription.ExpirationDateTime}";
    }

    public async Task<ActionResult<string>> Post([FromQuery]string validationToken = null)
    {
      // handle validation
      if(!string.IsNullOrEmpty(validationToken))
      {
        Console.WriteLine($"Received Token: '{validationToken}'");
        return Ok(validationToken);
      }

      // handle notifications
      using (StreamReader reader = new StreamReader(Request.Body))
      {
        string content = await reader.ReadToEndAsync();

        Console.WriteLine(content);

        var notifications = JsonConvert.DeserializeObject<Notifications>(content);

        foreach(var notification in notifications.Items)
        {
          Console.WriteLine($"Received notification: '{notification.Resource}', {notification.ResourceData?.Id}");
        }
      }

      return Ok();
    }

    private GraphServiceClient GetGraphClient()
    {
      var graphClient = new GraphServiceClient(new DelegateAuthenticationProvider((requestMessage) => {
        // get an access token for Graph
        var accessToken = GetAccessToken().Result;

        requestMessage
            .Headers
            .Authorization = new AuthenticationHeaderValue("bearer", accessToken);

        return Task.FromResult(0);
      }));

      return graphClient;
    }

    private async Task<string> GetAccessToken()
    {
      IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(config.AppId)
        .WithClientSecret(config.AppSecret)
        .WithAuthority($"https://login.microsoftonline.com/{config.TenantId}")
        .WithRedirectUri("https://daemon")
        .Build();

      string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

      var result = await app.AcquireTokenForClient(scopes).ExecuteAsync();

      return result.AccessToken;
    }

    // Renew notifications
    private void CheckSubscriptions(Object stateInfo)
    {
        AutoResetEvent autoEvent = (AutoResetEvent)stateInfo;

        Console.WriteLine($"Checking subscriptions {DateTime.Now.ToString("h:mm:ss.fff")}");
        Console.WriteLine($"Current subscription count {Subscriptions.Count()}");

        foreach(var subscription in Subscriptions)
        {
            // if the subscription expires in the next 2 min, renew it
            if(subscription.Value.ExpirationDateTime < DateTime.UtcNow.AddMinutes(2))
            {
            RenewSubscription(subscription.Value);
            }
        }
    }

    private async void RenewSubscription(Subscription subscription)
    {
        Console.WriteLine($"Current subscription: {subscription.Id}, Expiration: {subscription.ExpirationDateTime}");

        var graphServiceClient = GetGraphClient();

        var newSubscription = new Subscription
        {
            ExpirationDateTime = DateTime.UtcNow.AddMinutes(5)
        };

        await graphServiceClient
            .Subscriptions[subscription.Id]
            .Request()
            .UpdateAsync(newSubscription);

        subscription.ExpirationDateTime = newSubscription.ExpirationDateTime;
        Console.WriteLine($"Renewed subscription: {subscription.Id}, New Expiration: {subscription.ExpirationDateTime}");
    }

  }
}
```


## Test the application

1. On the command prompt, run below command:

    ```
    dotnet run
    ```

2. In the browser, navigate to the URL http://localhost:5000/api/notifications to subscribe to change notifications.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/10.png)

    The application is now subscribed to receive notifications from the Microsoft Graph, when an update is made on any user in the Office 365 tenant.

3. In the browser navigate to the Microsoft 365 admin center ([https://admin.microsoft.com/AdminPortal](https://admin.microsoft.com/AdminPortal)).
4. Select **Users** > **Active users**.
5. Edit contact information of any active user.

    ![](/media/2020-07-15-using-change-notifications-with-ms-graph/11.png)


## Summary

It also allows you to avoid implementing a polling infrastructure where you submit requests to Microsoft Graph at regular intervals to get the most recent changes.


## Code Download
The code developed during this article can be found [here](https://github.com/nanddeepn/code-samples/tree/master/MSGraph/changenotifications).
