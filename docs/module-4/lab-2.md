---
parent: Module 4 - Uniquitous Connectivity - Database
title: "Lab 2: Connect the Product API to a MySQL Database"
nav_order: 2
assets: "../../assets/images/module4/lab2/"
---
1. TOC
{:toc}

# Lab 2: Connect the Product API to a MySQL Database
{: .no_toc }

## Overview
In this lab, we will implement the generated subflows by connecting to a MySQL database. The operations are real simple so you are going to implement all the operations.

## Step 1: Add Database Connector to the project
We need to add the Database connector to the project. To do that follow these steps.

1. Go to the Mule Palette

2. Click **Add Module**

    ![]({{ page.assets }}lab2_add_module.png)

3. Select **Database Connector**.

4. Drag and Drop Icon to the light blue panel.

    ![]({{ page.assets }}lab2_add_database_module.png)

    Going back to the Palette, you now will see the Database connector.

    ![]({{ page.assets }}lab2_palette_database_connector.png)

## Step 2: Create a Property File

A good practice in Mule is to parameterize connectivity information. To configure the database connector we will leverage this best practice.

1. Create a folder called `config` in `src/main/resources`

2. Create a file called `configuration.yaml` in the `src/main/resources/config`

    ![]({{ page.assets }}lab2_config_file.png)

3. Insert the following text into configuration.yaml file.

    ```yaml
        mysql:
        host: "mythicaldb.workshops.mulesoft.com"
        port: "6033"
        user: "product"
        password: "Mule1379"
        database: "products_test"
    ```

    {: .note-title }
    > TIP
    >
    >The password can be encrypted using the Mule Credential Vault.

4. Save the `configuration.yaml` file

5. Go back to the api.xml file. Press the **Global Elements** tab.

    ![]({{ page.assets }}lab2_global_elements.png)

6. Press the **Create** button and search for **Configuration Properties**

    ![]({{ page.assets }}lab2_configuration_properties.png)

7. Press OK.

8. In the file text box write `config/configuration.yaml`. You can also browse to the file.

    ![]({{ page.assets }}lab2_configuration_properties_file.png)

## Step 3: Implement Get Products Flow
In this step, you will implement the get:/product flow. To get the products from the database, you will need to:

- Retrieve the records from the database.

- Transform the records to json format.

The following instructions will guide you through the process:

1. Locate the `get product` flow.

    ![]({{ page.assets }}lab2_delete_get_products_set_payload.png)

    {: .warn }
    **DOUBLE CHECK**: Make sure you are working in the `get:\product:api-config` flow


2. Now you will add the Database connector to the flow.

3. Click on the database connector, and drag and drop the Select icon before the transformation.

    {: .warn }
    Drop the Database Connector in the Process section, not the Source section.

4. Double-Click on the **Database** icon and a configuration panel will be shown.

5. Change its name to **Get Products**.

    ![]({{ page.assets }}lab2_name_database_component_get_products.png)

6. Now that you have your properties defined, let’s add a new connector configuration.

    Go back to the database connector you dropped, click to open properties and click on the <img src="{{ page.assets }}lab2_studio_plus_button.png" width="25px"> button on the Connector Configuration.

7. In **Name** write `Products_Database_Configuration`

8. In **Connection** select MySQL Connection.

    When you select the database connection it asks you to add the JDBC Driver.

9. Press **Configure…​** button. A new window will open.

10. Select **Add Maven depenency**

11. Complete with the following data:

- **Group id**: `mysql`

- **Artifact id**: `mysql-connector-java`

- **Version**: `8.0.20`

    ![]({{ page.assets }}lab2_add_jdbc_dependency.png)

12. Click **Finish**.

    {: .note }
    Anypoint Studio will automatically download the dependency.

13. For our database configuration specify the following values for the properties:

    - **Host**: `${mysql.host}`

    - **Port**: `${mysql.port}`

    - **User**: `${mysql.user}`

    - **Password**: `${mysql.password}`

    - **Database**: `${mysql.database}`

    ![]({{ page.assets }}lab2_db_config_1.png)

    {:. note }
    Mule resolves the ${key} to the configuration properties that we put in the configuration.yaml file by default without any extra configuration needed.

