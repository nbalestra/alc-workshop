---
parent: Module 4 - Uniquitous Connectivity - Database
title: "Lab 3: Run and Debug the API"
nav_order: 3
assets: "../../assets/images/module4/lab3/"
---
1. TOC
{:toc}

# Lab 3: Run and Debug the API
{: .no_toc }

## Overview

In this lab we’ll run through an end to end test, and add unit testing coverage.

## Step 1: Full test
In this step, you will go through a use case of creating a product, making an update and finally deleting it. These instructions will assume you are using POSTMAN, but the API Console can be used directly within Anypoint Studio. Along the way you’ll check the state of the product. You will need to:

- Create a product.

- Check for its existence.

- Perform an update.

- Check the change has been applied.

- Delete the product.

- Confirm the product is no longer available.

The following instructions will guide you through the process:

1. Right click the application. Select **Run As > Mule Application**.

2. The application will start running, and after a few seconds the API console will be available for us to test out the integration.

3. Open up POSTMAN (or an equivalent).

4. Start by creating a product. Select the POST method and set the body using the example from the RAML specification, or the example below:

    ```json
        {
        "id": 1,
        "name": "ELMO",
        "description": "Playskool sesame street play all day Elmo",
        "productNumber": "UUID34234923934293",
        "manufactured": true,
        "safetyStockLevel": 540,
        "standardCost": 78,
        "listPrice": 99.9,
        "size": "200x300",
        "sizeUnitMeasureCode": "in",
        "weightUnitMeasureCode": "gr",
        "weight": 400,
        "daysToManufacture": 10,
        "categories": [
            "playskool",
            "sesame street"
        ],
        "stock": 2800,
        "colors": [
            "blue",
            "red",
            "green"
        ],
        "images": [
            "/wcsstore/HTSStorefrontAssetStore/Attachment/HomePage/Product eSpots/Play-All-Day-Elmo.png"
        ]
        }
    ```

    {: .warn }
    The productNumber must be unique, so if this record has already been created, try setting a different random value, or ask you instructor to provide a value by querying the database directly to check for existing records.

