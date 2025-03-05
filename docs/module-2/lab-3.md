---
parent: Module 2 - API Implementation
title: "Lab 3: Deploy the Omni Channel API implementation"
nav_order: 3
assets: "../../assets/images/module2/lab3/"
---
1. TOC
{:toc}


# Lab 3: Deploy the Omni Channel API implementation
{: .no_toc }

## Overview

In this lab we’ll deploy the complete Omni Channel API from Lab 2 to the [Cloudhub iPaas Platform](https://docs.mulesoft.com/runtime-manager/)L from Anypoint Studio itself the same way a developer would.

{: .note}
CloudHub is the world’s first integration Platform as a Service (iPaaS). CloudHub is built on top of Mule, the most widely used integration platform. CloudHub lets you manage applications in the cloud and manage servers located in your organization.

## Step 0: Update dependencies
Before we deploy our application, we need to make sure that the application is using the latest components from exchange.

1. Right click on your application and select **Manage Dependencies** --> **Manage Modules**

    ![]({{ page.assets }}module-2-lab3-manage-dependencies-menu.png)

2. For each module, select the module and click on Update Version:

  ![]({{ page.assets }}module-2-lab3-update-module.png)

3. Once all modules have been updated to the latest, click on **Apply and Close**

## Step 1: Deploy the Complete Omni Channel API to Cloudhub
Let’s begin:

1. Open your `pom.xml` file, select the Source tab and update your `<artifactId>` (line 6) adding your initials at the beggining of the name. It should look like this: '`egl-omni-channel-api`'
    ![]({{ page.assets }}module2_lab3_update_pom1.png)
    ![]({{ page.assets }}module2_lab3_update_pom.png)

2. Now let’s deploy it to Cloudhub. **Right click on the project name → Anypoint Platform → Deploy to CloudHub**.
    ![]({{ page.assets }}module2_lab3_step1_1_deploy_to_cloudhub.png)

3. If this is your first time deploying in this way, a popup menu asks you to provide your login credentials for Anypoint Platform. Studio stores your credentials and uses them automatically the next time you deploy to CloudHub.
    {: .note}
    You can manage these credentials through the Studio Preferences menu, in Anypoint Studio > Authentication.

    Once logged in to the Anypoint Platform one needs to select an **Environment** and **Business Group**. Choose the one **Business Group** you have access to and select as environment **Sandbox**.

    ![]({{ page.assets }}module2_lab3_step1_2_choose_environmentand_businessgroup.png)

4. Now we need to specify application configuration for our API.

    a. On the application name put **MyName-omni-channel-api**. You should see a green check if the name is available. If not, please use another name

    {: .highlight }
    Save this name as you will use it later on. An example would be **donald-omni-channel-api**

    b. On **Deployment Target** leave "Cloudhub 2.0" with the corresponding region (regions might change depending on your location)

    c. On **Runtime version** select **4.9.1 (or latest version available)**

    d. On **Worker Size** select **Micro - 0.1 vCore** (not to be confused with Micro (Memory Intensive) which will provision a slightly bigger worker)

    {: .highlight}
    Leave the rest of the default values.

    ![]({{ page.assets }}module2_lab3_step1_3_deploy_runtime_manager2.0.png)

5. Once the configuration is done, click on the **Deploy Application** button.

6. Once the deployment is triggered in CloudHub (it may take a few minutes. Bigger workers take a shorte time to start up), click on Open in Browser button in the pop up that will open.

  ![]({{ page.assets }}module-2-lab3-deployment-popup.png)

   You will be taken to Runtime Manager, where you will be able to manage your application(s). From Runtime Manager copy your application's Public Endpoint:

   ![]({{ page.assets }}module-2-lab3-pulbic-endpoint.png)

   Now in a browser window paste the Public Endpoint URL and apped `/console/` to it.

   In the our example we have [https://nb-omni-channel-api-1s3mvo.5sc6y6-2.usa-e2.cloudhub.io/console/](https://nb-omni-channel-api-1s3mvo.5sc6y6-2.usa-e2.cloudhub.io/console/).

   {: .note}
   The Region may change depending on which Platform you are working US or EU.


   ![]({{ page.assets }}module2_lab3_step1_4_api_console.png)

   Now you have a fully functional **Omni Channel API** of your own.

## Step 2: Test the API from a Mobile Application

Your API application is running on CloudHub, lets try to hit it using a mobile application. We have developed a mobile application to test our APIs. This mobile application will invoke your REST API using the CloudHub URL.

You can access the mobile application through:

[Mobile application](http://mythical-mobileapp.workshops.mulesoft.com/) 

1. Click on **create a new account** and create an account to access the Mythical Mobile Commerce App

    ![]({{ page.assets }}module2_lab3_mobile_signup.png)

2. Login using your email address. The mobile app is not enforcing any authentication yet, so the password can be anything.

    ![]({{ page.assets }}module2_lab3_mobile_login.png)

3. To connect this to your Mobile Experience API, open the hamburger menu and go to Settings then add your Cloudhub URL for your Mobile Experience API we just deployed (do not include `/console`).

    ![]({{ page.assets }}module2_lab3_mobile_settings2.png)

    The mobile app will communicate with your Experience API to bring your design to life with API Led Connectivity!

    ![]({{ page.assets }}module2_lab3_mobile_list.png)

## Summary
In this lab, we
- Deployed our Omni Channel API application to Anypoint Platform’s CloudHub directly from Studio. CloudHub provides you a way to manage your applications in the cloud or servers located on-premises.
- Test the application through the Mobile App.

Go Further:

- See the [CloudHub](https://docs.mulesoft.com/cloudhub/) doc for more information.
- See the [Deploy to CloudHub](https://docs.mulesoft.com/cloudhub/deploying-to-cloudhub) doc for more information.

**Congratulations! You have completed Lab 3.**

You can now proceed to the next optional [lab 4](./lab-4)