14. Click **Test Connection** to verify the connectivity

    ![]({{ page.assets }}lab2_db_config_test_connection_success.png)

15. Click **OK**

16. Click **OK** again to close the MySQL Configuration.

    Now that we have the Database configured, we will add the query.

    ```sql    
        SELECT  p.id, p.name, p.description, p.product_number, p.manufactured, p.colors, p.categories, p.stock, p.safety_stock_level, p.standard_cost, p.list_price, p.size, p.size_unit_measure_code, p.weight, p.weight_unit_measure_code, p.days_to_manufacture, p.images,  p.modified_date, p.created_date
        FROM product p
        LIMIT 10
    ```
    {:. note }
    We are limiting the query to retrieve only up to 10 items with the "LIMIT 10" parameter.

    ![]({{ page.assets }}lab2_get_products_query.png)

    {: .note }
    Many customers use their favorite query tool (SQL Query Analyzer, TOAD, DBVisualizer, …) to craft the query they want and then paste it into textbox in Studio.

    Now we need to transform the records from the database to JSON format.

17. Double-Click on the **Transform Message** icon. You will find the fields that come from the query on the left, and the fields that the API will return on the right. You can graphically map fields by dragging between fields.

    This Transform Message component uses MuleSoft’s universal DataWeave transformation language for transforming data from what format to another. There is no need to use xpath/XSLT for XML, code for JSON, code for CSV, etc. Use DataWeave for all transformations. Dataweave is a simple, powerful tool to query and transform data inside of Mule.

    ![]({{ page.assets }}lab2_get_products_transform_illustration.png)

