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

In this module you will be using the [Mule AI Chain](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/) connector to allow your API to converse with Azure OpenAI about your customer's orders!

Similarly to what you have already done in Module 2, we will create an API scaffolding using the specification you published to Exchange in the previous step.

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

5. Name the file `mule-chain.json` and click on **Finish**
6. Paste the following content in the newly created file:

    ```json
        {
            "AZURE_OPENAI": {
            "AZURE_OPENAI_KEY": "",
            "AZURE_OPENAI_ENDPOINT": "",
            "AZURE_OPENAI_DEPLOYMENT_NAME": ""
            }
        }
    ```

    For this workshop we'll be leveraging Azure OpenAI as the LLM to generate the natural language response. However, our connector supports multiple LLMs as you can se on the connector [documentation](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/mulesoft-ai-chain-connector-reference) page.
    
    {: .note}
    Your instructor should have provided you with the API Key and Deployment Name needed to connect to the LLM provisioned for this workshop.

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

8. We now need to update the previous file with the host name of the orders API that the connector will invoke to ground the customer's prompt. 

    {: .note}
    The API-Led workshop Anypoint environment already has a fully implemented version of the Order API which, among others, provides an endpoint to get all the orders given a customer ID. This implementation doesn't not connect to any backend and therefore you can pass the API any value as customerId. 

    - Login into Anypoint Platform 
    - Head to Runtime Manager
    - Select the Sandbox environment and select the `mon-order-api` application
    - Copy the **Public Endpoint** and paste it in the `tools.json` file where it says `<CHANGE THIS>`
    
    Check the following animation to see the steps we just described:

    ![]({{ page.assets }}module4-lab1-copy-host-name.gif)

9. Open the `<initials>-ai-agent-api.xml` file and in the Mule Palette click on Search in Exchange
10. Search for **mulesoft ai chain**, click on **Add** once the **MuleSoft AI Chain Connector is displayed** then click on **Finish**

    ![]({{ page.assets }}module4-lab1-import-mule-chain.png)

11. Now scroll down to the  `post:\chat:application\json:nb-ai-agent-api-config` flow and remove the **Logger** connector
12. From the Mule Palette select the **MuleSoft AI Chain** connector and then select the **Tools use ai service** operation

    ![]({{ page.assets }}module4-lab1-select-mulechain-connector.png)

13. Drag the **Tools use ai service** operation into the `post:\chat:application\json:nb-ai-agent-api-config` flow

    ![]({{ page.assets }}module4-lab1-post-flow.png)

    Believe it or not, our agent is pretty much done. The last task is to configure the MuleSoft ai Chain connector.

14. Click on the **Tools use ai service** connector you just added to the flow and in the configuration pane paste this value in the **Tool config** field:

    ```
        mule.home ++ "/apps/" ++ app.name ++ "/tools.json"
    ```

    {: .note }
    Ensure the "Switch to expression mode" button <img src="{{ page.assets }}module4-lab1-switch-mode.png" width="25px"> is enabled. If this is true you will see the value above showing syntax highlighting:
    ![]({{ page.assets }}module4-lab1-tool-config.png)

15. In the **Data** field enter:

    ```
        payload.prompt
    ```
    Again, ensure the "Switch to expression mode" button <img src="{{ page.assets }}module4-lab1-switch-mode.png" width="25px"> is enabled (it should already be the case)

16. Finally click the "**+**" button next to **Module configuration** and select the following values:
    - **LLM Type** : `AZURE_OPENAI`
    - **Config type** : `Configuration Json` (we'll specify the `muleChain.json` file we created earlier)
    - **File path**: `mule.home ++ "/apps/" ++ app.name ++ "/mule-chain.json"` 
        - Make sure the expression language button **Fx** is enabled. You should see the syntax being highlighted as explained earlier
    - **Model name** - `gpt-4o-mini`

    You can leave all the other fields with the pre-defined default values.

    ![]({{ page.assets }}module4-lab1-mule-chain-config.png)

    This is how the connector configuration should look like:

    ![]({{ page.assets }}module4-lab1-mule-chain-tool-config.png)

    And that's it! Your agent is basically done!

## Step 3 - Test your agent locally
We can test our agents directly in Studio before deploying it. We will use the embedded API console to invoke our agent

1. Right click on your application name in Package Explorer and select **Run As -> Mule Application**
    
    ![]({{ page.assets }}module-4-lab-1-run-the-app.png)

2. Once the application is running click on **Open console** in the APIKit Console pane:

    ![]({{ page.assets }}module-4-lab-1-open-console.png)

3. No click on the `POST` method:

    ![]({{ page.assets }}module-4-lab-1-agent-post.png)

4. Click on the Try It button:

    ![]({{ page.assets}}module-4-lab-1-try-it.png)

5. In the Body section, type "**What are my orders? My Client ID is 23423434**" and click on **Send**

    ![]({{ page.assets }}module-4-lab-1-prompt-body.png)

6. If everything goes well you should receive a response that provides a list of orders:

    ![]({{ page.assets }}module-4-lab-1-ai-response.png)

    {: .note }
    The API we are invoking is not actually connected to any backend and therefore you can provide any Customer ID. Also note that we wanted to keep this module simple hence why we need to provide a customer ID. In a real life project we would add other tools to allow the Agent to identify the customer so we wouldn't need to provide an ID which customers wouldn't necessarely know.

## Summary

In this module you've learned about the [MuleChain AI connector](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/) and specifically how to leverage the [Tooling](https://docs.mulesoft.com/mulesoft-ai-chain-connector/latest/configuring-tools-operations) to instruct the agent to autonomously invoke an existing API to ground a customer prompt.

Now let's move to the [Next](./module-4-lab-2) module to deploy and test our agent!