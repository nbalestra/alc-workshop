---
parent: Module 3 - Salesforce Integration
title: "Lab 4: (Optional) Error Handling"
nav_order: 3
assets: "../../assets/images/module3-sf/lab4/"
---
1. TOC
{:toc}

# Lab 3: Run the Mule app from Anypoint Studio
{: .no_toc }

## Overview
In Lab 2, you built the flow that will create a new user. If there’s an error in Salesforce, you’ll receive a Bad Request. This error message doesn’t give you full visibility into what really happened. In this lab you are going to learn how to Handle errors.

## Step 1: Add Try Scope
The Try scope enables you to handle errors that may occur when attempting to execute any of the components inside the Try scope.

1. Go to the palette and search for the **Try** component.

2. Insert the Try component after the Salesforce connector.

3. Move the Validation icon inside the Try component.

    ![]({{ page.assets }}module3_lab2_try_in_flow.png)

## Step 2: Catch the Error
When you handle the error you have two options.

- **On Error Continue**: This will catch the error and continue with the flow execution.

- **On Error Propagate**: This option will catch the error, but will throw it up.

You could eventually use one or the other depending on the error type.

In this case, we are going to propagate the error.

To do that, we are going to add an **On Error Propagate** handler to the Error handling part.

1. Go to the palette and search for On Error Propagate. Drag and Drop the component inside the Error handling part.

    ![]({{ page.assets }}module3_lab2_on_error_propagate.png)

2. Click on the component.

3. In the Type section click the magnifier glass on the right

4. Select `VALIDATION:INVALID_BOOLEAN`

    ![]({{ page.assets }}module3_lab2_on_error_propagate_select_type.png)

## Step 3: Log Salesforce Response
In this step, the idea is to log the Salesforce Response.

1. Add a Logger component to log the response.

    ![]({{ page.assets }}module3_lab2_add_logger.png)

2. Configure the component with the value `output application/json --- payload`

    ![]({{ page.assets }}module3_lab2_add_logger_conf.png)

    {: .note}
    When you press the f(x) button, you will see that a `#[]` is added to the field. This is because we are going to use a dataweave expression to log the Salesforce response.

## Step 4: Raise a new Error

After logging the response, we are going to raise a new error

1. Search for **Raise Error** component and insert after the logger.

    ![]({{ page.assets}}module3_lab2_add_raise_error.png)

2. Configure the component with the following values:

    - **Type**: `APP:CONFLICT`

    - **Description**: `#[error.description]`

        In the last step, we are raising a new error type called **APP:CONFLICT** and we are populating the details, with the Salesforce error message.

# Step 5: Handle a new Error
Now we are going to build the Response for this exception.

1. Go to the APIKit Error handling. Add an **On Error Propagate** component at the bottom.

    ![]({{ page.assets }}module3_lab2_on_error_propagate_apikit.png)

2. Click on the Component and configure the **Type** value with `APP:CONFLICT`.

    ![]({{ page.assets }}module3_lab2_on_error_configuration.png)

3. Add a Transform component inside the **On Error Propagate**.

    ![]({{ page.assets }}module3_lab2_app_conflict_transform.png)

4. Add the following transformation

    ```
        %dw 2.0
        output application/json
        ---
        {message: error.description}
    ```

    ![]({{ page.assets }}module3_lab2_error_mapping_transformation.png)

    {: .note}
    We are mapping the description message we set in the Raised Error component. If you want to see the complete error object, you can add a logger before the transformation.

    We also need to set the http status code. To do that we are going to add a variable inside the transformation component

5. Click on the <img src="{{ page.assets }}module3_lab2_transform_plus_button.png" width="30px"> inside the transformation component.

    ![]({{ page.assets }}module3_lab2_error_mapping_transformation_plus.png)

    A new window will appear

6. Select `Variable` in the combo and write `httpStatus` as Variable Name.

    ![]({{ page.assets }}module3_lab2_httpStatus.png)

7. Click **OK**.

8. Remove the script and only add `409`.

    ![]({{ page.assets }}module3_lab2_httpStatus_409.png)

    Save all the changes then run the project again and try to create a user twice. You should receive `409` as a response.

## Summary
In this lab, we

- Add Try Scope

- Catch the error

- Log Salesforce response

- Raise a new Error

- Handle a new Error

Go Further:

- See the link [Try Scope](https://docs.mulesoft.com/mule-runtime/latest/try-scope-concept) for more information

- See the link [Raise Error Component](https://docs.mulesoft.com/mule-runtime/latest/raise-error-component-reference) for more module3_lab2_error_mapping_transformation

- See the link [Error Handling](https://docs.mulesoft.com/mule-runtime/latest/error-handling) for more information

**Congratulations! You have completed Lab 4.**