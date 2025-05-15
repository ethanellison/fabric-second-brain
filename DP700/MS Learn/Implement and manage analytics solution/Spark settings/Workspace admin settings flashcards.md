
Title: Workspace administration settings in Microsoft Fabric - Flashcards

Question: What role is required to modify Apache Spark settings in a Fabric workspace?
Answer: The admin role for that workspace is required to modify Apache Spark settings.

Question: What is automatically created when you create a workspace in Microsoft Fabric?
Answer: A starter pool associated with that workspace is automatically created in Microsoft Fabric.

Question: What are the benefits of the simplified setup in Microsoft Fabric workspaces?
Answer: Faster Spark session starts and no need to configure node or machine sizes.

Question: What are the two types of Spark pools available in Microsoft Fabric workspaces?
Answer: Starter Pools and Custom Spark Pools are available in Microsoft Fabric workspaces.

Question: What is a Starter Pool in Microsoft Fabric and what are its characteristics?
Answer: Prehydrated live pools for faster experience, medium size, and default configuration based on Fabric capacity SKU.

Question: What customizations are allowed for Starter Pools by administrators in Microsoft Fabric?
Answer: Admins can customize the max nodes and executors based on Spark workload scale requirements.

Question: What is a Custom Spark Pool in Microsoft Fabric and what are its characteristics?
Answer: Allows sizing nodes, autoscaling, and dynamically allocating executors based on Spark job requirements.

Question: What admin setting enables the creation of Custom Spark Pools in Microsoft Fabric?
Answer: The "Customized workspace pools" option in the Spark Compute section of Capacity Admin settings.

Question: What is a single node cluster in Apache Spark for Microsoft Fabric?
Answer: Driver and executor run on a single node, offering high-availability during node failures.

Question: What are the benefits of enabling autoscaling for custom Spark pools in Fabric?
Answer: Acquires new nodes within the max limit and retires them after job execution for better performance.

Question: What does dynamically allocating executors do for Spark pools in Microsoft Fabric?
Answer: Automatically allocates optimal number of executors based on data volume for better performance.

Question: What compute configurations can workspace admins customize for items in Microsoft Fabric?
Answer: Driver/Executor Core and Driver/Executor Memory for individual items like notebooks and Spark job definitions.

Question: What does the Environment provide for Spark jobs in Microsoft Fabric?
Answer: Flexible configurations, compute properties, runtime selection, and library package dependencies.

Question: What options are available for setting the default environment in a Fabric workspace?
Answer: Select an existing Environment or create a new one through the Environment dropdown.

Question: What is configured when the option to have a default environment is disabled?
Answer: The Fabric runtime version is selected from the available runtime versions.

Question: What do Jobs settings allow admins to control in a Fabric workspace?
Answer: The job admission logic for all the Spark jobs in the workspace.

Question: What is Optimistic Job Admission and how is it enabled in Microsoft Fabric?
Answer: Enabled by default, manages job admissions; can be disabled by reserving maximum cores.

Question: What setting can be customized to control the session expiry for notebook interactive sessions?
Answer: The Spark session timeout can be customized in workspace settings.

Question: What is high concurrency mode in Apache Spark for Fabric data engineering?
Answer: Allows users to share the same Spark sessions across multiple notebooks.

Question: What does enabling autologging do for machine learning models in Microsoft Fabric?
Answer: Automatically captures input parameters, output metrics, and output items during model training.
```