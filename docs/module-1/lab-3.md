---
parent: Module 1 - API Design
title: Lab 3 - Publish the Omni Channel API to Exchange
nav_order: 4
assets: "../../assets/images/module1/lab3/"
---
1. TOC
{:toc}

# Lab 3: Publish the Omni Channel API to Exchange
{: .no_toc }

## Overview
In Lab 1 we examined how to use **Anypoint Exchange** to facilitate the API discovery process. In Lab 2 we learned how to use **Anypoint Design Center** to design the specifications for a modern RESTful API. In this lab we will learn how to publish our API to **Anypoint Exchange** so that API consumers and API developers within the organization are able to leverage the API through "self-service".

One of the biggest challenges encountered by early adopters of SOA was how to scale their development activities. SOA maintains many of the reuse principles that we have discussed in our "API-led" approach to software development. Unfortunately, most SOA efforts could not achieve their reusability objectives because of a few limiting factors including:

- Poorly documented services
- Lack of mature tools to facilitate service discovery
- Over-reliance on limited number of knowledgable resources
- Burdensome processes for gaining access to services

As a result, many SOA-based initiatives were only able to partially achieve their reusability objectives because developers would adopt the path of least resistance and simply recreate services rather than reuse them. The **MuleSoft Anypoint Platform** addresses all of these issues by providing innovative tools like **Anypoint Exchange** that alleviate these challenges.

Let’s take a look at the process for publishing and documenting our reusable Omni Channel Experience API.

## Step 1: Complete the Omni Channel Experience API Specification
Before we get started, make sure you are logged into the **Anypoint Platform** and viewing the **Anypoint Design Center** screen. You should have your "**\<username\> - Omni Channel Experience API**" open and visible. We have only defined a few resources and methods at this point and need to author the full API in order to complete the exercise. Rather than build it from scratch, we have the ability to import an API specification previously constructed for the workshop.

1. Go to the Setup located at the top right corner.
2. In the drop down menu select **Import from Exchange**

    ![Import from Exchange]({{ page.assets }}module1_lab3_import_from_exchange.png "Import from Exchange")

3. In the search box type **Omni Channel Experience API**.
4. Select **Omni Channel Experience API**
5. Press **Import asset**

    ![Import Asset]({{ page.assets }}module1_lab3_choose_omni_channel_exp.png "Choose Omni Channel")

    You will be asked whether you want to replace the root file.

6. Choose **Yes**

    ![Root File]({{ page.assets }}module1_lab3_root_file.png "Root File")

    {: .note}
    Anypoint Design Center supports both RAML and Open API specification (OAS) languages when importing an API specification. OAS is a common format popularized by open-source Swagger tools.

7. Delete the RAML file **{Your Anypoint Username}-omni-channel-experience-api.raml** from your project explorer that we were previously editing. We no longer need that file since the API we just imported replaces that specification. Click the "ellipsis" menu next to your file and then click "delete":

    ![Delete RAML]({{ page.assets }}module1_lab3_ellipsis_delete.png "Delete RAML")

8. Delete the _omni-channel-experience-api.raml_ file.

    ![Delete RAML 2]({{ page.assets }}module1_lab3_ellipsis_delete2.png "Delete RAML 2")

9. Select the new RAML file we imported. You should see the following complete API specification:

    ![Full API]({{ page.assets }}module1_lab3_full_api.png "Full API")

    {: .note}
    Note the extensive use of references to other assets published in **Anypoint Exchange**. This API designer is using the features of Anypoint Exchange to efficiently reuse common data types, traits, and other API fragments.    

This fully formed API specification is ready to be published! Feel free to explore the API using the mocking service if desired.

{: .note}
The full API specification designates that a `client_id` and `client_secret` are required to use the API properly. We have not secured our API yet and will cover that in a future module. If you are trying to test the API using the **mocking service** then you can enter any value for these parameters and it will still work properly.

## Step 2: Publish Your API to Anypoint Exchange

1. Click on the **Publish** button to publish to Exchange.

    ![DC Publish]({{ page.assets }}module1_lab3_design_center_publish.png "DC Publish")

2. A popup window to capture the attributes that you want to publish to Exchange will appear. Most of these fields will be pre-populated and the RAML will be verified to ensure that there are no errors. Complete the **Asset version** field with `1.0.0`.

    ![Tags]({{ page.assets }}module1_lab3_publish_exchange_tags.png "Tags")

3. Click the "Publish to Exchange" button when you have finished reviewing the API name and version information. We have now published our Omni Channel Experience API to **Anypoint Exchange** for other developers to discover and consume!

    ![Publish Exchange]({{ page.assets }}module1_lab3_publish_exchange_success.png "Publish to Exchange")

Each time you publish an API Specification you will generate two assets:
- RAML Spec: This is the RAML Specification. Everybody on the organization can discover the spec and documentation.
- Rest Connector: This is a connector automatically generated for the API you have just defined. We will cover this in more details on Module 2 and Module 9.

