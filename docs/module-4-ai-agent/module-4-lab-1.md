---
parent: Module 4 - AI Agent
title: "Lab 1: Design the AI Agent API"
nav_order: 1
assets: "../../assets/images/module4-ai/lab1/"
               
---
1. TOC
{:toc}

# Lab 1: Design the AI Agent API
{: .no_toc }

## Overview
In this module you will build and deploy a new API that will power a chatbot that we have built for you and you can find it here: [https://tinyurl.com/alc-ai-agent](https://tinyurl.com/alc-ai-agent)
Imagine for a second this is an Agent embedded into your new Mobile Application!

# Step 1 - Design the AI Agent API
In order for our agent UI to work, we need to deploy an API that exposes an endpoint `/chat` and send a payload like the following:
```json
{
    "prompt": "What are my orders?"
}
``` 

1. Create a new API in Deisgn Center as you learned in module 1:

    {: .warning}
    As usual, name your API using your full name initials "<initials> AI Agent API" to avoid opening someone else's specification

    ![]({{ page.assets }}module4-lab1-design-api.gif)
    

2. Once you are in API Designer, copy and paste this RAML API Specification:

    ```yaml
    #%RAML 1.0
    title: AI Agent
    mediaType:
    - application/json
    version: v.10
    protocols:
    - HTTPS
    /chat:
    post:
        body:
        description: Query the customer will ask about your products or their order status.
        properties:
            prompt:
            example: Example
            type: string
        responses:
        "200":
            description: Response returned by the AI Agent
            body:
            description: The response returned by your AI Agent
            properties:
                response:
                example: Example
                type: string
    ```

3. You can now publish the API to Exchange, as shown in the animation above.

# Step 2 - Implement the API

In this module you will be using the [Mule AI Chain](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/) connector to allow your API to converse with OpenAI about your customer's orders!

As you learned in Module 2, we will create an API scaffolding using the specification you published to Exchange in the previous step.

1. In Anypoint Studio select **File --> New --> Mule Project**
2. Call your project `<initials> - AI Agent API`
3. In **Import a published API** select the "**+**" icon, select **from Exchange** and type the name of the API specification you published to Exchange in Step 1

    ![]({{ page.assets }}module4-lab1-select-api.png)

    - click on **Add >** then **Finish**
    - Now you can click on **Finish** to scaffold the API implementation

    ![]({{ page.assets }}module4-lab1-finish-api-creation.png)

    You should now have a new API scaffolding in place.

4. In the Package Explorer right click on **src/main/resources** and select

    ![]({{ page.assets }}module4-lab1-new-file.png)

5. name the file `mule-chain.json` and click on **Finish**
6. Paste the following content in the newly created file:

    ```json
        {
            "OPENAI": {
                "OPENAI_API_KEY": "<PROVIDED BY YOUR INSTRUCTORE>"
            }
        }
    ```

    For this workshop we'll be leveraging OpenAI as the LLM to generate the natural language response. However, our connector supports multiple LLMs as you can se on the connector [documentation](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/mulesoft-ai-chain-connector-reference) page.
    
    {: .note}
    Your instructor will provide you with the OpenAI API Key that has been provisioned specifically for this workshop.

7. To allow your agent to invoke the orders API we will leverage the [Tooling](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/#tools-operations) capability of the Mule AI Chain connector. Tools is the ability of an LLM to invoke an action whenever the LLM is not able to answer the user's question. In our case the only way the LLM can provide details about orders is by invoking an external API which we will configure.
    
    Following the steps above, create a new file and name it tools.json

    Once created paste the following content (which we'll need to tweak a bit):

    ```json
        [
            {
                "action": "Execute GET requests for API endpoints.",
                "url": "<CHANGE THIS>/api/orders?customerId={customerId}",
                "name": "Retrieve orders",
                "method": "GET",
                "example-payload": "{}",
                "query": [
                    "What are my order",
                    "List all the open orders",
                    "Show a list of all my orders",
                    "Show me the list of my orders. My ID is assdf-342a-sdfa"
                ],
                "description": "Retrieve the list of orders for a given customer. If the ustomer ID is not known ask the customer to provide their ID. Once the ID is known replace {customerId} in the URL with the actual customer ID This action applies whenever users' intent is 'Orders overview', 'My orders', 'All orders'."
                }
            ]
    ```

8. We now need to update the previous file with the host name of the orders API. 

    {: .note}
    The API-Led workshop Anypoint environment already has a fully implemented version of the Order API which, among others, provides an endpoint to get all the orders given a customer ID. This implementation doesn't not connect to any backend and therefore you can pass the API any value as customerId. 

    - Login into Anypoint Platform 
    - Head to Runtime Manager
    - Select the Sandbox environment and select the `mon-order-api` application
    - Copy the **Public Endpoint** and paste it in the `tools.json` file where it says `<CHANGE THIS>`
    
    Check the following animation to see the steps we just described:
    

