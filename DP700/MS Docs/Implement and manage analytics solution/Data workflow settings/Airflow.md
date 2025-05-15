Title: Introducing Apache Airflow job in Microsoft Fabric

URL Source: https://blog.fabric.microsoft.com/en-us/blog/introducing-data-workflows-in-microsoft-fabric?ft=All

Published Time: 2024-05-21T08:00:00+00:00

Markdown Content:

We are thrilled to announce the preview of **Apache Airflow job**, a transformative capability within Microsoft Fabric that redefines your approach to constructing and managing data pipelines. Apache Airflow job in Microsoft Fabric is powered by the [Apache Airflow](https://airflow.apache.org/) runtime, and provides an integrated, cloud-based platform for **development**, **scheduling**, and **monitoring** python-based data workflows, articulated as Directed Acyclic Graphs (DAGs). This innovation delivers a Software-as-a-Service (SaaS) experience for data pipeline development and management using Apache Airflow, making Apache Airflow runtime readily accessible for the development and operationalization of your data workflows.

Some key functionalities:
-------------------------

*   **Instant Apache Airflow Runtime Provisioning:** Initiate a new Apache Airflow job and immediately access an Apache Airflow runtime to run/ debug/ operationalize your DAGs. 

Note: Apache Airflow job was earlier referred to as “Data workflows”. 

![Image 1: Instantly provisioned Apache Airflow runtime when you create a new Data workflow.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/word-image-10959-1-2.gif)
*   **Versatile Cloud-Based Authoring (IDE)**: In addition to your existing development tools to craft Apache Airflow DAGs, you can utilize the cloud-based authoring environment provided by Apache Airflow job for a truly cloud-native and SaaS-optimized authoring and debugging experience. 

![Image 2: Screenshot demonstrating the authoring capabilities for DAGs in Data workflows.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/word-image-10959-2-5.gif)
*   **Dynamic Auto-Scaling:**Execute hundreds of Apache Airflow tasks concurrently with our auto-scaling feature, designed to mitigate job queuing and enhance performance.
*   **Intelligent Auto-Pause**: Achieve cost-effectiveness by automatically pausing the Apache Airflow runtime minutes after inactivity in Apache Airflow job, optimizing capacity usage, particularly beneficial during development phases where continuous runtime is unnecessary.
*   **Enhanced Built-in Security**: Integrated within Microsoft Fabric, the Apache Airflow runtime supports Microsoft Entra ID, facilitating Single Sign-On (SSO) experiences when interfacing with Apache Airflow UIs. Additionally, it incorporates Microsoft Fabric workspace roles for robust security measures.
*   **Support for Apache Airflow Plugins and Libraries**: Since Apache Airflow job is powered by Apache Airflow, it supports all features, plugins, and libraries of Apache Airflow, offering comparable extensibility. _**If you’re currently using Workflow Orchestration Manager in Azure Data Factory, you have the option to transition to Fabric.**_ This allows you to execute the same Directed Acyclic Graphs (DAGs) within Apache Airflow job.
*   ****Custom pools for greater flexibility:****When you create a new Apache Airlfow job, the default pool used is a starter pool. This pool is instantly available and optimized to provide a server-free Apache Airflow runtime experience. It also turns off when not in use to save costs, making it perfect for development scenarios. However, if you require more control over the pools, you can create a custom pool. This allows you to specify the size, auto-scale configuration, and more. Setting up your Apache Airflow job for production in this manner enables unattended operation with an always-on Apache Airflow runtime, supporting the Apache Airflow scheduling capabilities. 
Custom pools can be created using the Workspace settings. This approach ensures your workflows are tailored to your specific needs.

![Image 3: Screenshot of workspace settings to configure custom pools.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-649.png)
To get started:
---------------

### Prerequisite for enabling the preview on your Microsoft Fabric tenant:

Enable the Apache Airflow job (earlier known as Data workflows) preview using admin portal or reach out to your Fabric admin.

*   Access the Microsoft Fabric Admin Portal.
*   Navigate to Tenant Settings.
*   Under Microsoft Fabric options, locate and expand the ‘Users can create and use Data workflows (preview)’ section. Note: This action is necessary only during the preview phase of Data workflows. ![Image 4: Screenshot showing the tenant admin portal in Microsoft Fabric using which the preview feature of Data workflows can be turned on for all Fabric users within the tenant.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-38.jpeg)

1.   Create a new Apache Airflow job within an existing or new workspace. 

![Image 5](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/create-apache-airflow-job.png)
2.   Add a new Directed Acyclic Graph (DAG) file via the Apache Airflow job user interface. 

![Image 6: Screenshot showing how to add a new DAG file.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-651.png)
3.   Save your DAG(s). 

![Image 7: Screenshot for saving the doc.](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-652.png)
4.   Debug your DAG interactively using the Apache Airflow job user interface.

![Image 8: Screenshot for running the dag](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-653.png)
Use Apache Airflow monitoring to observe your DAG executions. In the ribbon, click on Monitor your DAGs in Apache Airflow UI. 

![Image 9: A screenshot of a computer
Description automatically generated](https://dataplatformblogwebfd-d3h9cbawf0h8ecgf.b01.azurefd.net/wp-content/uploads/2024/05/a-screenshot-of-a-computer-description-automatica-654.png)

Resources
---------

*   For additional information, please consult the [product documentation](https://aka.ms/fabricairflowintrodoc).
*   If you’re not already using Fabric capacity, consider signing up for the [Microsoft Fabric free trial](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial) to evaluate this feature.