18. Remove the existing mapping and copy the mapping below into the text view of the dataweave transform.

    ```
        %dw 2.0
        output application/json
        ---
        payload map (product, index) -> {
            id: product.id,
            categories: (product.categories default "") splitBy ",",
            colors: (product.colors default "") splitBy ",",
            createdDate: product.created_date as String {format: "yyyy-MM-dd"},
            modifiedDate: product.modified_date as String {format: "yyyy-MM-dd"},
            safetyStockLevel: product.safety_stock_level as Number,
            stock: product.stock as Number,
            daysToManufacture: product.days_to_manufacture,
            name: product.name,
            description: product.description,
            images: (product.images default "") splitBy ",",
            listPrice: product.list_price,
            manufactured: product.manufactured,
            productNumber: product.product_number,
            size: product.size,
            sizeUnitMeasureCode: product.size_unit_measure_code,
            standardCost: product.standard_cost,
            weightUnitMeasureCode: product.weight_unit_measure_code,
            weight: product.weight

        }
    ```
    {: .note }
    Since colours, categories and images are arrays and in the database they are saved as comma-separated Strings, we added to add a function called Split By to the mapping that splits the single record into a list of them separated by ",". There are a number of functions available within Dataweave to support more complex mapping requirements. See [Dataweave](https://docs.mulesoft.com/dataweave/latest/) documentation for more information.

    ![]({{ page.assets }}lab2_get_products_transform_complete.png)

19. This concludes this step. Click save in the transform message window.

20. Now we are ready to test our API implementation.

    Follow the same steps from Lab 1.

    - Start the application.

    - Check the logs to make sure it is deployed successfully.

    - Test the Get Products using the GET method on the Product using the console, or Postman. You may need to set `Content-Type` and `Accept` headers to `application/json`. Ask your instructor for assistance if you receive error responses.

21. Now you get the response from the API, the product list is being retrieved from the MySQL database and you can check that the information is different from the one you saw in Lab 1.

    ![]({{ page.assets }}lab2_get_products_postman_results.png)

    *(Example result. Results may differ.)*

    Now that you have confirmed this flow is working, move onto the next implementation. If you have any problems, call over the instructor before continuing.

## Step 4: Implement Get Specific Product Flow

In this step, you are going to implement the get:/product/{id} flow.

To get the product from the database, you must:

    - Retrieve the records from the database.

    - Check if it was found.

    - If it was found, transform the record to json format.

    - If it was not found, create a "Not found" response.

The following instructions will guide you through the process:

1. Remove all the Transform icons from the `get:/product/{id}` flow. To do that you just right-click on the icon and select Delete. Or select it and hit Delete key.

2. Drag & Drop a Select component from the database connector.

3. Double Click on the icon to open the configuration tab.

4. Complete with the following parameters:

    - **Display Name**: `Get Product By ID`

    - **Connector configuration**: Select `Products_Database_Configuration`

    - **SQL Query Text**:

        ```sql
            SELECT p.id, p.name, p.description, p.product_number, p.manufactured, p.colors, p.categories, p.stock, p.safety_stock_level, p.standard_cost, p.list_price, p.size, p.size_unit_measure_code, p.weight, p.weight_unit_measure_code, p.days_to_manufacture, p.images,  p.modified_date, p.created_date
            FROM product p
            WHERE p.id = :id
        ```
5. Let’s add the Input Parameters. Click on the fx button, then click on the Transformation Icon.

    ![]({{ page.assets }}lab2_press_transform_button.png)

    A new window will appear. Similar to a dataweave transformation

6. Search for id. It should be in Attributes → uriParams → id.

7. Drag and drop the id from input to the output (Add `%dw 2.0` code if it’s not present)

    ![]({{ page.assets }}lab2_select_transform.png)

8. Press the <img src="{{ page.assets }}lab2_done_button.png" width="25px"> button

    {: .note }
    Here we are using a parametrized query. You define the variable in the query and then you complete with the parameters on the other text box.

    ![]({{ page.assets }}lab2_select_by_id_configuration.png)

    We are going to validate if the product was found. In case it doesn’t exist we are going to throw an exception. To do that we need to add the **Validations Module**

9. Go to the Mule Palette and add the Validation Module (Select latest version).

10. Select the Validation connector and drag & drop **is not empty** collection.

    The Flow should look similar to this:

    ![]({{ page.assets }}lab2_add_validator.png)

    This validator component is going to throw an Exception of type `VALIDATION:EMPTY_COLLECTION` What we want to do is to catch that Exception and send a 404 (Not Found) response.

11. To do that go to the Error Handling that is defined in the **api-main** flow.

12. Find the component **On Error Propagate type: APIKIT:NOT_FOUND**

    ![]({{ page.assets }}lab2_error_handling.png)

13. Click on it.

    You will see the configuration properties.

14. Click on the **Type** textbox.

    ![]({{ page.assets }}lab2_click_type.png)

    A checkbox list will open with all the possible errors that can be catched.

15. Select **VALIDATION:EMPTY_COLLECTION**

    ![]({{ page.assets }}lab2_validation_empty_collection.png)

    {: .warn }

    If the Error is not listed write the error name manually after the `APIKIT:NOT_FOUND` text, separated by a comma. The final string should be `APIKIT:NOT_FOUND,VALIDATION:EMPTY_COLLECTION`.

16. Press Save icon on the top right side of the panel.

17. Now we go back to the Get method we were working before and we will add a Transform component at the end of the flow.

    Copy this script into the text view of the DataWeave transform:

    ```
        %dw 2.0
        output application/json
        var product = payload[0]
        ---
        {
            id: product.id,
            name: product.name,
            description: product.description,
            manufactured: product.manufactured,
            productNumber: product.product_number,
            colors: (product.colors default "") splitBy "," ,
            categories:(product.categories default "") splitBy "," ,
            safetyStockLevel: product.safety_stock_level,
            standardCost: (product.standard_cost default "0.0") as String {format: "##.##"} as Number,
            listPrice: (product.list_price default "0.0") as String {format: "##.##"} as Number,
            stock: product.stock,
            safetyStockLevel: product.safety_stock_level,
            daysToManufacture: product.days_to_manufacture,
            size: product.size,
            sizeUnitMeasureCode: product.size_unit_measure_code,
            weight: product.weight,
            weightUnitMeasureCode: product.weight_unit_measure_code,
            daysToManufacture: product.days_to_manufacture,
            images: (product.images default "") splitBy ",",
            modifiedDate: (product.modified_date default "") as Date {format: "yyyy-MM-dd"},
            createdDate: (product.created_date default "") as Date {format: "yyyy-MM-dd"}
        }
    ```

18. **Transform Message** will transform the DB structure to the JSON format defined for the response. Database selects always return a list of rows, so one method to obtain a single result is to take the first one.

    ![]({{ page.assets }}lab2_get_product_transform_config.png)

19. Now you can run the API as in the preceding step, and check calling GET on "**product/{id}**" using an existing and a non-existing ID.

20. Stop the Mule Runtime using the console view after the test.

    The final flow should like similar to this.

    ![]({{ page.assets }}lab2_get_by_id_final_flow.png)

    Now that you have confirmed this flow is working, move onto the next implementation. If you have any problems, call over the instructor before continuing.

## Step 5: Implement Create Product Flow

In this step, you will implement the post:/product flow. To create the product in the database, you must:

- Insert the product in the DB

- Return the product plus the id as a response.

The following instructions will guide you through the process:

1. Remove the Transform component from the `post:\product:application\json:api-config` flow. To do that you just right-click on it and select Delete or select it and hit the Delete key.

2. Using the Palette, drag & drop the necessary components to create a flow like this:

    ![]({{ page.assets }}lab2_post_product_initial.png)

3. Start by configure the insert component. Set the parameters as follows to save the product into the database:

- **Display Name**: `Insert Product`

- **Connector configuration**: `Products_Database_Configuration`

- **SQL Query Text**:

    ```sql
        INSERT INTO product(name, description, product_number, manufactured, colors, categories, stock, safety_stock_level, standard_cost, list_price, size, size_unit_measure_code, weight, weight_unit_measure_code, days_to_manufacture, images, modified_date, created_date)
        VALUES(:name, :description, :productNumber, :manufactured, :colors,  :categories, :stock, :safetyStockLevel, :standardCost, :listPrice, :size, :sizeUnitMeasureCode, :weight, :weightUnitMeasureCode, :daysToManufacture, :images,CURDATE(), CURDATE() );
    ```

4. Click the function button.

5. Click on the <img src="{{ page.assets }}lab2_transform_button.png" width="25px"> button to open a dataweave panel.

6. Complete the Transformation with the following values:

    ```
        %dw 2.0
        output application/json
        fun getManufacturedCode(value) = if (value == true) 1 else 0
        ---
        {
                categories: (payload.categories default []) joinBy ",",
                colors: (payload.colors  default []) joinBy ",",
                daysToManufacture: payload.daysToManufacture,
                description: payload.description,
                images: (payload.images  default []) joinBy ",",
                listPrice: payload.listPrice,
                manufactured: getManufacturedCode(payload.manufactured),
                name: payload.name,
                productNumber: payload.productNumber,
                safetyStockLevel: payload.safetyStockLevel,
                size: payload.size,
                sizeUnitMeasureCode: payload.sizeUnitMeasureCode,
                standardCost: payload.standardCost,
                stock: payload.stock,
                weight: payload.weight,
                weightUnitMeasureCode: payload.weightUnitMeasureCode
        }
    ```

    ![]({{ page.assets }}lab2_post_product_database_config.png)

    {: .note }
    Look that in Dataweave you can declare functions and use *if else* structures. This is used to transform the manufactured value from boolean to integer.

    As this operation creates a new product, you will need to obtain the newly generated id. How this is implemented can vary with different database engines' drivers.

7. In the case of MySQL, go to "Advanced" in the "Insert Product" block.

8. Find **Auto Generated Keys** section.

9. Set **Auto generate keys** to `True`

10. In the **Auto generated keys columns names** select **Edit inline**.

- Add value `id` in the table below.

- The configuration will be like this:

    ![]({{ page.assets }}lab2_post_product_database_config_2.png)

13. Now we are going to configure the response Transform Message. In this step we need to merge the original request payload with the returned ID from the database. The next script will do the trick. We are getting the original request payload and adding a field called "id".

    ![]({{ page.assets }}lab2_transform_message.png)

14. Add the following transformation:

    ```
        %dw 2.0
        output application/json
        ---
        payload ++
        id: vars.generated_key
    ```

    ![]({{ page.assets }}lab2_transform_message_detail.png)

    15. Finally let’s configure a Custom Business Event. Set the parameters to record the created product’s information for future business analysis:

        ![]({{ page.assets }}lab2_custom_business_event.png)

        ![]({{ page.assets }}lab2_custom_business_event_design.png)

        a. **Display Name**: `New Product Created Event`

        b. **Event Name**: `New Product Created Event`

        c. In the **Key Performance Indicators** table, add these two attributes:

        | Name         | Expression/Value |
        |:-------------|:-----------------| 
        | Product ID   | `#[payload.id]`  |
        | Product Name | `#[payload.name]`|

        ![]({{ page.assets }}lab2_post_product_event_config.png)

16. Now you can run the API as in the preceding steps, and check it is working by calling POST on "products".

    {: .warn }
    The `productNumber` must be unique, you need to change the `productNumber` with a random value. If you have problems with this, ask your instructor to provide a value by querying the database directly to check for existing records

    Stop the Mule Runtime using the console view after the test.

    Now that you have confirmed this flow is working, move onto the next implementation. If you have any problems, call over the instructor before continuing.

## Step 6: Implement Update Product Flow

In this step, we are going to implement the `put:\product{id}` flow. To update the product in the database, we just need to call the database.

The following instructions will guide you through the process:

1. Remove the components from the `put:\product{id}` flow. To do that you just right-click on each and select Delete or select it and hit Delete key.

2. Using the Palette, drag & drop the needed components to create a flow like this:

    ![]({{ page.assets }}lab2_put_product_initial.png)

3. Configure the Database with this parameters:

- **Display Name**: `Update Product`

- **Connector configuration**: `Products_Database_Configuration`

- **SQL Query Text**:

    ```sql
    update product
    set name = :name, description = :description, product_number = :productNumber, manufactured = :manufactured, colors = :colors,
    categories= :categories, stock = :stock, safety_stock_level = :safetyStockLevel, standard_cost = :standardCost, list_price = :listPrice, size = :size,
    size_unit_measure_code = :sizeUnitMeasureCode, weight = :weight, weight_unit_measure_code = :weightUnitMeasureCode, days_to_manufacture = :daysToManufacture,
    images = :images,  modified_date = CURDATE()
    where id = :id
    ```

4. Click on <img src="{{ page.assets }}lab2_fx_button.png" width="25px"> and then <img src="{{ page.assets }}lab2_transform_button.png" width="25px"> button to open a dataweave panel.

5. Complete with the following script"

    ```
    %dw 2.0
    output application/java
    fun getManufacturedCode(value) = if (value == true) 1 else 0
    ---
    {
            id: attributes.uriParams.id,
            categories: (payload.categories default []) joinBy ",",
            colors: (payload.colors default []) joinBy ",",
            daysToManufacture: payload.daysToManufacture,
            description: payload.description,
            images: (payload.images default []) joinBy ",",
            listPrice: payload.listPrice,
            manufactured: getManufacturedCode(payload.manufactured),
            name: payload.name,
            productNumber: payload.productNumber,
            safetyStockLevel: payload.safetyStockLevel,
            size: payload.size,
            sizeUnitMeasureCode: payload.sizeUnitMeasureCode,
            standardCost: payload.standardCost,
            stock: payload.stock,
            weight: payload.weight,
            weightUnitMeasureCode: payload.weightUnitMeasureCode
    }
    ```

    Like we did in the previous step we are going to log the number of records updated

6. Focus on the logger component

7. Press the <img src="{{ page.assets }}lab2_fx_button.png" width="25px"> button to add a dataweave expression.

8. Add the following dataweave expression:

    ```
    "Number of affected rows: " ++ ((payload.affectedRows default -1 )as String)
    ```

    ![]({{ page.assets }}lab2_logger_update_config.png)

    Stop the Mule Runtime using the console view after the test.

    Now that you have confirmed this flow is working, move onto the next implementation. If you have any problems, call over the instructor before continuing.

## Step 7: Implement Delete Product Flow

In this step, we are going to implement the delete:\product{id} flow. To delete the product in the database, we just need to call the database.

The following instructions will guide you through the process:

1. Remove all the components from the delete:\product{id} flow. To do that you just right-click on each component and select Delete or select a component and hit the Delete key.

2. Using the Palette, drag & drop the needed components to create a flow like this:

    ![]({{ page.assets }}lab2_delete_product_initial.png)

3. Configure the Database component as follows:

- **Display Name**: `Delete Product`

- **Connector configuration**: `Products_Database_Configuration`

- **SQL Query Text** (press function button):
    ```sql
      delete from product where id=:id
    ```
- **Input Parameters**:

    ```
    {
    id:attributes.uriParams.id
    }
    ```

    We will add a logger to see how many records where deleted.

4. Focus on the logger component

5. Press the <img src="{{ page.assets }}lab2_fx_button.png" width="25px"> button to add a dataweave expression.

6. Add the following dataweave expression:

    ```
        "Number of records deleted: " ++ (payload as String)
    ```

    ![]({{ page.assets }}lab2_logger_config.png)

    We are returning a `204`, so there will be no message response.

    Now you can run the API as in the preceding steps, and check it is working by calling `DELETE` on "`products/{id}`" for an id you know exists (such as the one created in the previous step).

    {: .note }
    Note that if you have more time you can extend the implementation as in the `GET /products/{id}` flow to respond with a not found response if the record does not exist.

    Stop the Mule Runtime using the console view after the test.

    If you have any problems, call over the instructor before continuing.

Now that we have a working implementation, in the next lab we will test it end to end and add unit tests.

## Summary
You completed the following steps:

- [Overview](#overview)

- [Step 1: Add Database Connector to the project](#step-1-add-database-connector-to-the-project)

- [Step 2: Create a Property File](#step-2-create-a-property-file)

- [Step 3: Implement Get Products Flow](#step-3-implement-get-products-flow)

- [Step 4: Implement Get Specific Product Flow](#step-4-implement-get-specific-product-flow)

- [Step 5: Implement Create Product Flow](#step-5-implement-create-product-flow)

- [Step 6: Implement Update Product Flow](#step-6-implement-update-product-flow)

- [Step 7: Implement Delete Product Flow](#step-7-implement-delete-product-flow)

In this module we implemented the System API against the MySQL database. We ran and tested the API on the local Mule Runtime which comes embedded in Anypoint Studio.

You were introduced to the use of Dataweave, a powerful tool to transform messages from a database record to json format in this case. You also saw how simple accessing a database can be using the Database Connector.

It is important to note that the implementation of an API is a real Mule application. Building integration applications and exposing APIs do not require additional knowledge, making it easy for Mule developers to work on both integrations and APIs.

See the [Dataweave](https://docs.mulesoft.com/dataweave/latest/) documentation for more information.

See the [Database Connector](https://docs.mulesoft.com/db-connector/latest/) documentation for more information.

See the [Configuration Properties](https://docs.mulesoft.com/mule-runtime/latest/configuring-properties) documentation for more information.

See the [Secure Configuration Properties](https://docs.mulesoft.com/mule-runtime/latest/secure-configuration-properties) documentation for more information.

Please proceed to [Lab 3](./lab-3): Run and Debug the API