5. Set the endpoint to be [http://localhost:8081/api/product](http://localhost:8081/api/product), and add a `Content-Type` header with value `application/json`.
    
    ![]({{ page.assets }}lab3_postman_post.png)

6. Review the mock body of the operation, which in this case, contains the name, description, category, etc. of the product to be created, and click the Send button.

7. If everything was successful, you should receive a 201 response code, and a response body that contains the new product ID.

    **Take note of this ID**. We will use it in the following steps.
    
    ![]({{ page.assets }}lab3_postman_post_response.png)

8. Open a new tab for the GET method on the `/product/{id}` resource.

9. Enter the ID from the product you created before, and click Send.

    If everyhing goes Ok, you should get a 200 status response code, with the body of the product you created before.

    ![]({{ page.assets }}lab3_postman_get_response_1.png)

8. You have created a product and confirmed it exists. Now update it. Open a new tab and select PUT.

9. Again, specify the product ID to update in the URL (set the url to [http://localhost:8081/api/product/{id}](http://localhost:8081/api/product/{id} with your product id.), which is the same ID from before, and take a moment to check how you are changing the product (pick a minor update or your choice). Set a `Content-Type` header with value `application/json`.

    ![]({{ page.assets }}lab3_postman_put_request.png)

10. After taking note of your update click **Send**.

11. If everything goes ok, you should get a 204 status response code and no body.

    After you update the information, perform a **GET** operation again under the `/product/{id}` resource, specifying the ID of your product, and you should obtain the updated information.

12. To end with your basic operations, delete the product. Cretae a new tab and select DELETE as the HTTP verb. Set the url to [http://localhost:8081/api/product/{id}](http://localhost:8081/api/product/{id})  with your product id.

    If everything goes Ok, you should get a 204 status response code and no body.

    ![]({{ page.assets }}lab3_postman_delete_response.png)

13. To confirm we have deleted it, perform a GET operation under the `/product/{id}` resource, specifying the ID of your product. You should get no information. In fact, as the product and resource do not exist, you should get a 404 status response code, and the message "Resource not found", which is what we want.

    ![]({{ page.assets }}lab3_postman_get_after_delete.png)

    Stop the Mule runtime.

## Step 2: Add MUnit to your API
MUnit is an application testing framework that allows you to easily build automated tests for your integrations and APIs. It provides a full suite of integration and unit test capabilities, and is fully integrated with Maven and Surefire for integration with your continuous testing and deployment environment.

In this step you will use the MUnit for RAML feature to create a test for a specific flow.

The following instructions will guide you through the process.

1. Inside api-product.xml file, right click on the "APIKit Router" component and select "Create Test Suite for api.xml from API Specification".

    ![]({{ page.assets }}lab3_munit_generate.png)

    A new *sapi-products-db-apikit-test.xml* file will be generated within the folder *src/test/munit* and a viewer will open.

    Concentrate on the test flow called "get:/product:api-config-200-application/json-FlowTest". We are going to mock the Database invocation, so you do not have to depend on external resources.

2. Go to the palette and select the **Mock When** operation from the **MUnit Tools** component.

    ![]({{ page.assets }}lab3_mock_when.png)

3. Drag & drop in the Behavior section

    ![]({{ page.assets }}lab3_munit_mock_view.png)

4. Click on the Mock component configuration.

    ![]({{ page.assets }}lab3_munit_mock_config_1.png)

5. In the **General** Panel complete with the following data

- **Processor**: `db:select`

- In the **Then return** section, select **Payload** tab

6. Complete the field **Value** with this representation of a product simulating the Database response (press `function` button first):

    ```
        #[
            [{
                id: 1,
                name: "ELMO",
                description: "Playskool sesame street play all day Elmo",
                product_number: "UUID34234923934293",
                manufactured: true,
                safety_stock_level: 540,
                standard_cost: 78,
                list_price: 99.9,
                size: "200x300",
                size_unit_measure_code: "in",
                weight_unit_measure_code: "gr",
                weight: 400,
                days_to_manufacture: 10,
                categories: "playskool, sesame street",
                stock: 2800,
                colors: "blue, red, green",
                images: "/wcsstore/HTSStorefrontAssetStore/Attachment/HomePage/Product_eSpots/Play-All-Day-Elmo.png",
                created_date: now(),
                modified_date: now()
            }, {
                id: 2,
                name: "ELMO",
                description: "Playskool sesame street play all day Elmo",
                product_number: "UUID34234923934293",
                manufactured: true,
                safety_stock_level: 540,
                standard_cost: 78,
                list_price: 99.9,
                size: "200x300",
                size_unit_measure_code: "in",
                weight_unit_measure_code: "gr",
                weight: 400,
                days_to_manufacture: 10,
                categories: "playskool, sesame street",
                stock: 2800,
                colors: "blue, red, green",
                images: "/wcssatore/HTSStorefrontAssetStore/Attachment/HomePage/Product_eSpots/Play-All-Day-Elmo.png",
                created_date: now(),
                modified_date: now()
            }]
            ]
    ```

7. Now complete the field `Media-Type` with the value `application/java`

8. The final configuration for the `Then return` section should look like this:

    ![]({{ page.assets }}lab3_munit_mock_get_products_database.png)

9. Check the assert called "Assert That - Payload is Expected", adding a simple expression that checks that the result is the expected one. In this case you can simply check that the "id" that comes in the JSON response equals "1" (which is what you set it to in the mocked response) extracting it with a DataWeave inline expression.

    Set these attributes on the component:

    - **Expression**: `#[payload[0].id]`

    - **Is**: #`[MunitTools::equalTo(1)]`

    - Failure Message: `The response payload is not correct!`

    ![]({{ page.assets }}lab3_munit_mock_get_products_assert.png)

10. To run the test, right click on the flow name and select "Run MUnit test". This will open the MUnit Tab and shows the execution.

    ![]({{ page.assets }}lab3_munit_mock_get_products_run.png)

11. If everything goes OK, you will see the test executed successfully.

    ![]({{ page.assets }}lab3_munit_mock_get_products_result.png)

If you have any problems, call over the instructor to assist.

You can download a complete project from exchange.

## Summary
In this module we ran through a full set of tests, and added unit testing into the API.

- [Overview](#overview)

- [Step 1: Full test](#step-1-full-test)

- [Step 2: Add MUnit to your API](#step-2-add-munit-to-your-api)

We saw how easy it is to use MUnit to provide unit test coverage for the API.

See the [MUnit](https://docs.mulesoft.com/munit/latest/) documentation for more information.

**Congratulations! You have completed Lab 3.**

Please proceed to the optional [Lab 4](./lab-4)