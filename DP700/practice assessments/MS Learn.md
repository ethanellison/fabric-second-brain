
Your organization uses Microsoft Fabric to manage data across departments. You need to ensure each department can manage its data governance settings independently. What should you do? Your company uses Microsoft Fabric to manage data engineering workloads. The team has performance issues with Spark jobs due to inefficient resource allocation.

You need to optimize the Spark pool configuration to improve performance and reduce costs.
Which three actions should you take? Each correct answer presents part of the solution.
Configure dynamic allocation for executors.
Enable autoscale for node provisioning.
Enable fixed resource allocation.

Select memory-optimized nodes.

Use compute-optimized nodes.

Use static node allocation.

Configuring dynamic allocation for executors optimizes resource usage by adjusting executor processes according to data volume, enhancing performance and efficiency.

Enabling autoscale for node provisioning allows the Spark pool to dynamically adjust the number of nodes based on workload demands, optimizing resource usage and cost.

Selecting memory-optimized nodes provides better performance for data-intensive operations, which is critical for improving Spark job efficiency. Enabling fixed resource allocation does not address performance issues related to dynamic resource needs and could lead to inefficient resource usage. Using static node allocation lacks flexibility in resource allocation, necessary for handling varying workloads efficiently. Using compute-optimized nodes may seem beneficial for performance but are not ideal for data-intensive Spark jobs compared to memory-optimized nodes.

https://learn.microsoft.com/en-us/training/modules/use-apache-spark-work-files-

lakehouse/2-spark

https://learn.microsoft.com/en-us/credentials/certifications/fabric-data-engineer-associate/practice/results