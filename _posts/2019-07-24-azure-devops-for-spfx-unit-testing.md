---
title: "SPFx Unit Testing with Azure DevOps"
date: "2019-07-24"
---

## Overview

SharePoint Framework is JavaScript based and supports any JavaScript framework for testing. The test cases can be implemented using Jest and Enzyme. The test cases help to validate the SPFx solutions for each of its releases. Please read through my previous article Unit Test With Jest And Enzyme to develop test cases for SPFx solution. The test cases had to run manually.

In this article, we will automate running the test cases using Azure DevOps.

## Changes to SPFx Solution for Automated Testing using Azure DevOps

Follow below steps to make changes to the SPFx solution.

1. On the command prompt, run below command to open the SPFx solution in a editor of your choice.

code .

1. Run below command to install the jest-junit dependency to get test reports which Azure DevOps can process.

npm i jest-junit --save-dev --save-exact

1. Add below jest configuration after devDependencies section inside package.json file.

"jest": {  
  "reporters": \[  
    \[  
      "jest-junit", {  
        "suiteName": "SPFx Unit Test",  
        "outputDirectory": "./reports/",  
        "outputName": "./junit.xml"  
      }  
    \]  
  \]  
}

1. On the command prompt, run below command to test the configuration locally.

npm test

1. This will create a new folder named “reports” and junit.xml file.

## Azure DevOps Build Pipeline Configuration

Create a repository in Azure DevOps projects and add the code. Once the repository is created, create a new build pipeline by adding below tasks.

### Install Node JS

- On the default agent, click the + sign.
- Search for “Node”.
- Add Node.js tool installer.
- Specify the version as 8.x or 10.x

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-532.png)

### Install npm packages

Restore needed npm packages before starting the build process.

- Add npm task.
- Verify if the command is set to install.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-533.png)

### Run Test Cases

- Add npm task.
- Set command to custom.
- In the “Command and arguments” type “test”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-534.png)

### Publish Test Results

- Add “Publish Test Results” task.
- Select “Test result format” as JUnit.
- In the “Test results files”, specify “\*\*/junit.xml”

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-535.png)

## Execute Test Cases with Azure DevOps

Queue a new build and wait for the output.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-536.png)

Click “Tests” tab to view the test outcome.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-537.png)

The test outcome will display the number of passed and failed tests along with the run duration and pass percentage. Click on each of the test case to view the details of each test case run.

## Summary

The test cases help to validate the SPFx solutions for each of its releases. The test cases can be implemented using Jest and Enzyme. We can automate running the test cases using Azure DevOps. The test outcome from Azure DevOps summarizes the number of passed and failed test cases. Although the test cases fail, build may continue further with the packaging and deployment.

This content was originally posted [here](https://www.c-sharpcorner.com/article/azure-devops-for-spfx-unit-testing2/).
