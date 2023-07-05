---
title: "Reading Azure Key Vault Secret from Azure Automation Runbook using Managed Identity"
date: "2023-07-05"
share: true
header:
  image: media/2023-07-05-read-key-vault-secret-from-az-automation-mi/08.png
  teaser: media/2023-07-05-read-key-vault-secret-from-az-automation-mi/08.png
categories:
  - Azure
  - Managed Identity
tags:
  - "2023"
  - July 2023
last_modified_at: 2023-07-05T00:00:00-00:00
---
## Overview

Managing secrets, keys, certificates, and credentials is always challenging for developers. Managed identities provide an effective way to overcome managing these for them.

In this article, we are going to explore one such scenario of reading Azure Key Vault Secret from the Azure Automation runbook using Managed Identity.


## Azure Key Vault

Let us start by creating an Azure Key Vault.

Instead of maintaining the configurations at an individual resources (e.g., Azure Function) level, it is always a good idea to maintain them at a single location, i.e., Azure Key Vault.

From the Azure portal, create a key vault as follows:

![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/01.png)

Once the key vault is ready, create a secret by navigating to the **Secrets** option under **Objects**.

![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/02.png)

Click **+ Generate/Import** to create a new secret.

![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/03.png)


**Troubleshoot**

If you see the message "You are unauthorized to view these contents." While generating a new secret, follow the below steps:

1. Under **Settings** , click **Access configuration**.
2. Click **Go to access control (IAM)**.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/04.png)

3. Click **Add** \> **Add role assignment**.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/05.png)

4. Select the appropriate role (e.g., Key Vault Administrator). Add your user as a member and assign the role.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/06.png)

Now, we will maintain the secrets from the key vault using the specified user.


## Access Key Vault Secret from Azure Automation Runbook

Head to the Azure portal to create an Azure Automation Runbook.

![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/07.png)


## Enable Managed Identity on the Azure Automation Account

Follow the below steps to enable the managed identity on the Azure Automation account:

1. Under **Account Settings** , click **Identity**.
2. Turn **On** the System assigned managed identity (if not already).
3. Click **Save**.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/08.png)

4. Once enabled, click **Azure role assignments**.
5. Add role assignment as follows.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/09.png)

6. Click **Save**.

Now, our Azure Automation account has a reader role on the Key vault via managed identity.


## Use Managed Identity to access Key Vault Secret

Follow the below steps to use managed identity to access Key Vault Secret:

1. Navigate to the Azure Automation account.
2. Under **Process Automation** click **Runbooks** to create a runbook.

    ![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/10.png)

Write the below code:

```powershell
try {
    # Read from Azure Key Vault using managed identity
    $connection = Connect-AzAccount -Identity
    $clientIdSecret = Get-AzKeyVaultSecret -VaultName "az-nachan-key-vault" -Name "ClientId" -AsPlainText
    Write-Output $clientIdSecret
}
catch {
    $errorMessage = $_
    Write-Output $errorMessage

    $ErrorActionPreference = "Stop"
}
```

Run the automation runbook to validate the secret is being read from the key vault.

![](/media/2023-07-05-read-key-vault-secret-from-az-automation-mi/11.png)


## Summary

Managed identities provide an effective way to overcome managing secrets for the developers. Azure Key Vault Secret can be read from the Azure Automation runbook using Managed Identity.


## References

- [Managed identities for Azure resources](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview?WT.mc_id=M365-MVP-5003693)