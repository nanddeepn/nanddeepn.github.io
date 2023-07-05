---
title: "Reading Azure Key Vault Secret from Azure Function using Managed Identity"
date: "2023-06-25"
share: true
header:
  image: media/2023-06-25-read-key-vault-secret-from-function-mi/17.png
  teaser: media/2023-06-25-read-key-vault-secret-from-function-mi/17.png
categories:
  - Azure
  - Managed Identity
tags:
  - "2023"
  - June 2023
last_modified_at: 2023-06-25T00:00:00-00:00
---
## Overview

Managing secrets, keys, certificates, and credentials is always challenging for developers. Managed identities provide an effective way to overcome managing these for them.

In this article, we are going to explore one such scenario of reading Azure Key Vault Secret from Azure Function using Managed Identity.


## Azure Function

Let us start with creating an Azure function.

From the Azure portal, create a Function app as follows:

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/01.png)

In the Function app, let us create an HTTP trigger-based function with an Authorization level as a Function to read the configuration values from the Application settings.


## Store the Application Settings

Follow the below steps to store the application settings, which we will read from the Azure function later:

1. In the Azure Function app, under **Settings** click **Configuration**.
2. Under **Application settings**, click **+ New application setting**.
3. Create a new application setting as follows:

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/02.png)

4. Click **Save**.


## Read application settings from the Azure Function

Write below code the Azure function to read the application setting:

```powershell
using namespace System.Net

# Input bindings are passed in via param block.

param($Request, $TriggerMetadata)

$clientId = $env:ClientId
$body = "Application setting: $clientId"

# Associate values to output bindings by calling 'Push-OutputBinding'.
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = [HttpStatusCode]::OK
    Body = $body
})
```

Run the function to verify that it correctly reads the Application setting.

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/03.png)


## Azure Key Vault

Instead of maintaining the configurations at an individual resources (e.g., Azure Function) level, it is always a good idea to maintain them at a single location, i.e., Azure Key Vault.

From the Azure portal, create a key vault as follows:

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/04.png)

Once the key vault is ready, create a secret by navigating to the **Secrets** option under **Objects**.

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/05.png)

Click **+ Generate/Import** to create a new secret.

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/06.png)


**Troubleshoot**

If you see the message "You are unauthorized to view these contents" while generating a new secret, follow the below steps:

1. Under **Settings**, click **Access configuration**.
2. Click **Go to access control (IAM)**.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/07.png)

3. Click **Add** > **Add role assignment**.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/08.png)

4. Select the appropriate role (e.g., Key Vault Administrator). Add your user as a member and assign the role.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/09.png)

Now, we will maintain the secrets from the key vault using the specified user.


## Enable Managed Identity on the Azure Function App

Follow the below steps to enable the managed identity on the Azure Function App:

1. Navigate to the Azure Function App.
2. Under **Settings**, click **Identity**.
3. Turn **On** the System assigned managed identity.
4. Click **Save**.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/10.png)

5. Once enabled, click **Azure role assignments**.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/11.png)

6. Add role assignment as follows.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/12.png)

7. Click **Save**.

Now, our Azure function app has a contributor role on the Key vault via managed identity.


## Get Secret Identifier from Key Vault

We need to get the Secret Identifier of the secret we have stored in the key vault by the below steps:

1. Navigate to the Key Vault.
2. Under **Objects**, click **Secrets**.
3. Select our secret created in the previous step (e.g., ClientId)
4. Select the latest version.
5. Note down the Secret Identifier.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/13.png)


## Use Managed Identity to access Key Vault Secret

Follow the below steps to use managed identity to access Key Vault Secret:

1. Navigate to the Azure Function app.
2. Under **Settings**, click **Configuration**.
3. Edit the ClientId configuration as follows:

```
@Microsoft.KeyVault(SecretUri=<Secret Identifier>)
```

Replace the `<Secret Identifier>` with the Secret identifier from the previous step.

```
@Microsoft.KeyVault(SecretUri=https://az-nachan-kv.vault.azure.net/secrets/ClientId/0ea2f333cb4840f0bc93223965a6145a)
```

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/14.png)

You will see an error for Key Vault Reference.

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/15.png)


## Resolve the error

Follow the below steps to give the Azure function app's managed identity permission on Azure Key Vault:

1. Navigate back to Azure Key Vault.
2. Click **Access control (IAM)**.
3. Click **Add** > **Add role assignment**.
4. Select a role (E.g., Key Vault Administrator).
5. Under **Assign access to**, select a **Managed identity**.
6. Select the Function app's managed identity.

    ![](/media/2023-06-25-read-key-vault-secret-from-function-mi/16.png)


## Access Key Vault Secret from Azure Function

Navigate back to the Azure function app and restart it. Now you should see a green tick for Key Vault Reference.

![](/media/2023-06-25-read-key-vault-secret-from-function-mi/17.png)

Run the function to validate the secret is being read from the key vault.


## Summary

Managed identities provide an effective way to overcome managing secrets for the developers. Azure Key Vault Secret can be read from Azure Function using Managed Identity.


## References

- [Managed identities for Azure resources](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview?WT.mc_id=M365-MVP-5003693)
