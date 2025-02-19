---
parent: Module 2 - API Implementation
title: "Lab 2: Download the complete Omni Channel project"
nav_order: 2
assets: "../../assets/images/module2/lab2/"
---
1. TOC
{:toc}

# Lab 2: Download the complete Omni Channel project
{: .no_toc }

## Overview

Now that we’ve seen how APIKit helps developers import the designed REST API to enable MuleSoft flow development, we’ll download a fully implemented Omni Channel API project.

## Step 1: Import the project from Anypoint Exchange

1. From Studio open exchange
    ![]({{ page.assets }}module2_lab2_open_exchange.png)

2. Filter by Examples

3. Select the sub organization

4. Click on the omni-channel-api
    ![]({{ page.assets }}module2_lab2_omni_channel.png)

5. In the portal press **Open**
    ![]({{ page.assets }}module2_lab2_open_project.png)

    Once you open it, the project is going to be imported into Studio. (Perform any updates required)

    ![]({{ page.assets }}module2_lab2_menu_import.png)

## Step 2: Examine the different flows within the project

The previous lab already introduced the **APIKit**, **Error handling** and **Dataweave Transform Message** processors. In below flows we introduce **REST Connectors**, **Flow Control** components and **Scopes**. So let’s first look at these in more detail.

![]({{ page.assets }}module2_lab2_step3_4a_rest_connector.png)

**REST Connector**

A REST Connector is a component that can be used in Mule applications that’s been custom created via a specific framework. This framework allows you to create components entirely by using other existing Mule components. When an API Specification is published to exchange a REST Connector is automatically generated for it in Exchange by REST Connect Tool.

![]({{ page.assets }}module2_lab2_step3_4b_flow_control.png)

**Flow Control**

Flow Control Routers such as the **Choice Router** allows for controling the message flow within a Mule flow.

![]({{ page.assets }}module2_lab2_step3_4c_scopes.png)

**Scopes**

Sometimes referred to as "wrappers", the message processors known as **Scopes** appear as processing blocks when you first place them on the Message Flow canvas.

Now, lets look at all the flows within the project.

![]({{ page.assets }}module2_lab2_step3_5_get_order_by_id.png)

This flow calls the Order API to get an order from the database based on the order id

![]({{ page.assets }}module2_lab2_step3_6_get_order_search.png)

**get:\orders\search**

This flow calls the Order API to get orders from the database based on a search

![]({{ page.assets }}module2_lab2_step3_7_get_product_by_id.png)

**get:\products\product\\\{product_id\}**

This flow calls the Product API to get a product based on the product id
![]({{ page.assets }}module2_lab2_step3_8_get_product_search.png)

**get:\products\search**

This flow returns products from the database by calling the Product API

![]({{ page.assets }}module2_lab2_step3_9_get_user_shopping_cart.png)


**get:\users\user\\\{user_id\}\shopping_cart**

This flow returns all the items in a users shopping cart based on the user id

![]({{ page.assets }}module2_lab2_step3_10_put_user_shopping_cart.png)

**put:\users\user\\\{user_id\}\shopping_cart**

This flow puts an item into a users shopping cart based on the user id
![]({{ page.assets }}module2_lab2_step3_11_post_shopping_cart.png)

**post:\users\user\\\{user_id\}\shopping_cart\confirmation**

This flow confirms the items to purchase in a users shopping cart.

## Step 3: Update Shopping Cart confirmation Flow (Optional)

In order to be able to confirm your order once it is placed, you might need to do some minor updates to the confirmation flow. You will be albe to test is in the next lab once you have the application deployed.

1. Search for "Set variable" in the mule palette.
    ![]({{ page.assets }}module2_lab2_set_variable.png)

2. Add the Set variable at the beginning of the confirmation flow
    ![]({{ page.assets }}module2_lab2_add_variable.png)

3. Click on the Set variable and configure it with the following:
    a. Name: `user_id`
    b. Value: `attributes.uriParams.user_id` (remember to click the function button)

4. Finally set a defaul value of 1 in the **Create User Response** transform
    ![]({{ page.assets }}module2_lab2_set_default.png)

Once you have done this step you will be able to complete the order placed in the next lab.

Save all your changes and proceed to the next step.

## Step 4: Test the application

To test the API, let’s run it within Studio first.

1. Right click the application.
2. Select **Run As > Mule Application**.
    ![]({{ page.assets }}module2_lab2_step4_12_studio_run.png)

3. The application will start running, and the console will show the Mule Runtime logs
    ![]({{ page.assets }}module2_lab2_step4_13_studio_console_deployed.png)

    {: .note}
    Anypoint Studio deployed this application to an embedded Mule Runtime. There is no need to deploy to a separate Mule server environment. The developer will be able to develop and test the application locally until it’s ready to be deployed to a shared development or QA environment.

4. Test the application using the console. Click the **Open Console** on the APIkit Console tab.
    ![]({{ page.assets }}module2_lab2_step4_14_apikit_console_base_url.png)

5. A browser window opens at: [http://localhost:8081/console/](http://localhost:8081/console/).

6. Click open the **/products/search** resource on the left and click **GET**.
    ![]({{ page.assets }}module2_lab2_step4_15_browser_apikit_console_tryit.png)

7. Then tick the **Show optional parameters** and fill them in as below and click **SEND**.
    ![]({{ page.assets }}module2_lab2_step4_16_apikit_console_get_request.png)

8. Unlike Lab 1, this project is a complete solution and is making calls to System and Process REST APIs. The response you see in the Console is coming from other sources.
    ![]({{ page.assets }}module2_lab2_step4_17_apikit_console_response.png)

    Let’s go ahead and stop the application to get ready for the next lab.

9. Go to the console tab and press the red squared button to stop the Mule runtime server.
    ![]({{ page.assets }}module2_lab2_step4_18_studio_stop.png)

## Summary

In this lab, we went trough all these steps:

- [Step 1: Import the project from Anypoint Exchange](#step-1-import-the-project-from-anypoint-exchange)
- [Step 2: Examine the different flows within the project](#step-2-examine-the-different-flows-within-the-project)
- [Step 4: Test the application](#step-4-test-the-application)

This lab shows how quick and easy it is to enable your teams to get up and running quickly with pre-built project templates.

- See the [REST Connectors](https://docs.mulesoft.com/exchange/to-deploy-using-rest-connect) documentation for more information.
- See the [Flow Control Routers](https://docs.mulesoft.com/mule-runtime/latest/about-components#flow-control-routers) documentation for more information.
- See the [Scopes](https://docs.mulesoft.com/mule-runtime/latest/about-components#scopes) documentation for more information.

**Congratulations! You have completed Lab 2.**

You can now proceed to [Lab 3](./module-2-lab-3){: .btn .btn-blue  .mr-2  }