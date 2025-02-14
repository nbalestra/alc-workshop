---
parent: Module 3 - Salesforce Integration
title: "Lab 1: Import the RAML Definition into Studio"
nav_order: 1
assets: "../../assets/images/module-3-sf/lab1/"
---
1. TOC
{:toc}

# Lab 1: Import the RAML Definition into Studio
{: .no_toc }

## Overview
In this lab, we will create the skeleton implementation of our Customer API that will process requests to Salesforce. You will use Anypoint Studio to create a Mule application that will have one flow for each method of each resource (i.e. GET customers). Additionally you will use APIKit (as part of your Mule application) to process REST requests, transform them to messages to be handled and processed by each flow.

![]({{ page.assets }}module3_lab1_intro.png)

The implementation will consist of a few steps:

## Copy link to clipboardStep 1: Create a new Mule Project and import RAML Definition

We are going to use our API definition to create an implementation of our Customer API. To do this we need to import the RAML Specification from the **Design Center**

1. If it’s not already opened, start Anypoint Studio from the desktop icon and select the same workspace you’ve been working (example: C:\workspaces\myworkspace).
2. From Anypoint Studio’s menu, select File > New > Mule Project to create a New Mule project. A Window will pop-up to define the details of this new application.
3. Give the project the name customer-api
4. Select the **Mule Server 4.4.0 EE** (or latest version available).
5. Under **Import a Publish API** we are going to import the RAML Spec from Exchange. Click on <img src="{{ page.assets }}module3_lab1_plus_button.png" width="30px"> and select from **Exchange**

    ![]({{ page.assets }}module3_lab1_new_mule_api_project.png)

6. Once you clicked there, you will see a new window. If you don’t see any username. You need to login to the Anypoint Platform. So click on the Add Account button.

    {: .note}
    Ignore any popup about secure storage or any other that may be displayed during loggin

7. This is the same thing you did in the last module. You will see a list with all the APIs Select **Customer API** published by your workshop business gropup (you might find it easier if you type "Customer API" with quotes) and press **Add**.

    ![]({{ page.assets }}module3_lab1_vcs_choose_project.png)

    {: .warning }
    Be sure when you select the **Customer API**. Validate that the **Publisher** has the name of the workshop business group.


8. Press **Finish**.

9. Check everything is fine. Be sure that the **Scaffold flows from these API Specifications** is checked. Press **Finish**.

    ![]({{ page.assets }}module3_lab1_check_create.png)

10. APIkit creates your new Anypoint Studio project with a generated implementation based on your API specification. The generated configuration implements the inbound HTTP listener, as well as, all the resources, request/response data type metadata, among other thing

    ![]({{ page.assets }}module3_lab1_new_api_full.png)

## Step 2: Run the API

1. To test the API, let’s run it within Studio. Right click on the application in the **Package Explorer** view.

2. Select **Run As​ > Mule Application​**. (Feel free to skip this if you’ve done this in the previous lab).

    ![]({{ page.assets }}module3_lab1_run_mule_app.png)

3. Every time we start the application, we need to make sure that it is started successfully. Check in the Console tab that the project appears as "**DEPLOYED**".

    ![]({{ page.assets }}module3_lab1_deployed.png)

4. Once the application is deployed, Anypoint Studio will open a tab with the APIkit Consoles where we can test the application (to access the console, just click the link to open the **Customer API** Console in a browser).

    ![]({{ page.assets }}module3_lab1_api_console.png)

    ![]({{ page.assets }}module3_lab1_api_console_browser.png)

5. Click the **POST** method under `/customer` and test the customer creation.

6. In the Accept field complete with `application/json`

7. Press the **Send** Button.

    ![]({{ page.assets }}module3_lab1_api_console_send.png)

8. Scroll down the third panel and you will see the sample response defined in the API specification.

    ![]({{ page.assets }}module3_lab1_api_console_201.png)

9. Go to the console tab and press the red button to stop the Mule runtime server.

    ![]({{ page.assets }}module3_lab1_studio_stop_mule.png)

## Summary
In this lab, we

- Import the RAML Definition directly into Anypoint Studio
- Auto-generated a skeleton project to implement the API
- Run the skeleton project and tested it via the API Console

Go Further:
- See the link [APIkit](https://docs.mulesoft.com/apikit/latest/) doc for more information.

*Congratulations! You have completed Lab 1.*

Please proceed to [Lab 2](./module-3-sf-lab-2){: .btn .btn-blue  .mr-2  }