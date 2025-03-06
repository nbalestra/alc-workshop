---
parent: Module 4 - Uniquitous Connectivity - Database
title: "Lab 4: (Optional) Error Handling"
nav_order: 4
assets: "../../assets/images/module4/lab4/"
---
1. TOC
{:toc}

# Lab 4: (Optional) Error Handling
{: .no_toc }

## Overview

In Lab 2, you implemented the API. Maybe it happened you’ve got a 500 error trying to creating a product with a repeated Unique ID. The error message does not describe any particular error. In this lab you are going to learn how to Handle errors.

## Step 1: Add Try Scope
The Try scope enables you to handle errors that may occur when attempting to execute any of the components inside the Try scope.

In this case, the **Product Number** in a product, is unique. So when we do an insert we could get an error. We are going to throw a `409` status code with the details of the error. Head to the `post:\product` flow.

1. Go to the palette and search for the **Try** component.

2. Insert the Try component component after the Set Variable component

3. Move the insert component inside the Try component. You should see something similar to this

    ![]({{ page.assets }}lab2_try_flow.png)

## Step 2: Catch the Error
When you handle the error you have two options.

- **On Error Continue**: This will catch the error and continue with the flow execution.

- **On Error Propagate**: This option will catch the error, but will throw it up.

You could eventually use one or the other depending on the error type.

In this case we are going to propagate the error.

To do that we are going to add an **On Error Propagate** handler to the Error handling part.

1. Go to the palette and search for **On Error Propagate**. Drag and Drop the component inside the Error handling part.

    ![]({{ page.assets }}lab2_propagate.png)

    We only want to catch the **Query Execution** error, so that’s what we are going to configure.

2. Click on the **Error Propagate** component and in **Type** write `DB:QUERY_EXECUTION`. You can also select from the drop down list.

    ![]({{ page.assets }}lab2_propagate_config.png)

## Step 3: Raise a new Error
In this step, we are going to raise a new exception with the `DB:QUERY_EXECUTION` message.

1. Drag and drop **Raise Error** component inside the **On Error Propagate** component.

    ![]({{ page.assets }}lab2_raise_error.png)

2. Configure with the following values

    | Name	      | Value                  |
    |:------------|:-----------------------|
    | Type        | `APP:CONFLICT`         |
    | description | `#[error.description]` |

    ![]({{ page.assets }}lab2_raise_error_config.png)

## Step 4: Handle a new Error
Now we need to go to the APIKit Error Handling and add a new **On Error Propagate** Handler to return `409`.

1. Go to the **Error Handling** in the `api-main` flow and add a new **On Error Propagate Handler**

2. Click inside and configure `APP:CONFLICT` in **Type**.

    ![]({{ page.assets }}lab2_api_kit_propagate.png)

3. Add a **Transformation** Component inside

    We are going to return a payload and a httpStatus variable with the error code.

4. In the payload configuration we are going to return the error description, so copy and paste the following script:

    ```
    %dw 2.0
    output application/json
    ---
    {message: error.description}
    ```

    ![]({{ page.assets }}lab2_error_desc_transform.png)

    We also are going to return a http status code. To do that we need to create a variable

5. Click <img src="{{ page.assets }}lab2_add_target.png" width="25px"> button. A new window will appear.

6. Select **Variable** from the drop down list and set `httpStatus` as the Variable Name.

    ![]({{ page.assets }}lab2_add_variable_menu.png)

7. Remove the existing content and simply write `409` in the script box.

    ![]({{ page.assets }}lab2_add_variable_output.png)

    At the end you will have something similar to this:

    ![]({{ page.assets }}lab2_api_kit_conflict_handlerpng.png)

## Step 5: Test the error
1. Run the API, and validate you receive a `409` by calling `POST` on "products" with an existing product number.

    You should see something like this:  

    ![]({{ page.assets }}lab2_409_console.png)

    Try to return a 409 in the update flow, when a product doesn’t exist.

    {: .note-title }
    > TIP
    >
    > Enclose the **Update Product** component with a **Try Scope** and Raise an Exception as we did before.

## Summary

In this module we have seen the following:

- [Overview](#overview)

- [Step 1: Add Try Scope](#step-1-add-try-scope)

- [Step 2: Catch the Error](#step-2-catch-the-error)

- [Step 3: Raise a new Error](#step-3-raise-a-new-error)

- [Step 4: Handle a new Error](#step-4-handle-a-new-error)

- [Step 5: Test the error](#step-5-test-the-error)

Go Further:

- See the link [Try Scope](https://docs.mulesoft.com/mule-runtime/latest/try-scope-concept) for more information

- See the link [Raise Error Component](https://docs.mulesoft.com/mule-runtime/latest/raise-error-component-reference) for more

- See the link [Error Handling](https://docs.mulesoft.com/mule-runtime/latest/error-handling) for more information

**Congratulations! You have completed Lab 4.**