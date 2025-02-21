---
parent: Module 2 - API Implementation
title: "Lab 1: Implement the Omni Channel API in Studio"
nav_order: 1
assets: "../../assets/images/module2/lab1/"
---
1. TOC
{:toc}

# Lab 1: Implement the Omni Channel API in Studio
{: .no_toc }

## Overview

In this first lab, you will use Anypoint Studio to create a Mule application where there will be one flow for each method of each resource (i.e. GET). Additionally you will use APIKit (as part of your Mule application) to process each REST requests, transform them to messages to be handled and processed by each flow.

The implementation will consist of a few steps

1. Create a new Mule Project in Anypoint Studio from the Omni Channel API RAML
2. Run the Mule app from Anypoint Studio

Then we will test this new API using the API Console.

## Step 1: Create a new Mule Project from the RAML
In this step we will create a new Mule application in Anypoint Studio from the Omni Channel API RAML Definition. This will be the implementation of our REST API.

1. Start Anypoint Studio from the desktop icon.
    <p>
    <img src="{{ page.assets }}module2_lab1_step1_1_studio_logo.png" width="50">
    </p>

2. When you open the Studio for the first time, you are going to be asked for the workspace where the projects are going to be saved. Select the one that comes by default.

    ![]({{ page.assets }}module2_lab1_choose_workspace.png)

3. A popup requiring indows defender exception will be displayed. Click no, as you don’t have admin access to windows

    ![]({{ page.assets }}module2_lab1_win_defender.png)

4. You are presented with the Anypoint Studio Welcome Screen.

    ![]({{ page.assets }}module2_lab1_step1_2_welcome_screen.png)

5. Scroll to the bottom if you want to know more on Studio. After that, press **Continue to Studio**.

6. In the Workspace Launcher, use the default workspace directory location.

7. From Anypoint Studio’s menu, select File > New > Mule Project to create a New Mule project. A Window will pop-up to define the details of this new application.

    ![]({{ page.assets }}module2_lab1_file_new.png)

8. Give the project the name **omni-channel-api-v1**

9. Select the **Mule Server 4.4.0 EE** (or latest version if present).

10. Under **Import a Published API** we are going to import the RAML Spec from Exchange. Click on <img src="{{ page.assets }}module2_lab1_plus_button.png" width="25px"> and select **from Exchange**

    ![]({{ page.assets }}module2_lab1_new_mule_api_project.png)

11. Once you clicked there, you will see a new window. You need to login to the Anypoint Platform. So click on the **Add Account** button.

    ![]({{ page.assets }}module2_lab1_add_account.png)

12. A new Login window will appear. Insert your credentials.

    ![]({{ page.assets }}module2_lab1_platform_login.png)

13. In the Search field put `Omni Channel`

14. Select the **\<username\> Omni Channel Experience API** that you created in Module 1 and click **Add**.

    {: .warning}
    If you haven’t done Module 1, you can import the **Omni Channel Experience API**. Be sure you aren’t choosing the one whose **Publisher** is **Mulesoft**

    ![]({{ page.assets }}module2_lab1_step1_6_import_RAML.png)

15. Press Finish

16. Check everything is fine. Be sure that the **Scaffold flows from these API Specifications** is checked.

    ![]({{ page.assets }}module2_lab1_check_scaffold.png)

17. Now click **Finish**

    ![]({{ page.assets }}module2_lab1_step1_7_studio_overview.png)

18. You might be prompted to perform updates, click **Perform update** and wait until the update finishes

    ![]({{ page.assets }}module2_lab1_perform_updates.png)

19. After the update is done, a skeleton project that implements your API will be displayed.

    **Done! now let’s explore what was automatically generated.**

20. At the top of the diagram you will see a flow called **api-main** and **api-console** followed by flows, one for each method defined in the RAML.

    The flows below are defined by your API Design in the RAML file. Typically, there will be flows that look like this `get:\resource`, `post:\resource`, `put:\resource`, etc. Note that the name of the flow is very important for the APIkit router to be able to route the request to the appropriate flow - you don’t want to change these.

    When APIkit detects example data in the response of a method in the RAML it inserts a **Dataweave Transform Message** into the flow which returns the static response specified by the example data reference.

    The static response returned by the auto-generated **Dataweave Transform Message** allows you to test the API as a stub immediately after generation. Obviously these flows can be enhanced to provide more advanced mock services as well as evolve them into full API implementation as we will see in the next lab.




**api-main**

<img src="{{ page.assets }}module2_lab1_step1_8_apikit_main.png" width="150px">

