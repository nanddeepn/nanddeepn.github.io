---
title: "Set up Azure DevOps Pipeline for Power Platform - Continuous Deployment (Part 2)"
date: "2022-06-27"
share: true
header:
  image: media/2022-06-27-azure-devops-pipeline-power-platform-2/01.png
  teaser: media/2022-06-27-azure-devops-pipeline-power-platform-2/01.png
categories:
  - Power Platform
  - DevOps
tags:
  - "2022"
  - June 2022
last_modified_at: 2022-06-27T00:00:00-00:00
---
## Overview

Azure DevOps pipelines make it easy to automate exporting solutions from one environment to importing to other.

In the previous article, we covered how to export the Power Platform solution from one environment, and in this article, we will import it to another environment.

## The Plan

The Power platform solution, we exported from the source environment (Nanddeep Nachan&#39;s Environment) will now to imported to the target environment i.e. nachan365 (default).

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/01.png)

## Azure AD App Registration

We will use the same Azure AD app registration from the previous article, instead of creating a new one.

## Create system user in Dataverse pointing to Azure AD App registration

1. Open [Power Platform Admin center](https://admin.powerplatform.microsoft.com/).
2. Navigate to the source environment.
3. Click **Settings**.
4. Under **Users + permissions**, click **Users**.
5. Click the **app users list**.
6. Click **+ New app user**.
7. Click **+ Add an app**.
8. Select the previously created Azure AD app from the list.
9. Click **Add**.
10. Select the **Business unit**.
11. Under **Security roles** , select **System Administrator**.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/02.png)

12. Click **Create**.

## Create Azure DevOps Pipeline - Service Connection

1. In the [Azure DevOps](https://dev.azure.com/), navigate to your project.
2. From the bottom left, click **Project settings**.
3. Under **Pipelines**, click **Service connections**.
4. Click **Create service connection**.
5. Search and select **Power Platform**.
6. Click **Next**.
7. Provide the service connection details from previous configurations.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/03.png)

8. Click **Save**.

Now, we should have 2 service connections each pointing to the source and target environment.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/04.png)

## Create Release Pipeline

We will not automate the release process by setting up a new pipeline.

1. In the Azure DevOps project, click **Pipelines** > **Releases**.
2. Click **New pipeline**.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/05.png)

3. We will start with the **Empty job**.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/06.png)

4. Name the stage (e.g., by the target environment)

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/07.png)

5. Select the **Source type** as **Build**.
6. Click **Add**.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/08.png)

7. Click **1 job, 0 task**.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/09.png)

## Add Tasks to the pipeline

Add below tasks to your pipeline.

Task #1: **Power Platform Tool Installer**

This will install the Power Platform Build Tools.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/10.png)

Task #2: **Power Platform Import Solution**. 

This will import a solution into a target environment.

1. Specify Service connection
2. For the **Solution Input File** (which is expected to be zip), specify the path from the drop folder, where the build pipeline produces the artifacts.

    ![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/11.png)

Task #3: **Power Platform Publish Customizations**. 

This will publish all customizations in an environment.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/12.png)

## Execute the release pipeline

Click **Create Release** from the top to start the release pipeline.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/13.png)

Wait for the release pipeline to finish.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/14.png)

## The End Result

Once the release pipeline finishes successfully, the Power Platform solution will be deployed to the target environment.

![](/media/2022-06-27-azure-devops-pipeline-power-platform-2/15.png)

## Summary

Azure DevOps pipelines make it easy to automate to move your solutions from one environment to other. Power Platform Build Tools tasks help to automate common build and deployment tasks in Azure DevOps pipelines.

## References

- [CI/CD for Power Platform](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/azure-devops-continuous-integration-for-power-platform?WT.mc_id=M365-MVP-5003693)
- [Microsoft Power Platform Build Tools for Azure DevOps](https://docs.microsoft.com/en-us/power-platform/alm/devops-build-tools?WT.mc_id=M365-MVP-5003693)
- [Microsoft Power Platform Build Tools tasks](https://docs.microsoft.com/en-us/power-platform/alm/devops-build-tool-tasks?WT.mc_id=M365-MVP-5003693)
