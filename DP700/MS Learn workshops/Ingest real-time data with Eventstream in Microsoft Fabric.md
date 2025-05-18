
URL Source: http://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/09-real-time-analytics-eventstream.html

[![reference arch](https://learn.microsoft.com/en-ca/training/wwl/query-data-kql-database-microsoft-fabric/media/real-time-intelligence-core.png)]

Markdown Content:
Eventstream is a feature in Microsoft Fabric that captures, transforms, and routes real-time events to various destinations. You can add event data sources, destinations, and transformations to the eventstream.

In this exercise, you’ll ingest data from a sample data source that emits a stream of events related to observations of bicycle collection points in a bike-share system in which people can rent bikes within a city.

This lab takes approximately **30** minutes to complete.

> **Note**: You need a [Microsoft Fabric tenant](https://learn.microsoft.com/fabric/get-started/fabric-trial) to complete this exercise.

Create a workspace
------------------

Before working with data in Fabric, you need to create a workspace with the Fabric capacity enabled.

1.   Navigate to the [Microsoft Fabric home page](https://app.fabric.microsoft.com/home?experience=fabric) at `https://app.fabric.microsoft.com/home?experience=fabric` in a browser and sign in with your Fabric credentials.
2.   In the menu bar on the left, select **Workspaces** (the icon looks similar to 🗇).
3.   Create a new workspace with a name of your choice, selecting a licensing mode that includes Fabric capacity (_Trial_, _Premium_, or _Fabric_).
4.   When your new workspace opens, it should be empty.

[![Image 1: Screenshot of an empty workspace in Fabric.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-workspace.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-workspace.png)

Create an eventhouse
--------------------

Now that you have a workspace, you can start creating the Fabric items you’ll need for your real-time intelligence solution. we’ll start by creating an eventhouse.

1.   In the workspace you just created, select **+ New item**. In the _New item_ pane, select **Eventhouse**, giving it a unique name of your choice.
2.   Close any tips or prompts that are displayed until you see your new empty eventhouse.

[![Image 2: Screenshot of a new eventhouse](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/create-eventhouse.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/create-eventhouse.png)

3.   In the pane on the left, note that your eventhouse contains a KQL database with the same name as the eventhouse.
4.   Select the KQL database to view it.

Currently there are no tables in the database. In the rest of this exercise you’ll use an eventstream to load data from a real-time source into a table.

Create an Eventstream
---------------------

1.   In the main page of your KQL database, select **Get data**.
2.   For the data source, select **Eventstream**>**New eventstream**. Name the Eventstream `Bicycle-data`.

The creation of your new event stream in the workspace will be completed in just a few moments. Once established, you will be automatically redirected to the primary editor, ready to begin integrating sources into your event stream.

[![Image 3: Screenshot of a new eventstream.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/empty-eventstream.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/empty-eventstream.png)

Add a source
------------

1.   In the Eventstream canvas, select **Use sample data**.
2.   Name the source `Bicycles`, and select the **Bicycles** sample data.

Your stream will be mapped and you will be automatically displayed on the **eventstream canvas**.

[![Image 4: Review the eventstream canvas](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/real-time-intelligence-eventstream-sourced.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/real-time-intelligence-eventstream-sourced.png)

Add a destination
-----------------

1.   Select the **Transform events or add destination** tile and search for **Eventhouse**.
2.   In the **Eventhouse** pane, configure the following setup options. 
    *   **Data ingestion mode:**: Event processing before ingestion
    *   **Destination name:**`bikes-table`
    *   **Workspace:**_Select the workspace you created at the beginning of this exercise_
    *   **Eventhouse**: _Select your eventhouse_
    *   **KQL database:**_Select your KQL database_
    *   **Destination table:** Create a new table named `bikes`
    *   **Input data format:** JSON

[![Image 5: Eventstream destination settings.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-database-event-processing-before-ingestion.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-database-event-processing-before-ingestion.png)

3.   In the **Eventhouse** pane, select **Save**.
4.   On the toolbar, select **Publish**.
5.   Wait a minute or so for the data destination to become active. Then select the **bikes-table** node in the design canvas and view the **Data preview** pane underneath to see the latest data that has been ingested:

[![Image 6: A destination table in an eventstream.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stream-data-preview.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stream-data-preview.png)

6.   Wait a few minutes and then use the **Refresh** button to refresh the **Data preview** pane. The stream is running perpetually, so new data may have been added to the table.
7.   Beneath the eventstream design canvas, view the **Data insights** tab to see details of the data events that have been captured.

Query captured data
-------------------

The eventstream you have created takes data from the sample source of bicycle data and loads it into the database in your eventhouse. You can analyze the captured data by querying the table in the database.

1.   In the menu bar on the left, select your KQL database.
2.   On the **database** tab, in the toolbar for your KQL database, use the **Refresh** button to refresh the view until you see the **bikes** table under the database. Then select the **bikes** table.

[![Image 7: A table in a KQL database.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-table.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-table.png)

3.   In the **…** menu for the **bikes** table, select **Query table**>**Records ingested in the last 24 hours**.
4.   In the query pane, note that the following query has been generated and run, with the results shown beneath:

code

```
// See the most recent data - records ingested in the last 24 hours.
 bikes
 | where ingestion_time() between (now(-1d) .. now())
```
5.   Select the query code and run it to see 100 rows of data from the table.

[![Image 8: Screenshot of a KQL query.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-query.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-query.png)

Transform event data
--------------------

The data you’ve captured is unaltered from the source. In many scenarios, you may want to transform the data in the event stream before loading it into a destination.

1.   In the menu bar on the left, select the **Bicycle-data** eventstream.
2.   On the toolbar, select **Edit** to edit the eventstream.
3.   In the **Transform events** menu, select **Group by** to add a new **Group by** node to the eventstream.
4.   Drag a connection from the output of the **Bicycle-data** node to the input of the new **Group by** node Then use the _pencil_ icon in the **Group by** node to edit it.

[![Image 9: Add group by to the transformation event.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventstream-add-aggregates.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventstream-add-aggregates.png)

5.   Configure out the properties of the **Group by** settings section: 
    *   **Operation name:** GroupByStreet
    *   **Aggregate type:**_Select_ Sum
    *   **Field:**_select_ No_Bikes. _Then select **Add** to create the function_ SUM of No_Bikes
    *   **Group aggregations by (optional):** Street
    *   **Time window**: Tumbling
    *   **Duration**: 5 seconds
    *   **Offset**: 0 seconds

> **Note**: This configuration will cause the eventstream to calculate the total number of bicycles in each street every 5 seconds.

6.   Save the configuration and return to the eventstream canvas, where an error is indicated (because you need to store the output from the transformation somewhere!).

7.   Use the **+** icon to the right of the **GroupByStreet** node to add a new **Eventhouse** node.
8.   Configure the new eventhouse node with the following options: 
    *   **Data ingestion mode:**: Event processing before ingestion
    *   **Destination name:**`bikes-by-street-table`
    *   **Workspace:**_Select the workspace you created at the beginning of this exercise_
    *   **Eventhouse**: _Select your eventhouse_
    *   **KQL database:**_Select your KQL database_
    *   **Destination table:** Create a new table named `bikes-by-street`
    *   **Input data format:** JSON

[![Image 10: Screenshot of a table for grouped data.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/group-by-table.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/group-by-table.png)

9.   In the **Eventhouse** pane, select **Save**.
10.   On the toolbar, select **Publish**.
11.   Wait a minute or so for the changes to become active.
12.   In the design canvas, select the **bikes-by-street-table** node, and view the **data preview** pane beneath the canvas.

[![Image 11: Screenshot of a table for grouped data.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stream-table-with-windows.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stream-table-with-windows.png)

Note that the trasformed data includes the grouping field you specified (**Street**), the aggregation you specified (**SUM_no_Bikes**), and a timestamp field indicating the end of the 5 second tumbling window in which the event occurred (**Window_End_Time**).

Query the transformed data
--------------------------

Now you can query the bicycle data that has been transformed and loaded into a table by your eventstream

1.   In the menu bar on the left, select your KQL database.
2.       1.   On the **database** tab, in the toolbar for your KQL database, use the **Refresh** button to refresh the view until you see the **bikes-by-street** table under the database.

3.   In the **…** menu for the **bikes-by-street** table, select **Query data**>**Show any 100 records**.
4.   In the query pane, note that the following query is generated and run:

code

```
['bikes-by-street']
 | take 100
```
5.   Modify the KQL query to retrieve the total number of bikes per street within each 5 second window:

code

```
['bikes-by-street']
 | summarize TotalBikes = sum(tolong(SUM_No_Bikes)) by Window_End_Time, Street
 | sort by Window_End_Time desc , Street asc
```
6.   Select the modified query and run it.

The results show the number of bikes observed in each street within each 5 second time period.

[![Image 12: Screenshot of a query returning grouped data.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-group-query.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-group-query.png)

Clean up resources
------------------

In this exercise, you have created an eventhouse and pipulated tables in its database by using an eventstream.

When you’ve finished exploring your KQL database, you can delete the workspace you created for this exercise.

1.   In the bar on the left, select the icon for your workspace.
2.   In the toolbar, select **Workspace settings**.
3.   In the **General** section, select **Remove this workspace**. .