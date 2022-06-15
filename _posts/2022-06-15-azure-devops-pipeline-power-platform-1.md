---
title: "Set up Azure DevOps Pipeline for Power Platform - Part 1"
date: "2022-06-15"
share: true
header:
  image: media/2022-06-15-azure-devops-pipeline-power-platform-1/28.png
  teaser: media/2022-06-15-azure-devops-pipeline-power-platform-1/28.png
categories:
  - Power Platform
  - DevOps
tags:
  - "2022"
  - June 2022
last_modified_at: 2022-06-15T00:00:00-00:00
---
## Overview

Power Platform solutions can be easily exported from one environment and imported to other with manual efforts. Azure DevOps pipelines make it easy to automate to move your solutions from one environment to other.

With the two-part series, we will set up Azure DevOps pipelines for this scenario. In this article, we will cover how to export the Power Platform solution from one environment, and in the next article, we will import it to another environment.

## Prerequisites

We will need the below things to get started:
1. Power Apps portal
2. Azure Portal
3. Azure DevOps
4. Power Platform Build Tools

## Azure AD App Registration

As a first step, we need to configure service connections using a service principal. This can be done by creating an application registration in Azure Active Directory (AAD).

**Register an application**

1. Open [Azure Portal](https://portal.azure.com).
2. Click **Azure Active Directory**.
3. From the left menu, click **App registrations**.
4. Click **+ New registration**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/01.png)

5. Create a new app registration as follows:

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/02.png)

6. Note down the Application ID and tenant ID.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/03.png)

**Add API permissions**

1. From the left navigation, click **API permissions**.
2. Click **+ Add a permission**.
3. Click **APIs my organization uses**
4. Search and select **Dataverse**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/04.png)

5. Select the **delegated permissions** and **Add permissions**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/05.png)

**Generate client secret**

6. From left menu, click **Certificates &amp; secrets**
7. Click **+ New client secret**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/06.png)

8. Note down the secret value.

## Create system user in Dataverse pointing to Azure AD App registration

1. Open [Power Platform Admin center](https://admin.powerplatform.microsoft.com/).
2. Navigate to your environment.
3. Click **Settings**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/07.png)

4. Under **Users + permissions** , click **Users**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/08.png)

5. Click the **app users list**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/09.png)

6. Click **+ New app user**.
7. Click **+ Add an app**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/10.png)

8. Select the previously created Azure AD app from the list.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/11.png)

9. Click **Add**.
10. Select the **Business unit**.
11. Under **Security roles** , select **System Administrator**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/12.png)

12. Click **Create**.

## Install Power Platform Build Tools extension

To perform Power Platform operations from Azure DevOps, you first need to install the [Power Platform Build Tools extension](https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.PowerPlatform-BuildTools) from the Visual Studio Marketplace. This extension will add Power Platform service connection and Azure Pipeline tasks.

1. Browse to the extension.
2. Click **Get it free**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/13.png)

3. Select your Azure DevOps organization.
4. Click **Install**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/14.png)

## Create Azure DevOps Pipeline

1. In the [Azure DevOps](https://dev.azure.com/) navigate to your project.
2. From the bottom left, click **Project settings**.
3. Under **Pipelines** , click **Service connections**.
4. Click **Create service connection**.
5. Search and select **Power Platform**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/15.png)

6. Click **Next**.
7. Provide the service connection details from previous configurations.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/16.png)

8. Click **Save**.

## Create Pipeline

Follow below steps to create a pipeline:
1. In the Azure DevOps project, click **Pipelines**.
2. Click **Create Pipeline**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/17.png)

3. Select **Azure Repos Git**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/18.png)

4. Select your repository.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/19.png)

5. Select **Starter pipeline**.
    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/20.png)

### Add tasks to the pipeline

Add below tasks to your pipeline.

Task #1: **Power Platform Tool Installer**

This will install the Power Platform Build Tools.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/21.png)

Task #2: **Power Platform Who Am I** 

This will send a request to the Dynamics CRM instance and get information of the authenticated user.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/22.png)

Task #3: **Power Platform Export Solution**

This will help to export an existing solution.

- Select Service connection.
- Specify the solution name to export.
- Specify path and zip file name as solution output file.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/23.png)

Task #4: **Power Platform Unpack Solution**

This task takes a compressed solution file and decomposes it into multiple XML files so that these files can be more easily read and managed by a source control system.

- Specify **the Solution Input File** as the file exported in the previous step.
- In the **Target Folder to Unpack Solution** , specify the path to unpack the solution.
- Select the type of solution.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/24.png)

Task #5: **Publish build artifacts** 

This will make the artifacts available at the specified path.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/25.png)

Task #6: **Command line** 

This will help to check-in the solution to the git repository.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/26.png)

```yaml
cd $(Build.SourcesDirectory)

# Set a per-project email address and username
git config user.email "nanddeep.nachan@contoso.com"
git config user.name "Nanddeep Nachan"

# Navigate to the main branch
git checkout main

# Update the local version of a repository from a remote
git pull

# Add all files to the Git repository
git add --all

# Record the changes in the repository
git commit -m "Updated the solution"

# Authenticate against a git repository in a build process
git -c http.extraheader="AUTHORIZATION: bearer $(System.AccessToken)" push --set-upstream origin $exportBranchName
```

The full YAML pipeline can be downloaded from [here](/media/2022-06-15-azure-devops-pipeline-power-platform-1/Power Apps DevOps.yml).

## Locate the Build Artifacts

To see the artifacts published as an output of pipeline execution follow the below steps:

1. Click on the finished build.
2. In the **Summary** , under the **Related** section, click **published**.

    ![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/27.png)

The published files will be available under the drop folder.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/28.png)

## Repository

The repository will have the solution checked-in.

![](/media/2022-06-15-azure-devops-pipeline-power-platform-1/29.png)

In the next article, we will import the exported solution to another environment.

## Summary

Azure DevOps pipelines make it easy to automate to move your solutions from one environment to other. Power Platform Build Tools tasks help to automate common build and deployment tasks in Azure DevOps pipelines.

## References

- [CI/CD for Power Platform](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/azure-devops-continuous-integration-for-power-platform?WT.mc_id=M365-MVP-5003693)
- [Microsoft Power Platform Build Tools for Azure DevOps](https://docs.microsoft.com/en-us/power-platform/alm/devops-build-tools?WT.mc_id=M365-MVP-5003693)
- [Microsoft Power Platform Build Tools tasks](https://docs.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks?WT.mc_id=M365-MVP-5003693)
