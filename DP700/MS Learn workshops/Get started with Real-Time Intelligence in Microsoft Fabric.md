URL Source: http://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/07-real-time-Intelligence.html

Markdown Content:
Microsoft Fabric provides Real-Time Intelligence, enabling you to create analytical solutions for real-time data streams. In this exercise, you’ll use the Real-Time Intelligence capabilities in Microsoft Fabric to ingest, analyze, and visualize a real-time stream of stock market data.

This lab takes approximately **30** minutes to complete.

> **Note**: You need a [Microsoft Fabric tenant](https://learn.microsoft.com/fabric/get-started/fabric-trial) to complete this exercise.

Create a workspace
------------------

Before working with data in Fabric, you need to create a workspace with the Fabric capacity enabled.

1.   Navigate to the [Microsoft Fabric home page](https://app.fabric.microsoft.com/home?experience=fabric) at `https://app.fabric.microsoft.com/home?experience=fabric` in a browser, and sign in with your Fabric credentials.
2.   In the menu bar on the left, select **Workspaces** (the icon looks similar to 🗇).
3.   Create a new workspace with a name of your choice, selecting a licensing mode that includes Fabric capacity (_Trial_, _Premium_, or _Fabric_).
4.   When your new workspace opens, it should be empty.

[![Image 1: Screenshot of an empty workspace in Fabric.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-workspace.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-workspace.png)

Create an eventstream
---------------------

Now you’re ready to find and ingest real-time data from a streaming source. To do this, you’ll start in the Fabric Real-Time Hub.

> **Tip**: The first time you use the Real-Time Hub, some _getting started_ tips may be displayed. You can close these.

1.   In the menu bar on the left, select the **Real-Time** hub.

The real-time hub provides an easy way to find and manage sources of streaming data.

[![Image 2: Screenshot of the real-time hub in Fabric.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/real-time-hub.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/real-time-hub.png)

2.   In the real-time hub, in the **Connect to** section, select **Data sources**.
3.   Find the **Stock market** sample data source and select **Connect**. Then in the **Connect** wizard, name the source `stock` and edit the default eventstream name to change it to `stock-data`. The default stream associated with this data will automatically be named _stock-data-stream_:

[![Image 3: Screenshot of a new eventstream.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/name-eventstream.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/name-eventstream.png)

4.   Select **Next** and wait for the source and eventstream to be created, then select **Open eventstream**. The eventstream will show the **stock** source and the **stock-data-stream** on the design canvas:

[![Image 4: Screenshot of the eventstream canvas.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-stock-stream.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/new-stock-stream.png)

Create an eventhouse
--------------------

The eventstream ingests the real-time stock data, but doesn’t currently do anything with it. Let’s create an eventhouse where we can store the captured data in a table.

1.   On the menu bar on the left, select **Create**. In the _New_ page, under the _Real-Time Inteligence_ section, select **Eventhouse**. Give it a unique name of your choice.

> **Note**: If the **Create** option is not pinned to the sidebar, you need to select the ellipsis (**…**) option first.

Close any tips or prompts that are displayed until you see your new empty eventhouse.

[![Image 5: Screenshot of a new eventhouse](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/create-eventhouse.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/create-eventhouse.png)

2.   In the pane on the left, note that your eventhouse contains a KQL database with the same name as the eventhouse. You can create tables for your real-time data in this database, or create additional databases as necessary.
3.   Select the database, and note that there is an associated _queryset_. This file contains some sample KQL queries that you can use to get started querying the tables in your database.

However, currently there are no tables to query. Let’s resolve that problem by getting data from the eventstream into a new table.

4.   In the main page of your KQL database, select **Get data**.
5.   For the data source, select **Eventstream**>**Existing eventstream**.
6.   In the **Select or create a destination table** pane, create a new table named `stock`. Then in the **Configure the data source** pane, select your workspace and the **stock-data** eventstream and name the connection `stock-table`.

[![Image 6: Screenshot of configuration for loading a table from an eventstream.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/configure-destination.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/configure-destination.png)

7.   Use the **Next** button to complete the steps to inspect the data and then finish the configuration. Then close the configuration window to see your eventhouse with the stock table.

[![Image 7: Screenshot of and eventhouse with a table.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventhouse-with-table.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventhouse-with-table.png)

The connection between the stream and the table has been created. Let’s verify that in the eventstream.

8.   In the menu bar on the left, select the **Real-Time** hub and then view the **My data streams** page. In the **…** menu for the **stock-data-stream** stream, select **Open eventstream**.

The eventstream now shows a destination for the stream:

[![Image 8: Screenshot an eventstream with a destination.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventstream-destination.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/eventstream-destination.png)

> **Tip**: Select the destination on the design canvas, and if no data preview is shown beneath it, select **Refresh**.

In this exercise, you’ve created a very simple eventstream that captures real-time data and loads it into a table. In a real solution, you’d typically add transformations to aggregate the data over temporal windows (for example, to capture the average price of each stock over five-minute periods).

Now let’s explore how you can query and analyze the captured data.

Query the captured data
-----------------------

The eventstream captures real-time stock market data and loads it into a table in your KQL database. You can query this table to see the captured data.

1.   In the menu bar on the left, select your eventhouse database.
2.   Select the _queryset_ for your database.
3.   In the query pane, modify the first example query as shown here:

code

```
stock
 | take 100
```
4.   Select the query code and run it to see 100 rows of data from the table.

[![Image 9: Screenshot of a KQL query.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-stock-query.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/kql-stock-query.png)

5.   Review the results, then modify the query to retrieve the average price for each stock symbol in the last 5 minutes:

code

```
stock
 | where ["time"] > ago(5m)
 | summarize avgPrice = avg(todecimal(bidPrice)) by symbol
 | project symbol, avgPrice
```
6.   Highlight the modified query and run it to see the results.
7.   Wait a few seconds and run it again, noting that the average prices change as new data is added to the table from the real-time stream.

Create a real-time dashboard
----------------------------

Now that you have a table that is being populated by stream of data, you can use a real-time dashboard to visualize the data.

1.   In the query editor, select the KQL query you used to retrieve the average stock prices for the last five minutes.
2.   On the toolbar, select **Pin to dashboard**. Then pin the query **in a new dashboard** with the following settings: 
    *   **Dashboard name**: `Stock Dashboard`
    *   **Tile name**: `Average Prices`

3.   Create the dashboard and open it. It should look like this:

[![Image 10: Screenshot of a new dashboard.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stock-dashboard-table.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stock-dashboard-table.png)

4.   At the top of the dashboard, switch from **Viewing** mode to **Editing** mode.
5.   Select the **Edit** (_pencil_) icon for the **Average Prices** tile.
6.   In the **Visual formatting** pane, change the **Visual** from _Table_ to _Column chart_:

[![Image 11: Screenshot of a dashboard tile being edited.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/edit-dashboard-tile.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/edit-dashboard-tile.png)

7.   At the top of the dashboard, select **Apply changes** and view your modified dashboard:

[![Image 12: Screenshot of a dashboard with a chart tile.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stock-dashboard-chart.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/stock-dashboard-chart.png)

Now you have a live visualization of your real-time stock data.

Create an alert
---------------

Real-Time Intelligence in Microsoft Fabric includes a technology named _Activator_, which can trigger actions based on real-time events. Let’s use it to alert you when the average stock price increases by a specific amount.

1.   In the dashboard window containing your stock price visualization, in the toolbar, select **Set alert**.
2.   In the **Set alert** pane, create an alert with the following settings: 
    *   **Run query every**: 5 minutes
    *   **Check**: On each event grouped by
    *   **Grouping field**: symbol
    *   **When**: avgPrice
    *   **Condition**: Increases by
    *   **Value**: 100
    *   **Action**: Send me an email
    *   **Save location**: 
        *   **Workspace**: _Your workspace_
        *   **Item**: Create a new item
        *   **New item name**: _A unique name of your choice_

[![Image 13: Screenshot of alert settings.](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/configure-activator.png)](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/Images/configure-activator.png)

3.   Create the alert and wait for it to be saved. Then close the pane confirming it has been created.
4.   In the menu bar on the left, select the page for your workspace (saving any unsaved changes to your dashboard if prompted).
5.   On the workspace page, view the items you have created in this exercise, including the activator for your alert.
6.   Open the activator, and in its page, under the **avgPrice** node, select the unique identifier for your alert. Then view its **History** tab.

Your alert may not have been triggered, in which case the history will contain no data. If the average stock price ever changes by more than 100, the activator will send you an email and the alert will be recorded in the history.

Clean up resources
------------------

In this exercise, you have create an eventhouse, ingested real-time data using an eventstream, queried the ingested data in a KQL database table, created a real-time dashboard to visualize the real-time data, and configured an alert using Activator.

If you’ve finished exploring Real-Time Intelligence in Fabric, you can delete the workspace you created for this exercise.

1.   In the bar on the left, select the icon for your workspace.
2.   In the toolbar, select **Workspace settings**.
3.   In the **General** section, select **Remove this workspace**.