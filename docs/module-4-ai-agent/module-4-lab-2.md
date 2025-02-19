---
parent: Module 4 - AI Agent
title: "Lab 2: Deploy and test your agent"
nav_order: 1
assets: "../../assets/images/module4-ai/lab2/"
               
---

1. TOC
{:toc}

# Lab 2: Deploy and test your agent
{: .no_toc }

# Overview
Now that we have tested our agent within Studio, is time to deploy it to Anypoint Cloudhub so we can plug it into our agent UI.

# Step 1 - Deploy the agent
1. Right click on your AI application in Package Explorer and then select **Anypoint Platform -> Deploy to Cloudhub**

    ![]({{ page.assets }}module-4-lab-2-deploy-agent.png)

2. Select `Micro` in Replica Size and click on Deploy Application

    {: .warning}
    If your deployment fails with the error `The asset is invalid, Error while trying to set type: app. Expected type is: rest-api.` open the `pom.xml` file and change the value of `artifactId` by, for example, adding `-v1.0` to the existing value.

3. Once the deployment is completed, click on **Open n Browser** in the Deployment popup:

    ![]({{ page.assets }}module-4-lab-1-open-browser.png)

    You should already be logged in...

4. If prompted choose the Sandbox environment and select your application

5. Copy your application's endpoint:

    ![]({{ page.assets }}module-4-lab-2-copy-endpoint.png)

6. Now open the [Agent UI application](https://tinyurl.com/alc-ai-agent)

7. Paste your public endpoint in the **Agent UR** text box that you will find at the top of the page

    ![]({{ page.assets }}module-4-lab-2-url-text-box.png)
    
    Ensure that the URL includes the `/api/chat` path. In our example this would be 
    
    ```
        https://nb-ai-agent-api-1s3mvo.5sc6y6-4.usa-e2.cloudhub.io/api/chat
    ```

8. Test your agent by asking "**What are my orders? My ID is 34343**"

    ![]({{ page.assets }}module-4-lab-1-agent-response.png)

    {: .note }
    You might get a slightly different output but that's to be expected given the non-deterministic nature of Generative AI. The most important thing to note is that the response should provide a list of orders!

Congratulations!
{: .fs-9}

You just built your first AI agent grounded in your business data!

# Conclusions

In this module you have learned how easy it is to create AI agent that don't just provide generic answers but are capable of answering questions grounded in your business data. The agent you built in this module only scratched the surface of what can be achieved by leveraging the MuleSoft AI Chain together with all other MuleChain open source connectors that can be found at [https://mac-project.ai/](https://mac-project.ai/)

