---
parent: Module 3 - Salesforce Integration
title: "Lab 3: Run the Mule app from Anypoint Studio"
nav_order: 3
assets: "../../assets/images/module-3-sf/lab3/"
---
1. TOC
{:toc}

# Lab 3: Run the Mule app from Anypoint Studio
{: .no_toc }

## Overview
In this step we are going to test the implementation that we just finished in the previous lab. To test the API, we are going to use the Studio.

## Step 1: Run the API

1. Let’s explore the application that was just created. (we will skip the HTTP configuration since it was covered in the previous lab. The flows that were created represent the different operations available on the Customer API).

2. To test the API, let’s run it within Studio. Right click on the application in the **Package Explorer** view.

3. Select **Run As​ > Mule Application​**. (Feel free to skip this if you’ve done this in the previous lab)

    ![]({{ page.assets }}module3_lab3_run_as.png)

4. Every time we start the application, we need to make sure that it is started successfully. Check in the Console tab that the project appears as "**DEPLOYED**".

    ![]({{ page.assets }}module3_lab3_deployed.png)

5. Once the application is deployed, Anypoint Studio will open a tab with the APIkit Consoles where we can test the application (to access the console, just click the link to open the **Customer API** Console in a browser)

    ![]({{ page.assets }}module3_lab3_api_console.png)

    ![]({{ page.assets }}module3_lab1_api_console_browser.png)

6. Click the **POST** tab. Look at the third panel.

7. Complete the **Accept** field with the value `application/json`.

## Step 2: Create the User

1. Go to the API Console.

2. Go to the **Body** tab

3. Instead of using the default json example, **change the user name and email**.

4. Click the **SEND** button

    ![]({{ page.assets}}module3_lab3_edit_post.png)

    Once we get the successful  <img src="{{ page.assets }}module3_lab3_POST_successful.png" width="75"> response from the API, we can go to Salesforce and check if the customer exists.

    {: .warning}
    It could happen that you are receiving a 400 response. That means that there is a user that already exists. That is not a bad news, it means that the error is being mapped when you validate the response.

## Step 3: Check if Customer exists

To validate that the API is working, we will login to Salesforce​.

1. Open a browser and go to link [http://salesforce.com](http://salesforce.com)

2. Click on Login​ using the following credentials:

    - Username​: `workshop_read@salesforce.com`

    - Password​: `Muley1313!`

    {: .note}
    The user for the login is different from the one used to config the app. This corresponds to Salesforce Security.

3. In Salesforce, search for the existence of the email you would like to register or you already registered​.

    ![]({{ page.assets }}module3_lab3_sfdc_search.png)

    Also you can click on the apps icon, clic **View All** and select **Accounts** and then search in the list. If the users exists, it will be listed in the **Accounts section.

    ![]({{ page.assets }}module3_lab3_sfdc_search2.png)

    ![]({{ page.assets }}module3_lab3_sfdc_found.png)

4. You can see the details by clicking on the **Account Name**

    ![]({{ page.assets }}module3_lab3_sfdc_details.png)

5. Try adding another customer to further validate.

This concludes the lab.

You can download a complete project from exchange.

## Summary
In this lab, we

- Run the Mule app from Anypoint Studio

- Create a user in the Salesforce

Go Further:

- See the link [Studio Visual Debugger](https://docs.mulesoft.com/studio/latest/visual-debugger-concept) for more information

**Congratulations! You have completed Lab 3.**

Please proceed to [Lab 4](./module-3-sf-lab-4){: .btn .btn-blue  .mr-2  }