## Step 3: Augment the Documentation for Your API
As demonstrated in Step 2, publishing your API to exchange is as easy as a few clicks. However, Exchange has really only just captured the basic attributes and documentation for your API at this point. Ensuring that your API is easy to find and "self-service ready" is your responsibility as an API designer. Let’s find our API in Exchange and add more to our API documentation.

1. Navigate to Exchange from the "hamburger menu" at the top left corner of Design Center.

    ![Navigate to Exchange]({{ page.assets }}module1_lab3_navigate_to_exchange.png "Navigate to Exchange")

    ![Navigate to Exchange 2]({{ page.assets }}module1_lab3_navigate_to_exchange2.png "Navigate to Exchange 2")

2. Use the search bar to find your API if it is not already visible in the list of Exchange assets. Click on your API once you find it:

    ![Exchange Select Asset]({{ page.assets }}module1_lab3_exchange_assets_select.png "Exchange Select Asset")

3. Click on the "Edit documentation" button to launch edit mode.

    ![Edit asset]({{ page.assets }}module1_lab3_exchange_edit.png "Edit asset")

4. Exchange assets are documented using "markdown" language. Markdown language is a format-agnostic syntax used for creating documentation independent of how it will be rendered (i.e. HTML, PDF, text, etc). It is used by popular applications like GitHub as the standard way to create software documentation. For more information on markdown please refer to:

    [https://guides.github.com/features/mastering-markdown/](https://guides.github.com/features/mastering-markdown/)

    Create your own documentation for your API, or copy the following text into the editor:

    ```markdown
    The **Omni Channel Experience API** is used to simplify access to Orders, Products, and the Shopping Cart features required for mobile and e-commerce applications.

    Developers requiring can easily compose innovative applications by leveraging this API to do the following:

    - Retrieve the Product Catalog and details about a product
    - Add products to a stateful shopping cart
    - Search for orders and retrieve their status
    - Create a new order
    ```

    ![Exchange Markdown]({{ page.assets }}module1_lab3_exchange_editor_markdown.png "Exchange Markdown")

    {: .note }
    If you prefer WYSIWYG editing then you can click on the "Visual" button at the top and be presented with a visual editor instead of the markdown-based editor. Both editors produce markdown at the end.

5. Click on the "Save" button. Your draft of the API documentation is saved but not published until you formally "Publish" the final updates to Exchange, so feel free to save multiple drafts as you create your documentation.
    ![Editor Draft]({{ page.assets }}module1_lab3_exchange_editor_draft_page.png "Editor Draft")

    - You can press Publish, Discard , Edit or View Published.
        - Discard - Throw away changes
        - View Published - View published documentation
        - Edit - Lets you edit the content
        - Publish - Publish documentation changes to exchange

6. Press the **Publish** button. Your documentation changes are now visible on Exchange!

    ![Editor Final Page]({{ page.assets }}module1_lab3_exchange_editor_final_page.png "Editor Final Page")

    Exchange documentation can be comprised of multiple pages. So far we have just created the home page for our API.

    It is common practice to include links to other content like video tutorials in our Exchange documentation. This makes it easier for the interested developer to gain access to all of the documentation required to learn how to use the API.

## Step 4: Validate the API
Like in Design Center, in Exchange you can look at the **API Summary**. The **API Summary** gives you the opportunity to do a request and call the mock service or an implementation of the API.

We are going to see how it can be done.

1. Go to the **API Summary**

    ![API Summary]({{ page.assets }}module1_lab4_exchange_api_summary.png "API Summary")

2. Press **/products/search** --> `[GET]`

    ![Product Search]({{ page.assets }}module1_lab4_exchange_product_search.png "Product Search")

3. On the right side of the panel you will see the **GET** method to be called.

    You can select from a combo list of available implementation endpoints to call. In this case we only have the mock service and what’s defined in the specification. Later in Module 5, we will add to this list of available endpoints once we complete the API Life Cycle.

    You can see the **Parameters, Headers** and how they are filled.

    In the **Parameters** tab you can see all the optional parameters that can be send.

    ![Product Search]({{ page.assets }}module1_lab4_product_search.png "Product Search")

4. Press **SEND** and see what happens.

    ![Product Search Response]({{ page.assets }}module1_lab4_exchange_product_search_response.png "Product Search Response")

    - You can see the mock response with the following information:
    
        The http response and the response time.

        ![HTTP Response]({{ page.assets }}module1_lab4_http_response.png "HTTP Response")
    
    - Under the <img src="../../assets/images/module1/icons_3_dots_white.png" width="25px"> you can find:

        ![Message icon]({{ page.assets }}module1_lab4_message_icons.png "Message Icons")
    
    - These options let you download the response, copy the response to clipboard, save it as HAR 1.2 and format the view.

    - The **Response detail** option shows the Request Detail. URL, Response and Request headers.
        ![Response Details]({{ page.assets }}module1_lab4_response_details.png "Response Details")


**Congratulations! You have completed all of the labs in this Module!**

You can now proceed to the next Module