This is the main flow. It exposes an HTTP service and processes the requests using the [APIKit Router](https://docs.mulesoft.com/apikit/).

The HTTP request will be converted to a mule message, and redirected to the requested flow by the APIKit router.

By selecting the http listener one can take a look at the HTTP configuration, there you will see the Protocol, Host and Port attributes, all that information means that the listener is in fact listening for requests on [http://localhost:8081/api](http://localhost:8081/api).

Inside the api-main flow also an [Error-Handling](https://mule4-docs.mulesoft.com/mule-runtime/4.0/error-handling.html) block is included. In this block multiple **On Error Propagate** definitions are autogenerated for common API error responses such as `METHOD_NOT_ALLOWED`, `BAD_REQUEST` etc.

---

**put:\users\user\\\{user_id\}\shopping_cart**


<img src="{{ page.assets }}module2_lab1_step1_9_apikit_put_user_shopping_cart.png" width="230px">


This flow puts an item into a users shopping cart based on the user id. This put operation returns no response and therefore the **Dataweave Transform Message** processor is replaced automatically by a **Logger** message processor.

---

**get:\orders\order\\\{order_id\}**

<img src="{{ page.assets }}module2_lab1_step1_10_apikit_get_order_by_id.png" width="230px">


This flow gets an order based on an order id

---
**get:\products\product\\\{product_id\}**

<img src="{{ page.assets }}module2_lab1_step1_11_apikit_get_product_by_id.png" width="230px">

This flow gets a product based on a product id

---

**get:\orders\search**

<img src="{{ page.assets }}module2_lab1_step1_12_apikit_get_order_search.png" width="230px">

This flow returns orders based on search criteria

---

**get:\products\search**

<img src="{{ page.assets }}module2_lab1_step1_13_apikit_get_product_search.png" width="230px">

This flow returns products based on a search criteria

---
**get:\users\user\\\{user_id\}\shopping_cart**

<img src="{{ page.assets }}module2_lab1_step1_14_apikit_get_user_shopping_cart.png" width="230px">


This flow returns all the items in a users shopping cart based on a user id

---

**post:\users\user\\\{user_id\}\shopping_cart\confirmation**

<img src="{{ page.assets }}module2_lab1_step1_15_apikit_post_shopping_cart_confirm.png" width="230px">

This flow confirms the items to purchase in a users shopping cart.

## Step 2: Test the Generated API Project

1. To test the API, let’s run it within Studio first. Right click the application.

2. Select **Run As > Mule Application**.

    ![]({{ page.assets }}module2_lab1_step2_1_studio_run_as.png)

3. The application will start running, and the console will show the Mule Runtime logs

    ![]({{ page.assets }}module2_lab1_step2_2_studio_console_deployed.png)

    {: .note}
    Anypoint Studio deployed this application to an embedded Mule Runtime. There is no need to deploy to a separate Mule server environment. The developer will be able to develop and test the application locally until it’s ready to be deployed to a shared development or QA environment. . Test the application using the console. Click button **Open Console** on the APIkit Console tab.

    ![]({{ page.assets }}module2_lab1_step2_3_apikit_console_base_url.png)

4. A browser window opens at: [http://localhost:8081/console/](http://localhost:8081/console/).

5. Click open the `/products/search` resource on the left and click `GET`.

6. Click the `SEND` button on the bottom right to submit the request.

    ![]({{ page.assets }}module2_lab1_step2_4_browser_apikit_console_tryit.png)

7. Then tick the **Show optional parameters** and fill them in as below and click **SEND**.

    ![]({{ page.assets }}module2_lab1_step2_5_apikit_console_get_request.png)

    Scrolling down the third panel, you will see the mocked data that is defined in the imported RAML.

    ![]({{ page.assets }}module2_lab1_step2_6_apikit_console_response.png)    

    It is a live application deployed to the Mule runtime server but it is leveraging the RAML information to provide a “mocking” experience.

    Let’s go ahead and stop the application to get ready for the next lab.

8. Go to the console tab and press the red squared button to stop the Mule runtime server.

    ![]({{ page.assets }}module2_lab1_step2_7_studio_stop.png)

9. Right click on the project and close it

    ![]({{ page.assets }}module2_lab1_step2_7_studio_close.png)

## Summary

In this lab, we

- Downloaded the API definition directly into Anypoint Studio and added as a dependency
- Used the RAML specification to generate the initial implementation skeleton for the API.
- Auto-generated a skeleton project to implement the API.

This lab shows how APIkit quickly allows developers to import the designed REST API (RAML and supporting artifacts) to enable rapid API logic implementation in the generated MuleSoft flows. The APIkit also deploys the interactive console with the MuleSoft flows. The lab shows the extent to which a developer can deploy an on-premise and interactive API from the API specification in RAML.

It is important to note that the implementation of an API is a real MuleSoft application and allowed us to test the end-to-end API. Building integration applications and exposing APIs do not require additional knowledge making it easy for MuleSoft developers to work on both integrations and APIs.

- See the [APIkit](https://docs.mulesoft.com/apikit/latest/) documentation for more information.
- See the [Import an API Specification from Exchange](https://docs.mulesoft.com/studio/latest/import-api-specification-exchange) documentation for more information.
- See the [Anypoint Exchange](https://docs.mulesoft.com/exchange/) documentation for more information.
- See the [Error-Handling](https://docs.mulesoft.com/mule-runtime/latest/intro-error-handlers) documentation for more information.

**Congratulations! You have completed Lab 1.**

Please proceed to [Lab 2](./lab-2){: .btn .btn-blue  .mr-2  }