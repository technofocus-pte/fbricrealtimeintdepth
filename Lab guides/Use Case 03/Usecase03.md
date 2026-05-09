# Use case 3: Ingesting data using Eventstream

**Introduction**

In this lab, you will start by rapidly generating real-time data and
understanding how that data can be processed and visualized in Microsoft
Fabric. With the initial reporting in place, multiple modules are
available that explore data warehousing, data lakehouse architecture,
data activator, data science, and of course, real-time analytics. The
modules are designed to be cohesive but flexible -- they all involve the
same core scenario but have limited dependencies so you can consume the
modules that make the most sense for you.

The basic architecture of the solution is illustrated below. The app
deployed in the beginning of this lab (either as a docker container or
running in Jupyter notebook) will publish events to our Fabric
environment. The data is ingested into a KQL database for real-time
reporting in Power BI.

In this lab, you’ll get hands-on with a fictitious financial company
"AbboCost." AbboCost would like to set up a stock monitoring platform to
monitor price fluctuations and report on historical data. Throughout the
workshop, we'll look at how every aspect of Microsoft Fabric can be
incorporated as part of a larger solution -- by having everything in an
integrated solution, you'll be able to quickly and securely integrate
data, build reports, create data warehouses and lakehouses, forecast
using ML models, and more.

Now that our data is streaming into our KQL database, we can begin to
query and explore the data, leveraging KQL to gain insights into the
data. A KQL queryset is used to run queries, view, and transform data
from a KQL database. Like other artifacts, a KQL queryset exists within
the context of a workspace. A queryset can contain multiple queries each
stored in a tab. In this exercise, we'll create several KQL queries of
increasing complexity to support different business uses.

As a refresher on the scenario, AbboCost Financial has been modernizing
their stock market reporting for their financial analysts. In previous
modules, we've developed the solution by creating real-time dashboards,
a data warehouse, and more.

In this module, AbboCost would like to explore predictive analytics to
help inform their advisors. The goal is to analyze historical data for
patterns that can be used to create forecasts of future values.
Microsoft Fabric's Data Science capability is the ideal place to do this
kind of exploration.

Most of this lab will be done within Jupyter notebooks, an industry
standard way of doing exploratory data analysis, building models,
visualizing datasets, and processing data.

![Data Lakehouse with Azure Synapse Analytics](./media/image1.png)

# Objectives

- To sign up for the free Microsoft Fabric trial, redeem Azure Pass, and
  configure necessary permissions within the Azure portal.

- To create fabric capacity and workspace, storage account, and fabric
  workspace.

- To deploy the stock generator app via Azure Container Instance using
  an ARM template.

- To configure Eventstream in Microsoft Fabric for ingesting real-time
  data from Azure Event Hubs, ensuring seamless integration and data
  preview for subsequent analysis.

- To create a KQL database within Microsoft Fabric and send data from
  Eventstream to the KQL database.

- To explore stock price data using KQL, progressively developing
  queries to analyze trends, calculate price differentials, and
  visualize data for actionable insights.

- To leverage Power BI to create dynamic, real-time reports based on
  analyzed stock data, configuring auto-refresh settings for timely
  updates and enhancing visualization for informed decision-making.

- To create a Lakehouse named "StocksLakehouse" in the Data Engineering
  persona of Synapse Data Engineering Home, confirming successful SQL
  endpoint creation.

- To add the Lakehouse to the StockEventStream, configure it, perform
  data cleanup, and ensure the Lakehouse receives required data fields.

- To import notebooks "Lakehouse 1-4" into RealTimeWorkspace, then
  execute them individually.

- To build curated and aggregated data tables suitable for building a
  dimensional model and supporting data science activities.

- To build a dimensional model within the Lakehouse environment,
  incorporating fact and dimension tables and creating a semantic model
  based on the dimensional model.

- To import and configure notebooks for building and storing machine
  learning models.

- To explore and execute the DS 1-Build Model notebook for building and
  validating a stock prediction model.

- To examine model metadata and performance metrics in the
  RealTimeWorkspace.

- To open and explore the DS 2-Predict Stock Prices notebook for
  predicting stock prices.

- To run the notebook for generating predictions and storing them in the
  Lakehouse.

- To import and explore the DS 3-Forecast All notebook for generating
  predictions and storing them in the Lakehouse.

- To execute the notebook and verify the predictions in the Lakehouse
  schema.

- To build a semantic model in Power BI using the
  StockLakehousePredictions data and create visualizations in Power BI
  to analyze stock predictions and market trends.

- To configure relationships between tables and publish the prediction
  report.

# Exercise 1: Environment Setup

To follow the lab exercises, a set of resources must be provisioned. At
the heart of the scenario is the real-time stock price generator script
that generates a continuous stream of stock prices that are used
throughout the workshop.

We recommend deploying the stock price generator via Azure Container
Instance because the default Spark cluster will consume a large number
of resources.

## **Task 1: Create a Fabric workspace**

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: https://app.fabric.microsoft.com/ then press the
    **Enter** button.
    |  |  |
    | --- | --- |
    | Username | **+++@lab.CloudPortalCredential(User1).Username+++** |
    | Password | **+++@lab.CloudPortalCredential(User1).Password+++** |

3.  In the **Home** page click on **+ New Workspaces** as shown in the
    below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image2.png)

3.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

    |  |  |
    | --- | --- |
    | Name | +++RealTimeWorkspace@lab.LabInstance.Id+++ (must be a unique number) |
    | Advanced | Under License mode, select Fabric capacity |
    | Default | storage format Small dataset storage format |

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image5.png)

## Task 2: Get data with Eventstream

1.  Create a new Eventhouse by clicking on the **+New item** button in
    the navigation bar. select **Eventstream**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/img1.png)

2.  Name the Eventstream +++**StockEventStream**+++, click on
    the **Create** button.

![](./media/img2.png)

3.  On the Screen **Design a flow to ingest, transform, and route
    streaming events** click on **Use custom Endpoint**. This will
    create an event hub connected to the Eventstream.

> ![](./media/img3.png)

4.  Click on **Add**.

> ![](./media/img4.png)

5.  Click on the **Publish** button.

> ![](./media/img5.png)
>
> ![](./media/img6.png)

7.  On the **Eventstream** pane, select the **keys** under
    the **Details**, select **SAS key Authentication ,** copy
    the **Event hub name**, **connection strings-primarykey** and paste
    them on a notepad, as you need them in the upcoming task

> ![](./media/img7.png)

8.  Now, click on **RealTimeWorkspaceXXX** on the left-sided navigation
    pane.

> ![](./media/img8.png)

9.  On the **RealTimeWorkspace** page, from the menu bar, navigate and
    click on **-\>|Import** button, then select **Notebook** and
    select **From this computer** as shown in the below image.

> ![](./media/image9.png)

10. Select **Upload** from the **Import status** pane that appears on
    the right side of the screen.

> ![](./media/img10.png)

11. Navigate and select S**tockGeneratorNotebook** notebooks
    from **C:\LabFiles**and click on the **Open** button.

> ![](./media/img11.png)

4.  You will see a notification stating **Imported successfully.**

5.  Then, select the S**tockGeneratorNotebook** notebook.

> ![](./media/img12.png)

6.  In the notebook, paste the connection string from above in the
    single quotes of the **ConnectionString **variable of the first
    cell. It should look similar to the below image:

> ![](./media/img13.png)

7.  With the connection string in place, click *Run All* in the top
    toolbar. The first few cells may take a few moments to start the
    Spark session and import the required libraries.

> ![](./media/img14.png)

8.  Skip running the 9^(th) cell, which is the final cell in the
    notebook.

> ![](./media/img15.png)

9.  Switch back to the Eventstream to observe a preview of the events
    generated by the notebook:

> ![](./media/img16.png)
>
> ![](./media/img17.png)
>
> ![](./media/img18.png)

# Exercise 2: KQL Database Configuration and Ingestion

Now that our environment is fully configured, we will complete the
ingestion of the Eventstream, so that the data is ingested into a KQL
database. This data will also be stored in Fabric OneLake.

## Task 1: Create KQL Database

Kusto Query Language (KQL) is the query language used by Real-Time
analytics in Microsoft Fabric along with several other solutions, like
Azure Data Explorer, Log Analytics, Microsoft 365 Defender, etc. Similar
to Structured Query Language (SQL), KQL is optimized for ad-hoc queries
over big data, time series data, and data transformation.

To work with the data, we'll create a KQL database and stream data from
the Eventstream into the KQL DB.

1.  In the left-sided navigation menu, navigate and click on **RealTime
    workspaceXXX**, as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image29.png)

2.  In the **Real-Time Intelligence** page, navigate to +**New item**
    section and select **Eventhouse** to create Eventhouse.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

3.  In the **New Eventhouse** dialog box, enter +++**StockDB+++**in
    the **Name** field, click on the **Create** button and open the new
    Eventhouse.

> ![](./media/image31.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  From the **System overview** page, select the **StockDB** database
    you created in the previous task.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image33.png)

5.  Select **StockDB**, click on the **OneLake availability** as shown
    in the below image to change the setting and, then click on the
    **Turn on** Toggle the button enable OneLake access.

> ![](./media/image34.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

## Task 2: Send data from the Eventstream to the KQL database

1.  In the left-sided navigation menu, navigate and click on
    **StockEventStream** created in the previous task, as shown in the
    below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

2.  On the Eventstream, click on the **Edit** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image37.png)

3.  Our data should be arriving into our Eventstream, and we'll now
    configure the data to be ingested into the KQL database we created
    in the above task. On the Eventstream, click on **Transform events
    or add destination***,* then navigate and click on **Eventhouse**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image38.png)

4.  On the KQL settings, select **Direct ingestion**. While we have the
    opportunity to process event data at this stage, for our purposes,
    we will ingest the data directly into the KQL database. Set the
    destination name to +++***KQL+++***, then select your **workspace,
    Eventhouse** and KQL database created in the above task, then click
    on **Save** button.

> ![](./media/image39.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image40.png)

5.  On the Eventstream, select **Publish.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image41.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image42.png)

6.  On the Eventstream pane, select **configure** in the **KQL**
    destination.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image43.png)

7.  On the first settings page, select **+New table** and enter the
    name +++***StockPrice+++*** for the table to hold the data in
    StockDB. Click on the **Next** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image44.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

8.  The next page allows us to inspect and configure the schema. Be sure
    to change the format from TXT to **JSON**, if necessary. The default
    columns of *symbol*, *price*, and *timestamp* should be formatted as
    shown in the below image; then click on the **Finish** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image46.png)

9.  On the **Summary** page, if there are no errors, you’ll see a
    **green checkmark** as shown in the below image, then click on the
    **Close** button to complete the configuration.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image47.png)

10. Select the **KQL** destination and click on the **Refresh** button

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image48.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image49.png)

# Exercise 3: Exploring the Data

In this exercise, you'll create several KQL queries of increasing
complexity to support different business uses.

## Task 1: Create KQL queryset: StockQueryset

1.  Click on **RealTimeWorkspace** on the left-sided navigation pane.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

2.  From your workspace, click on ***+** **New item \> KQL Queryset** as
    shown in the below image. In the **New KQL Queryset** dialog box,*
    enter +++**StockQueryset**+++, then click on the **Create**
    button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image51.png)
>
> ![A screenshot of a login box AI-generated content may be
> incorrect.](./media/image52.png)

3.  In StockQueryset page select **Eventhouse/KQL Database**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image53.png)

4.  Select the ***StockDB*** and click on the **Connect** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image54.png)

5.  The KQL query window will open, allowing you to query the data.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

6.  The default query code will look like the code shown in the below
    image; it contains 3 distinct KQL queries. You may
    see *YOUR_TABLE_HERE* instead of the ***StockPrice*** table. Select
    and delete them.

> ![](./media/image56.png)

7.  In the query editor, copy and paste the following code. Select the
    entire text and click on *the **Run*** button to execute the query.
    After the query is executed, you will see the results.

    ```
    // Use "take" to view a sample number of records in the table and check the data.
    StockPrice
    | take 100;
    
    // See how many records are in the table.
    StockPrice
    | count;
    
    // This query returns the number of ingestions per hour in the given table.
    StockPrice
    | summarize IngestionCount = count() by bin(ingestion_time(), 1h);
    ```

***Note:** To run a single query when there are multiple queries in the
editor, you can highlight the query text or place your cursor so the
cursor is in the context of the query (for example, at the beginning or
end of the query) -- the current query should highlight in blue. To run
the query, click Run in the toolbar. If you'd like to run all 3 to
display the results in 3 different tables, each query will need to have
a semicolon (;) after the statement, as shown below.*

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image57.png)

8.  The results will be displayed in 3 different tables as shown in the
    below image. Click on each table tab to review the data.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image58.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image59.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image60.png)

## **Task 2: New Query of StockByTime**

1.  Create a new tab within the queryset by clicking on the ***+* icon**
    as shown in the below image. Rename this tab as
    +++***StockByTime***+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image61.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image62.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image63.png)

2.  We can begin to add our own calculations, such as calculating the
    change over time. For example,
    the [prev()](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/prevfunction) function,
    a type of windowing function, allows us to look at values from
    previous rows; we can use this to calculate the change in price. In
    addition, as the previous price values are stock symbol specific, we
    can [partition](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/partition-operator) the
    data when making calculations.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

    ```
    StockPrice
    | where timestamp > ago(75m)
    | project symbol, price, timestamp
    | partition by symbol
    (
        order by timestamp asc
        | extend prev_price = prev(price, 1)
        | extend prev_price_10min = prev(price, 600)
    )
    | where timestamp > ago(60m)
    | order by timestamp asc, symbol asc
    | extend pricedifference_10min = round(price - prev_price_10min, 2)
    | extend percentdifference_10min = round(round(price - prev_price_10min, 2) / prev_price_10min, 4)
    | order by timestamp asc, symbol asc
    ```
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image64.png)

4.  In this KQL query, the results are first limited to the most recent
    75 minutes. While we ultimately limit the rows to the last 60
    minutes, our initial dataset needs enough data to lookup previous
    values. The data is then partitioned to group the data by symbol,
    and we look at the previous price (from 1 second ago) as well as the
    previous price from 10 minutes ago. Note that this query assumes
    data is generated at 1 second intervals. For the purposes of our
    data, subtle fluctuations are acceptable. However, if you need
    precision in these calculations (such as exactly 10 minutes ago and
    not 9:59 or 10:01), you'd need to approach this differently.

## Task 3: StockAggregate

1.  Create another new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    **+++*StockAggregate*+++**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image65.png)
>
> ![](./media/image66.png)

2.  This query will find the biggest price gains over a 10-minute period
    for each stock, and the time it occurred. This query uses
    the [summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator) operator,
    which produces a table that aggregates the input table into groups
    based on the specified parameters (in this case, *symbol*),
    while [arg_max](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/arg-max-aggregation-function) returns
    the greatest value.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

    ```
    StockPrice
    | project symbol, price, timestamp
    | partition by symbol
    (
        order by timestamp asc
        | extend prev_price = prev(price, 1)
        | extend prev_price_10min = prev(price, 600)
    )
    | order by timestamp asc, symbol asc
    | extend pricedifference_10min = round(price - prev_price_10min, 2)
    | extend percentdifference_10min = round(round(price - prev_price_10min, 2) / prev_price_10min, 4)
    | order by timestamp asc, symbol asc
    | summarize arg_max(pricedifference_10min, *) by symbol
    ```
 
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image67.png)

## Task 4: StockBinned

1.  Create another new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    +++**StockBinned*+++***

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image68.png)
>
> ![](./media/image69.png)

2.  KQL also has a [bin()
    function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/bin-function),
    which can be used to bucket results based on the bin parameter. In
    this case, by specifying a timestamp of 1 hour, the result is
    aggregated for each hour. The time period can be set to minute,
    hour, day, and so on.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

    ```
    StockPrice
    | summarize avg(price), min(price), max(price) by bin(timestamp, 1h), symbol
    | sort by timestamp asc, symbol asc
    ```
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image70.png)

4.  This is particularly useful when creating reports that aggregate
    real-time data over a longer time period.

## Task 5: Visualizations

1.  Create a final new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    +++**Visualizations*+++*.** We'll use this tab to explore
    visualizing data.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image71.png)
>
> ![](./media/image72.png)

2.  KQL supports a large number of visualizations by using
    the *render* operator. Run the below query, which is the same as the
    StockByTime query, but with an additional *render* operation added:

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

    ```
    StockPrice
    | where timestamp > ago(75m)
    | project symbol, price, timestamp
    | partition by symbol
    (
        order by timestamp asc
        | extend prev_price = prev(price, 1)
        | extend prev_price_10min = prev(price, 600)
    )
    | where timestamp > ago(60m)
    | order by timestamp asc, symbol asc
    | extend pricedifference_10min = round(price - prev_price_10min, 2)
    | extend percentdifference_10min = round(round(price - prev_price_10min, 2) / prev_price_10min, 4)
    | order by timestamp asc, symbol asc
    | render linechart with (series=symbol, xcolumn=timestamp, ycolumns=price)
    ```
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image73.png)

4.  This will render a line chart as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image74.png)

# Exercise 4: Optimizing Power BI Reporting Efficiency

With the data loaded in the database and our initial KQL queryset
complete, we can begin to craft visualizations for real-time dashboards.

## Task 1: Configuring Refresh Rate

Our Power BI tenant needs to be configured to allow for more frequent
updating.

1.  To configure this setting, navigate to the Power BI admin portal by
    clicking on the ***Settings* **icon in the upper right corner of the
    **Fabric portal**. Navigate to Governance and insights section, then
    click on **Admin portal**.

> ![](./media/image75.png)

2.  Select ***Capacity settings*** on the left, and select the **Fabric
    Capacity** that matches your current environment. Select **Fabric
    capacity** name

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image76.png)

3.  On the following screen, scroll down to the ***Power BI
    workloads*** section, and under ***Semantic Models*** (recently
    renamed from *Datasets*), configure ***Automatic page
    refresh*** to ***On***, with a **minimum refresh interval** of **1**
    second. Click ***Apply*.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image77.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image78.png)

4.  In caseUpdate your capacity workloads dialog box appears, then click
    on the **Yes** button.

> ![A white background with black text Description automatically
> generated](./media/image79.png)

## Task 2: Creating a basic Power BI report

1.  In the **Microsoft Fabric** page menu bar on the top, select
    **StockQueryset**.

![A screenshot of a computer message AI-generated content may be
incorrect.](./media/image80.png)

2.  From the ***StockQueryset*** queryset used in the previous module,
    select the ***StockByTime*** query tab.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image81.png)

3.  Select the query and run to view the results. Click** **on *the
    **Create Power BI report*** button in the command bar to bring this
    query into Power BI.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image82.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image83.png)

4.  On the report preview page, we can configure our initial chart,
    select a **line chart** to the design surface, and configure the
    report as follows. See the image below as a reference.

- Legend: **symbol**

- X-axis: **timestamp**

- Y-axis: **price**

![](./media/image84.png)

5.  In the Power BI (preview) page, from the ribbon, click on
    **File** and select **Save**.

> ![A screenshot of a graph Description automatically
> generated](./media/image85.png)

6.  On **Just a few details first** dialog box, in **Name your file in
    Power BI** field, enter +++**RealTimeStocks***+++*. In **Save it to
    a workspace** field, click on the dropdown and select
    ***RealTimeWorkspace***. Then, click on the **Continue** button**.**

> ![A screenshot of a computer Description automatically
> generated](./media/image86.png)

7.  In the Power BI (preview) page, click on **Open the file in Power BI
    to view, edit and get a shareable link.**

![](./media/image87.png)

8.  On the **RealTimeStock** page, click on the **Edit** button in the
    command bar to open the report editor.

> ![A graph of different colored lines Description automatically
> generated](./media/image88.png)

9.  Select the line chart on the report. Configure a **Filter**
    for ***timestamp*** to display data for the last 5 minutes using
    these settings:

- Filter type: Relative time

- Show items when the value: is in the last 5 minutes

> Click on ***Apply filter*** to enable the filter. You will see a
> similar type of output as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image89.png)

## Task 3: Creating a second visual for percent change

1.  Create a second line chart, under **Visualizations**, select **Line
    chart**.

2.  Instead of plotting the current stock price, select
    the ***percentdifference_10min*** value, which is a positive or
    negative value based off the difference between the current price
    and the value of the price from 10 minutes ago. Use these values for
    the chart:

- Legend: **symbol**

- X-axis: **timestamp**

- Y-axis: **average of percentdifference_10min**

![](./media/image90.png)

![A screenshot of a computer Description automatically
generated](./media/image91.png)

3.  Under the **Visualization,** select the **Analytics** represented by
    a magnifier-like icon as shown in the below image, then click on
    **Y-Axis Constant Line(1).** In the **Apply settings to**
    section**,** click on **+Add line,** then enter **Value 0.**

![](./media/image92.png)

4.  Select the line chart on the report. Configure a **Filter**
    for ***timestamp*** to display data for the last 5 minutes using
    these settings:

- Filter type: Relative time

- Show items when the value: is in the last 5 minutes

![](./media/image93.png)

## Task 4: Configuring the report to auto-refresh

1.  Deselect the chart. On the ***Visualizations* settings**,
    enable ***Page refresh*** to automatically refresh every second or
    two, based on your preference. Of course, realistically we need to
    balance the performance implications of refresh frequency, user
    demand, and system resources.

2.  Click on **Format your report** **page** icon, navigate and click on
    **Page refresh**. Turn on the toggle. Set the Auto page refresh
    value as **2 Seconds** as shown in the below image.

![](./media/image94.png)

3.  In the Power BI (preview) page, from the ribbon, click on
    **File** and select **Save**.

![](./media/image95.png)

# Exercise 5: Setting up the Lakehouse

## Task 1: Create the Lakehouse

Start by creating a Lakehouse.

***Note**: If you are completing this lab after the data science module
or another module that uses a Lakehouse, you may re-use that Lakehouse
or create a new one, but we'll assume the same Lakehouse is shared
across all modules.*

1.  Fabric home page, select **+New item** tile and click on
    **Lakehouse**tile.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image96.png)

2.  In the **New lakehouse** dialog box, enter +++**StocksLakehouse**+++
    in the **Name** field, then click on the **Create** button. A
    **StocksLakehouse** page will appear.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image97.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image98.png)

3.  You will see a notification stating - **Successfully created SQL
    endpoint**.

> **Note**: In case, you did not see the Notifications, then wait for
> few minutes.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image99.png)

## **Task 2. Add Lakehouse to the Eventstream**

From an architecture perspective, we'll implement a [Lambda
architecture](https://en.wikipedia.org/wiki/Lambda_architecture) by
splitting hot path and cold path data from the Eventstream. Hot path
will continue to the KQL database as already configured, and cold path
will be added to write the raw data to our lakehouse. Our data flow will
resemble the following:

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    pane and select **StockEventStream** as shown in the below image.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image100.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image101.png)

2.  In addition to adding Lakehouse to the Eventstream, we'll do some
    cleanup of the data using some of the functions available in the
    Eventstream.

3.  On the **StockEventStream** page, select **Edit**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image102.png)

4.  On the **StockEventStream** page, click on the **Add destination**
    on the output of the Eventstream to add a new destination.
    Select ***Lakehouse* **from the context menu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image103.png)

5.  In the Lakehouse pane that appears on the right side, enter the
    following details and click on **Save.**

    |  |  |
    | --- | --- |
    | Destination name | +++Lakehouse+++ |
    | Workspace | RealTimeWorkspace@lab.LabInstance.Id |
    | Lakehouse | StockLakehouse |
    | Delta table | Click on Create new> enter +++raw_stock_data+++ |
    | Input data format | Json |

> ![A screenshot of a computer Description automatically
> generated](./media/image104.png)

![A screenshot of a computer Description automatically
generated](./media/image105.png)

6.  Connect **StockEventStream** and **Lakehouse**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image106.png)
>
> ![](./media/image107.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image108.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image109.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image110.png)

7.  Select the Lakehouse and click on **Refresh** button

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image111.png)

8.  After clicking *Open event processor*, various processing can be
    added that perform aggregations, filtering, and changing datatypes.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image112.png)

9.  On the **StockEventStream** page, select **Edit**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image113.png)

9.  On the **StockEventStream** page, select **stockEventStream**, and
    click the **Insert a node** icon to add a **Mange field**. Then,
    select **Mange field.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image114.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image115.png)

10. In the eventstreem pane select **Managefields** pencil icon.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image116.png)

11. In the ***Manage fields*** pane that opens, click ***Add all
    fields*** to add all columns. Then, remove the fields
    **EventProcessedUtcTime**, **PartitionId**, and
    **EventEnqueuedUtcTime** by clicking the **ellipsis (...)** to the
    right of the field name, and click ***Remove***

![A screenshot of a computer Description automatically
generated](./media/image117.png)

![A screenshot of a computer Description automatically
generated](./media/image118.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image119.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image120.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image121.png)

12. Now change the ***timestamp*** column to a ***DateTime* **as it is
    likely classified as a string. Click the **three ellipsis (...)** to
    the right of the ***timestamp* column** and select ***Yes** change
    type*. This will allow us to change the datatype:
    select ***DateTime*,** as shown in the below image. Click on
    **Done**

> ![A screenshot of a computer Description automatically
> generated](./media/image122.png)
>
> ![](./media/image123.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image124.png)

12. Now, click on the **Publish** button to close the event processor

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image125.png)

13. Once it is completed, the Lakehouse will receive the symbol, price,
    and timestamp.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image126.png)

Our KQL (hot path) and Lakehouse (cold path) is now configured. It may
take a minute or two for data to be visible in the Lakehouse.

![](./media/image127.png)

## Task 3. Import notebooks

**Note**: If you have issues importing these notebooks, be sure you are
downloading the raw notebook file and not the HTML page from GitHub that
is displaying the notebook.

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    menu.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image128.png)

2.  In the **RealTimeWorkspaceXX** page, navigate and click on
    **Import** button, then select **Notebook** and select **From this
    computer** as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image129.png)

3.  Select **Upload** from the **Import status** pane that appears on
    the right side of the screen.

> ![A screenshot of a computer Description automatically
> generated](./media/image130.png)

4.  Navigate and select **Lakehouse 1-Import Data, Lakehouse 2-Build
    Aggregation, Lakehouse 3-Create Star Schema** and **Lakehouse 4-Load
    Star Schema** notebooks from **C:\LabFiles\Lab 03** and click on the
    **Open** button.

![](./media/image131.png)

5.  You will see a notification stating **Imported successfully.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image132.png)

## Task 4. Import additional data

In order the make the reports more interesting, we need a bit more data
to work with. For both the Lakehouse and Data Science modules,
additional historical data can be imported to supplement the data that
is already ingested. The notebook works by looking at the oldest data in
the table, prepending the historical data.

1.  In the **RealTimeWorkspace** page, select the ***Lakehouse 1 -
    Import Data* **notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image133.png)

2.  Under **Explorer**, navigate and select the **Add data items**, then
    click on the ***Existing data sources*** as shown in the below
    images*.*

> ***Important Note***: You’ll need to add the Lakehouse to every
> imported notebook -- do this each time you open a notebook for the
> first time.
>
> ![](./media/image134.png)

3.  On the **OneLake data hub** window, select ***StockLakehouse*** and
    click on the **Add** button.

> ![](./media/image135.png)

4.  The ***raw_stock_data*** table was created when the Eventstream was
    configured, and is the landing place for the data that is ingested
    from the Event Hub.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image136.png)

**Note**: You will see the **Run** button when you hover your mouse over
the cell in the notebook.

5.  To start the notebook and execute the cell, select the **Run** icon
    that appears on the left side of the cell.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image137.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image138.png)

6.  To download and unzip historical data to the lakehouse unmanaged
    files, run the 2^(nd) and 4^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image139.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image140.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image141.png)

7.  Create the target table if it doesn't exist; find minimum date in
    the table to act as a cut-off point., select and run the 5^(th)
    cell.

> ![A screenshot of a computer code AI-generated content may be
> incorrect.](./media/image142.png)

8.  Run the 6^(th) cell, 7^(th) cell

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image143.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image144.png)

**Important**: While verifying CSV files, if you encounter the error
shown below, restart the kernel and run the cell again. 

6.  Loads the data and filters out data outside the timeframe window.
    select and run the 8^(h) cell and 9^(th) cell.

![](./media/image145.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image146.png)

9.  While similar to 'commenting out' sections of code, freezing cells
    is powerful in that any output of the cells are also preserved.

![A screenshot of a computer program Description automatically
generated](./media/image147.png)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image148.png)

10. Select the Stop button to stop the Spark kernel.![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image149.png)

# Exercise 6: Building the Aggregation Tables

In this exercise, you’ll build curated and aggregated data suitable for
use in building our dimensional model and in data science. With the raw
data having a per-second frequency, this data size is often not ideal
for reporting or analysis. Further, the data isn't cleansed, so we're at
risk of non-conforming data causing issues in reports or pipelines where
erroneous data isn't expected. These new tables will store the data at
the per-minute and per-hour level. Fortunately, *data wrangler* makes
this an easy task.

The notebook used here will build both aggregation tables, which are
silver-level artifacts. While it is common to separate medallion layers
into different Lakehouses, given the small size of our data and for the
purposes of our lab, we'll be using the same Lakehouse to store all
layers.

## **Task 1: Build Aggregation Tables* notebook**

Take a moment to scroll through the notebook. Be sure to add the default
Lakehouse if it is not already added.

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    menu.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image150.png)

2.  *In the **RealTimeWork*****space** page, click on ***Lakehouse 2 –
    Build Aggregation Tables*** notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image151.png)

3.  Under Explorer, navigate and select the **Add data items**, then
    click on the ***Existing data sources*** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image152.png)

4.  On the **OneLake data hub** window, select the ***StockLakehouse**,*
    and click on the **Add** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image153.png)

5.  To build Aggregate Tables, select and run the 1^(st) , 2^(nd) ,
    3^(rd) , and 4^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image154.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image155.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image156.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image157.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image158.png)

6.  Then, select and run the 5^(th) , 6^(th), 7^(th) , and 8^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image159.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image160.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image161.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image162.png)

7.  Add data wrangler, select **9^(th)** cell, navigate dropdown **Data
    Wrangler**. Navigate and click on **anomaly_df** to load the
    dataframe in data wrangler**.**

8.  We'll use the *anomaly_df* because it was intentionally created with
    a few invalid rows that can be tested. 

> ![](./media/image163.png)

9.  In data wrangler, we'll record a number of steps to process data.
    Notice the data is visualized in the central column. Operations are
    in the top left, while an overview of each step is in the
    bottom left.

![A screenshot of a computer Description automatically
generated](./media/image164.png)

10. To remove null/empty values, under *Operations,* click on the
    dropdown beside ***Find and replace***, then navigate and click on
    ***Drop missing values***.

![A screenshot of a computer Description automatically
generated](./media/image165.png)

11. From the ***Target columns*** dropdown, select
    the ***symbol*** and ***price* **columns and then click on ***Apply
    button** below it as shown in the image**.***

![A screenshot of a computer Description automatically
generated](./media/image166.png)

![A screenshot of a computer Description automatically
generated](./media/image167.png)

![A screenshot of a computer Description automatically
generated](./media/image168.png)

12. Under **Operations** dropdown, navigate and click on **Sort and
    filter**, then click on ***Filter** as shown in the below image*. 

> ![A screenshot of a computer Description automatically
> generated](./media/image169.png)

13. **Uncheck** *Keep matching rows*, select ***price*** as the target
    column, and set the condition to **Equal** to **0**.
    Click **Apply**in the *Operations* panel beneath the Filter

> Note: The rows with zero are marked red as they will be dropped (if
> the other rows are marked red, ensure to uncheck the *Keep matching
> rows* checkbox).

![A screenshot of a computer Description automatically
generated](./media/image170.png)

![A screenshot of a computer Description automatically
generated](./media/image171.png)

14. Click on **+*Add code to notebook*** in the upper left side of the
    page. On the **Add code to notebook** window, ensure that **Include
    pandas code** is unchecked and click on the **Add** button.

![A screenshot of a computer Description automatically
generated](./media/image172.png)

![A screenshot of a computer code Description automatically
generated](./media/image173.png)

15. The code inserted will look similar to the below.

![A screenshot of a computer Description automatically
generated](./media/image174.png)

16. Run the cell and observe the output. You’ll observe that the invalid
    rows were removed.

> ![A screenshot of a computer Description automatically
> generated](./media/image175.png)

![A screenshot of a computer Description automatically
generated](./media/image176.png)

The function created, **clean_data**, contains all of the steps in
sequence and can be modified as needed. Notice that each step performed
in data wrangler is commented. Because data wrangler was loaded with
the *anomaly_df*, the method is written to take that dataframe by name,
but this can be any dataframe that matches the schema.

17. Modify the function name
    from ***clean_data*** to *remove_invalid_rows*, and change the
    line *anomaly_df_clean = clean_data(anomaly_df)* to *df_stocks_clean
    = remove_invalid_rows(df_stocks)* . Also, while not necessary for
    functionality, you can change the name of the dataframe used in the
    function to simply *df* as shown below

18. Run this cell and observe the output.

    ```
    # Code generated by Data Wrangler for PySpark DataFrame
    
    def remove_invalid_rows(df):
      # Drop rows with missing data in columns: 'symbol', 'price'
    	 df = df.dropna(subset=['symbol', 'price'])
    	# Filter rows based on column: 'price'
    	 df = df.filter(~(df['price'] == 0))
    	 return df
    
    df_stocks_clean = remove_invalid_rows(df_stocks)
    display(df_stocks_clean)
    ```
![A screenshot of a computer Description automatically
generated](./media/image177.png)

19. This function will now remove the invalid rows from
    our *df_stocks* dataframe and return a new dataframe
    called *df_stocks_clean*. It is common to use a different name for
    the output dataframe (such as *df_stocks_clean*) to make the cell
    idempotent -- this way, we can go back and re-run the cell, make
    modifications, etc., without having to reload our original data.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image178.png)

## Task 2: Build aggregation routine

In this task, you’ll be more involved because we'll build a number of
steps in data wrangler, adding derived columns and aggregating the data.
If you get stuck, continue as best you can and use the sample code in
the notebook to help fix any issues after.

1.  Add new column datestamp in the ***Symbol/Date/Hour/Minute
    Aggregation** Section*, place your cursor in the *Add data wrangler
    here* cell and select the cell. Dropdown the **Data Wrangler**.
    Navigate and click on **df_stocks_clean** as shown in the below
    image.

> ![](./media/image179.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image180.png)

2.  In **Data Wrangler:df_stocks_clean** pane, select **Operations**,
    then select **New column by example.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image181.png)

3.  Under ***Target columns*** field, click on the dropdown and select
    ***timestamp***. Then, in the ***Derived column** **name*** field,
    enter +++**datestamp*+++***

> ![A screenshot of a computer Description automatically
> generated](./media/image182.png)

4.  In the new ***datestamp*** column, enter an example value for any
    given row. For example, if the **timestamp** is **2025-05-20
    06:37:00** enter **2025-05-20**. This allows data wrangler to infer we
    are looking for the date without a time component; once the columns
    autofill, click on the **Apply** button.

> ![](./media/image183.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image184.png)

5.  Similar to adding the ***datestamp*** column as mentioned in the
    above steps, click again on **New column by example** as shown in
    the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image185.png)

6.  Under *Target columns*, choose ***timestamp***. Enter a ***Derived
    column** name* of +++***hour+++*.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image186.png)

7.  In the new **hour**column that appear in the data preview, enter
    an hour for any given row -- but try to pick a row that has a unique
    hour value. For example, if the **timestamp** is **2025-05-20
    06:37:00**  enter **6**. You may need to enter example values for
    several rows, as shown here. Click on **Apply** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image187.png)

8.  Same as with the hour column, create a new ***minute* **column. In
    the new *minute* column, enter a minute for any given row. For
    example, if the *timestamp* is *2025-05-20 06:37:00*  enter *37*.
    You may need to enter example values for several rows.

> ![](./media/image188.png)

9.  The code generated should look similar to:

    ```nocopy
    # Derive column 'minute' from column: 'timestamp'
     
    # Transform based on the following examples:
     
    # timestamp Output
     
    # 1: "2025-05-20 06:37:00" = > "37"
     
    pandas_df_stocks_clean.insert(3, "minute", pandas_df_stocks_clean["timestamp"].str.split(":").str[1])
    ```
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image189.png)

10. Next, convert the hour column to an integer. Click on the **ellipsis
    (...)** in the corner of the *hour* column and select ***Change
    column** type*. Click on the dropdown beside ***New type***,
    navigate and select ***int32**,* then click on the ***Apply
    button*** as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image190.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image191.png)

11. Convert the minute column to an integer using the same steps as you
    just performed for the hour. Click on the **ellipsis (...)** in the
    corner of the ***minute* column** and select ***Change column**
    type*. Click on the dropdown beside ***New type***, navigate and
    select ***int32**,* then click on the ***Apply button*** as shown in
    the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image192.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image193.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image194.png)

12. **Now, under the Operations section, navigate and click on *Group by
    and aggregate*** as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image195.png)

13. Click on the dropdown under ***Columns to group by*** field and
    select ***symbol*, *datestamp*, *hour*, *minute***.

![A screenshot of a computer Description automatically
generated](./media/image196.png)

14. Click on +*Add aggregation*, create a total of three aggregations as
    shown in the below images and click on the ***Apply*** button.

- price: Maximum

- price: Minimum

- price: Last value

![A screenshot of a computer Description automatically
generated](./media/image197.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image198.png)

15. Click ***Add code to notebook*** in the upper left corner of the
    page. On the ***Add code to notebook* window**, ensure *Include
    pandas code* is unchecked, then click on the ***Add** button.*

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image199.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image200.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image201.png)

13. **Review the code, i**n the cell that is added, in the last two
    lines of the cell, notice the dataframe returned is
    named ***df_stocks_clean_1***. Rename
    this ***df_stocks_agg_minute***, and change the name of the function
    to ***aggregate_data_minute*,** as shown below.

    **Old**:

    ```nocopy
    def clean_data(df_stocks_clean):
    ```

    ```nocopy
    df_stocks_clean_1 = clean_data(df_stocks_clean)
    display(df_stocks_clean_1)
    ```

    **New**:

    ```
    def aggregate_data_minute(df_stocks_clean):
    ```

    ```
    df_stocks_agg_minute = aggregate_data_minute(df_stocks_clean)
    display(df_stocks_agg_minute)
    ```
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image202.png)

14. Code generated by Data Wrangler for PySpark DataFrame cell, select
    the **Run** icon that appears to the left of the cell upon hover.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image203.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image204.png)

**Note**: If you get stuck, refer to the commented-out code as a
reference. If any of the data wrangling steps don't seem to be quite
correct (not getting the correct hour or minute, for example), refer to
the commented-out samples. Step 7 below has a number of additional
considerations that may help.

**Note** : If you are facing any errors while running the code, try
this.
```
from pyspark.sql import functions as F

def aggregate_data_minute(df_stocks_clean):

    # Convert timestamp string → timestamp type
    df_stocks_clean = df_stocks_clean.withColumn(
        "timestamp", F.to_timestamp("timestamp")
    )

    # Extract date, hour, minute using built-in functions
    df_stocks_clean = (
        df_stocks_clean
        .withColumn("datestamp", F.to_date("timestamp"))
        .withColumn("hour", F.hour("timestamp"))
        .withColumn("minute", F.minute("timestamp"))
    )

    # Aggregate by symbol + timestamp parts
    df_stocks_clean = (
        df_stocks_clean
        .groupBy("symbol", "datestamp", "hour", "minute")
        .agg(
            F.max("price").alias("price_max"),
            F.min("price").alias("price_min"),
            F.last("price").alias("price_last")
        )
        .dropna()
        .orderBy("symbol", "datestamp", "hour", "minute")
    )

    return df_stocks_clean


df_stocks_agg_minute = aggregate_data_minute(df_stocks_clean)
display(df_stocks_agg_minute)
```
**Note:** If you'd like to comment-out (or uncomment) large blocks, you
can highlight the section of code (or CTRL-A to select everything in the
current cell) and use CTRL-/ (Control *slash*) to toggler commenting.

15. In the merge cell, select the **Run** icon that appears to the left
    of the cell upon hover. The merge function writes the data into the
    table:

    ```
    # write the data to the stocks_minute_agg table
    
    merge_minute_agg(df_stocks_agg_minute)
    ```
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image205.png)

## Task 3: Aggregate hourly

Let's review current progress - our per-second data has been cleansed,
and then summarized to the per-minute level. This reduces our rowcount
from 86,400 rows/day to 1,440 rows/day per stock symbol. For reports
that might show monthly data, we can further aggregate the data to
per-hour frequency, reducing the data to 24 rows/day per stock symbol.

1.  In the final placeholder under the *Symbol/Date/Hour* section, load
    the existing ***df_stocks_agg_minute*** dataframe into data
    wrangler.

2.  In the final placeholder under the ***Symbol/Date/Hour*** section,
    place your cursor in the *Add data wrangler here* cell and select
    the cell. Dropdown the **Data Wrangler,** navigate and click on
    ***df_stocks_agg_minute*** as shown in the below image.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image206.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image207.png)

3.  Under ***Operations*,** select ***Group by and aggregate***. Click
    on the dropdown below **Columns to group by** field and select
    ***symbol*, *datestamp*, and *hour***, and then click on **+Add
    aggregations**. Create the following three aggregations and click on
    Apply button below it, as shown in the below image.

- price_min: Minimum

- price_max: Maximum

- price_last: Last value

> ![A screenshot of a computer Description automatically
> generated](./media/image208.png)

4.  Example code is shown below. In addition to renaming the function
    to *aggregate_data_hour*, the alias of each price column has also
    been changed to keep the column names the same. Because we are
    aggregating data that has already been aggregated, data wrangler is
    naming the columns like price_max_max, price_min_min; we will modify
    the aliases to keep the names the same for clarity.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image209.png)

5.  Click ***Add code to notebook*** in the upper left corner of the
    page. On the ***Add code to notebook* window**, ensure *Include
    pandas code* is unchecked and click on the ***Add** button.*

> ![A screenshot of a computer Description automatically
> generated](./media/image210.png)

![A screenshot of a computer code Description automatically
generated](./media/image211.png)

![A screenshot of a computer Description automatically
generated](./media/image212.png)

6. In the cell that is added, in the last two lines of the cell, notice
    the dataframe returned is named def
    clean_data(df_stocks_agg_minute):, rename this

**def aggregate_data_hour(df_stocks_agg_minute):**

7. In the cell that is added, in the last two lines of the cell, notice
    the dataframe returned is named **df_stocks_agg_minute_clean =
    clean_data(df_stocks_agg_minute).**Rename this **df_stocks_agg_hour
    = aggregate_data_hour(df_stocks_agg_minute),** and change the name
    of the function **display(df_stocks_agg_minute_clean)**
    to *aggregate_data_minute*, as shown below. 

    reference Code:

    ```nocopy
    # Code generated by Data Wrangler for PySpark DataFrame
    
    from pyspark.sql import functions as F
    
    def aggregate_data_hour(df_stocks_agg_minute):
        # Performed 3 aggregations grouped on columns: 'symbol', 'datestamp', 'hour'
        df_stocks_agg_minute = df_stocks_agg_minute.groupBy('symbol', 'datestamp', 'hour').agg(
            F.max('price_max').alias('price_max'), 
            F.min('price_min').alias('price_min'), 
            F.last('price_last').alias('price_last'))
        df_stocks_agg_minute = df_stocks_agg_minute.dropna()
        df_stocks_agg_minute = df_stocks_agg_minute.sort(df_stocks_agg_minute['symbol'].asc(), df_stocks_agg_minute['datestamp'].asc(), df_stocks_agg_minute['hour'].asc())
        return df_stocks_agg_minute
    
    df_stocks_agg_hour = aggregate_data_hour(df_stocks_agg_minute)
    display(df_stocks_agg_hour)
    
    ```

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image213.png)

8. Select and **Run** the cell.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image214.png)

9. The code to merge the hour aggregated data is in the next cell:
    **merge_hour_agg(df_stocks_agg_hour)**

10. Run the cell to complete the merge. There are a few utility cells at
    the bottom for checking the data in the tables -- explore the data a
    bit and feel free to experiment.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image215.png)

11. Use **Handy SQL Commands for testing** section for testing, cleaning
    out tables to re-run, etc. Select and **Run** the cells in this
    section.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image216.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image217.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image218.png)

12. Select the Stop button to stop the Spark kernel.![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image149.png)

# Exercise 7: Building the Dimensional Model

In this exercise, we'll further refine our aggregation tables and create
a traditional star schema using fact and dimension tables. If you've
completed the Data Warehouse module, this module will produce a similar
result, but is different in approach by using notebooks within a
Lakehouse.

**Note**: It is possible to use pipelines to orchestrate activities, but
this solution will be done completely within notebooks.

## Task 1: Create schema

This run-once notebook will setup the schema for building the fact and
dimension tables. Configure the sourceTableName variable in the first
cell (if needed) to match the hourly aggregation table. The begin/end
dates are for the date dimension table. This notebook will recreate all
tables, rebuilding the schema: existing fact and dimension tables will
be overwritten.

1.  Click on **RealTimeWorkspace** on the left-sided navigation menu.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image219.png)

2.  *In the RealTimeWorkshop* workspace, select the ***Lakehouse 3 –
    Create Star Schema*  **notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image220.png)

3.  Under the Explorer, Select **Add data items** and select **Existing
    data sources** ![A screenshot of a computer AI-generated content may
    be incorrect.](./media/image221.png)

4.  On the OneLake data hub window, select ***StockLakehouse***  and
    click on the **Add** button.

> ![](./media/image222.png)

5.  With the notebook loaded and the Lakehouse attached, notice the
    schema on the left. In addition to the **raw_stock_data** table,
    there should be the **stocks_minute_agg** and **stocks_hour_agg**
    tables.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image223.png)

6.  Run each cell individually by clicking the **play** button on the
    left side of each cell to follow along with the process.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image224.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image225.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image226.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image227.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image228.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image229.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image230.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image231.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image232.png)

7.  When all cells have been run successfully, navigate to
    **StocksLakehouse** section, click on the horizontal ellipsis beside
    **Tables** **(...)**, then navigate and click on ***Refresh** as
    shown in the below image*.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image233.png)

8.  Now, you can see all additional tables ***dim_symbol*, *dim_date*,
    and *fact_stocks_daily_prices*** for our dimensional model.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image234.png)

9.  Select the Stop button to stop the Spark kernel.![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image149.png)

## Task 2: Load fact table

Our fact table contains the daily stock prices (the high, low, and
closing price), while our dimensions are for our date and stock symbols
(which might contain company details and other information). Although
simple, conceptually, this model represents a star schema that can be
applied to larger datasets.

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    menu.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image235.png)

2.  In the RealTimeWorkshop workspace, select the **Lakehouse 4 – Load
    fact table** notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image236.png)

3.  Under the Explorer, select **Add data items**, then select
    **Existing data sources***.*

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image237.png)

4.  On the OneLake data hub tab, select the ***StockLakehouse***  and
    click on the **Add** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image238.png)

5.  Select and run each cell individually.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image239.png)

6.  Function adds symbols to dim_symbol that may not exist in table,
    select and **Run** the 2^(nd) and 3^(rd) cells.

> ![A screenshot of a computer Description automatically
> generated](./media/image240.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image241.png)

7.  To get new stock data to ingest, starting at watermark, select and
    run the 4^(th) cell.

> ![](./media/image242.png)

8.  Load the date dimension for later joins, select and **Run** the
    5^(th), 6^(th), and 7^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image243.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image244.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image245.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image246.png)

9.  To join the aggregated data to the date dimension, select and
    **Run** the 8^(th) and 9^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image247.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image248.png)

10. Create a final view with cleaned names for processing ease, select
    and **Run** the 10^(th), 11^(th) , and 12^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image249.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image250.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image251.png)

11. To obtain the result and plot a graph, select and **Run** 13^(th)
    and 14^(th) cells.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image252.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image253.png)
>
> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image254.png)

12. To validate the created tables, right click on the horizontal
    ellipsis (…) beside **Tables,** then navigate and click on
    **Refresh.** The tables will appear.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image255.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image256.png)

13. To schedule the notebook to run periodically, click on
    the ***Run*** tab, and click on ***Schedule** as shown in the below
    image.*

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image257.png)

14. In Lackhouse 4-Load Star Schema tab, select the below details and
    click on the **Apply** button.

- Schedule run: **On**

- Repeat: **Hourly**

- Every: **4 hours**

- Select today date

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image258.png)

12. Select the Stop button to stop the Spark kernel.![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image149.png)

## Task 3: Build semantic model and simple report

In this task, we'll create a new semantic model that we can use for
reporting, and create a simple Power BI report.

1.  Now, click on **StocksLakehouse** on the left-sided navigation menu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image259.png)

2.  In the ***StocksLakehouse** window,* navigate and click on ***New
    semantic model** in the command bar.*

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image260.png)

3.  Name the model +++**StocksDimensionalModel**+++and select the
    **fact_stocks_daily_prices**, **dim_date** and **dim_symbol**
    tables. Then, click on the **Confirm** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image261.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image262.png)

4.  When the semantic model opens, we need to define relationships
    between the fact and dimension tables.

5.  From the **fact_Stocks_Daily_Prices** table, drag
    the **Symbol_SK**  field and drop it on
    the **Symbol_SK**   field in the **dim_Symbol** table to create a
    relationship. The **New relationship** dialog box appears.

![A screenshot of a computer Description automatically
generated](./media/image263.png)

6.  In the **New relationship** dialog box:

- **From table** is populated with **fact_Stocks_Daily_Prices** and the
  column of **Symbol_SK.**

- **To table** is populated with **dim_symbol**  and the column of
  **Symbol_SK**

- Cardinality: **Many to one (\*:1)**

- Cross filter direction: **Single**

- Leave the box next to **Make this relationship active** selected.

- Select **Save.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image264.png)

> ![](./media/image265.png)

7.  From the **fact_Stocks_Daily_Prices** table, drag
    the **PrinceDateKey**  field and drop it on
    the **DateKey**   field in the **dim_date** table to create a
    relationship. The **New relationship** dialog box appears.

![A screenshot of a computer Description automatically
generated](./media/image266.png)

8.  In the **New relationship** dialog box:

- **From table** is populated with **fact_Stocks_Daily_Prices** and the
  column of **PrinceDateKey.**

- **To table** is populated with **dim_date**  and the column of
  **DateKey**

- Cardinality: **Many to one (\*:1)**

- Cross filter direction: **Single**

- Leave the box next to **Make this relationship active** selected.

- Select **Save.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image267.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image268.png)

9.  Click **New Report** to load the semantic model in Power BI.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image269.png)

10. In the **Power BI** page, under **Visualizations**, click to the
    **Line chart** icon to add a **Column chart** to your report.

- On the **Data** pane, expand **fact_Stocks_Daily_Prices**  and check
  the box next to **PriceDateKey**. This creates a column chart and adds
  the field to the **X-axis**.

- On the **Data** pane, expand **fact_Stocks_Daily_Prices** and check
  the box next to **ClosePrice**. This adds the field to the **Y-axis**.

- On the **Data** pane, expand **dim_Symbol** and check the box next
  to **Symbol**. This adds the field to the **Legend**.

![A screenshot of a computer Description automatically
generated](./media/image270.png)

11. Under **Filters,** select **PriceDateKey** and enter the below
    details. Click on the **Apply filter**

- Filter type: **Relative date**

- Show items when the value: **is in the last 45 days**

![A screenshot of a computer Description automatically
generated](./media/image271.png)

![A screenshot of a computer Description automatically
generated](./media/image272.png)

12. From the ribbon, select **File** \> **Save as.**

![A screenshot of a graph Description automatically
generated](./media/image273.png)

13. In the Save your report dialog box,
    enter +++**StocksDimensional**+++ as the name of your report and
    select **your workspace**. Click on the **Save button.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image274.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image275.png)

# Exercise 8: Building and storing an ML model

## Task -1: Import the notebook

1.  In the **StockDimensionalModel** page, click on
    **RealTimeWorkspace** on the left-sided navigation menu.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image276.png)

2.  In the **Synapse Data Engineering** workspace page, navigate and
    click on **+New** button, then select **Import notebook.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image277.png)

3.  In the **Import status** pane that appear on the right side, click
    on **Upload** .

![A screenshot of a computer Description automatically
generated](./media/image278.png)

4.  Navigate to **C:\LabFiles\Lab 05** and select **DS 1-Build Model, DS
    2-Predict Stock Prices and DS 3-Forecast All** notebooks, then click
    on the **Open** button.

![A screenshot of a computer Description automatically
generated](./media/image279.png)

5.  You will see a notification stating - **Imported successfully.**

> ![A screenshot of a computer Description automatically
> generated](./media/image280.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image281.png)

6.  *In the **RealTimeWorkspace**,* click on **DS 1-Build Model**
    notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image282.png)

7.  Under the Explorer, select **Lakehouse** and click on the
    **Add** button.

> ***Important*:** You will need to add the Lakehouse to every imported
> notebook -- do this each time you open a notebook for the first time.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image283.png)

8.  On the OneLake data hub window, select the ***StockLakehouse*** and
    click on the **Continue** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image284.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image285.png)

## Task 2: Explore and run the notebook

The **DS 1** notebook is documented throughout the notebook, but in short,
the notebook carries out the following tasks:

- Allows us to configure a stock to analyze (such as WHO or IDGD)

- Downloads historical data to analyze in CSV format

- Reads the data into a dataframe

- Completes some basic data cleansing

- Loads [Prophet](https://facebook.github.io/prophet/), a module for
  conducting time series analysis and prediction

- Builds a model based on historical data

- Validates the model

- Stores the model using MLflow

- Completes a future prediction

The routine that generates the stock data is largely random, but there
are some trends that should emerge. Because we don't know when you might
be doing this lab, we've generated several years’ worth of data. The
notebook will load the data and will truncate future data when building
the model. As part of an overall solution, we'd then supplement the
historical data with new real-time data in our Lakehouse, re-training
the model as necessary (daily/weekly/monthly).

1.  In 1^(st) cell uncomment the STOCK_SYMBOL=”IDGD” and
    STOCK_SYMBOL=”BCUZ”, then select and **Run** the cell.

![](./media/image286.png)

2.  Click ***Run all*** in the top toolbar and follow along as the work
    progresses.

3.  The notebook will take roughly **15-20** minutes to execute -- some
    steps, like **training the model** and **cross validation**, will
    take some time.

![A screenshot of a computer Description automatically
generated](./media/image287.png)

![A screenshot of a computer Description automatically
generated](./media/image288.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image289.png)
>
> ![A screenshot of a computer code Description automatically
> generated](./media/image290.png)
>
> ![A screenshot of a computer program Description automatically
> generated](./media/image291.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image292.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image293.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image294.png)
>
> ![A screenshot of a computer code AI-generated content may be
> incorrect.](./media/image295.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image296.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image297.png)
>
> ![A screen shot of a sound wave AI-generated content may be
> incorrect.](./media/image298.png)
>
> ![A screen shot of a graph Description automatically
> generated](./media/image299.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image300.png)
>
> ![A screenshot of a graph AI-generated content may be
> incorrect.](./media/image301.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image302.png)
>
> ![A graph with blue lines AI-generated content may be
> incorrect.](./media/image303.png)

![A screenshot of a computer code Description automatically
generated](./media/image304.png)

![A screenshot of a computer Description automatically
generated](./media/image305.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image306.png)
>
> ![A screenshot of a computer program Description automatically
> generated](./media/image307.png)
>
> ![A graph with a blue line Description automatically
> generated](./media/image308.png)
>
> ![A graph showing a graph Description automatically generated with
> medium confidence](./media/image309.png)
>
> ![A screenshot of a computer program Description automatically
> generated](./media/image310.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image311.png)

![A screenshot of a computer program Description automatically
generated](./media/image312.png)

![A screenshot of a computer Description automatically
generated](./media/image313.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image314.png)
>
> ![](./media/image315.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image316.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image317.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image318.png)
>
> ![A screen shot of a graph AI-generated content may be
> incorrect.](./media/image319.png)
>
> ![A screenshot of a computer code AI-generated content may be
> incorrect.](./media/image320.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image321.png)

4.  Select the Stop button to stop the Spark kernel.
   ![A screenshot of a computer AI-generated content may be incorrect.](./media/image149.png)

##  Task 3: Examine the model and runs

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    menu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image322.png)

2.  Experiments and runs can be viewed in the workspace resource list.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image323.png)

3.  In the **RealTimeWorkspace** page, select
    **WHO-stock-prediction-model** of ML model type.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image324.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image325.png)

4.  Metadata, in our case, includes input parameters we may tune for our
    model, as well as metrics on the model's accuracy, such as root mean
    square error (RMSE). RMSE represents the average error -- a zero
    would be a perfect fit between model and actual data, while higher
    numbers show an increase error. While lower numbers are better, a
    "good" number is subjective based on the scenario.

> ![](./media/image326.png)

# Exercise 9-Using models, saving to Lakehouse, building a report

## Task 1: Open and explore the *Predict Stock Prices* notebook

The notebook has been broken out into function definitions, such as *def
write_predictions*, which help encapsulate logic into smaller steps.
Notebooks can include other libraries (as you've seen already at the top
of most notebooks), and can also execute other notebooks. This notebook
completes these tasks at a high level:

- Creates the stock predictions table in the Lakehouse, if it doesn't
  exist

- Gets a list of all stock symbols

- Creates a prediction list by examining available ML models in MLflow

- Loops through the available ML models:

  - Generates predictions

  - Stores predictions in the lakehouse

1.  Now,click on **RealTimeWorkspace** on the left-sided navigation
    pane.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image327.png)

2.  *In the **RealTime*****Workspace**, click on the **DS 2-Predict
    Stock Prices** notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image328.png)

3.  Under the Explorer, select the **Lakehouse**, then click on the
    **Add** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image329.png)

4.  On the OneLake data hub window, select the ***StockLakehouse*** and
    click on the **Connect** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image330.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image331.png)

## Task 2: Run the notebook

1.  Creates the stock predictions table in the Lakehouse, select and
    **Run** the 1^(st) and 2^(nd) cells.

> ![](./media/image332.png)
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image333.png)

2.  Gets a list of all stock symbols, select and **Run** the 3^(rd) and
    4^(th) cells.

![](./media/image334.png)

![](./media/image335.png)

3.  Creates a prediction list by examining available ML models in
    MLflow, select and **Run** the 7^(th), 8^(th) , 9^(th) , and 10^(th)
    cells.

![](./media/image336.png)

![](./media/image337.png)

![](./media/image338.png)

![](./media/image339.png)

4.  To build predictions for each model store in Lakehouse , select and
    **Run** the 11^(th) and 12^(th) cells.

![](./media/image340.png)

![](./media/image341.png)

5.  When all cells have been run, refresh the schema by clicking on the
    three dots **(...)** beside *Tables,* then navigate and click on
    ***Refresh*.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image342.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image343.png)

# Exercise 10: Solution in practice

The first two sections in this module are common approaches to
developing a data science solution. The first section covered the
development of the model (exploration, feature engineering, tuning,
etc.), building, and then deploying the model. The second section
covered the consumption of the model, which is typically a separate
process and may even be done by different teams.

However, in this specific scenario, there is little benefit to creating
the model and generating predictions separately because the model we
developed is time-based univariate - the predictions that the model
generates will not change without retraining the model with new data.

Most ML models are multivariate, for example, consider a travel time
estimator that calculates travel time between two locations such model
could have dozens of input variables, but two major variables would
certainly include the time of day and weather conditions. Because the
weather is changing frequently, we'd pass this data into the model to
generate new travel time predictions (inputs: time of day and weather,
output: travel time).

Therefore, we should generate our predictions immediately after creating
the model. For practical purposes, this section shows how we could
implement the ML model building and forecasting in a single step.
Instead of using the downloaded historical data, this process will use
the aggregation tables built in module 06. Our data flow can be
visualizad like so:

## Task 1: Import the notebook

Take some time exploring the *DS 3 - Forecast All* notebook, and notice
a few key things:

- Stock data is read from the stock_minute_agg table.

- All symbols are loaded, and predictions are made on each.

- The routine will check MLflow for matching models, and load their
  parameters. This allows data science teams to build optimal
  parameters.

- Predictions for each stock are logged to the stock_predicitions table.
  There is no persistence of the model.

- There is no cross validation or other steps performed in 07a. While
  these steps are useful for the data scientist, it's not needed here.

1.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    menu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image344.png)

2.  *In the **RealTimeWorkspace***, click on the **DS 3-Forecast All**
    notebook.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image345.png)

3.  Under the Explorer select the **Lakehouse** and click on the
    ***Add .***

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image346.png)

4.  On the OneLake data hub window, select the ***StockLakehouse*** and
    click on the **Add** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image347.png)

5.  Select and **Run** the 1^(st) cell.

![](./media/image348.png)

6.  Click ***Run all*** in the command and follow along as the work
    progresses.

7.  Running the notebook for all symbols could take 10 minutes.
    ![](./media/image349.png)

![](./media/image350.png)

![](./media/image351.png)

![](./media/image352.png)

![](./media/image353.png)

![](./media/image354.png)

![](./media/image355.png)

![A screenshot of a computer program Description automatically
generated](./media/image356.png)

![A graph showing the growth of the stock market Description
automatically generated](./media/image357.png)

![A graph showing different colored lines Description automatically
generated](./media/image358.png)

![A graph showing different colored lines Description automatically
generated](./media/image359.png)

![A graph showing different colored lines Description automatically
generated](./media/image360.png)

![A graph of different colored lines Description automatically
generated](./media/image361.png)

![A graph showing the growth of a company Description automatically
generated](./media/image362.png)

![A graph showing different colored lines Description automatically
generated](./media/image363.png)

![A graph showing different colored lines Description automatically
generated](./media/image364.png)

8.  When all cells have been run, refresh the schema by clicking the
    three dots **(...)** dots to the right of the *Tables*, then
    navigate and click on **Refresh**

![](./media/image365.png)

# Exercise 12: Building a Prediction Report

## Task 1: Build a semantic model

In this step, we'll build a semantic model (formerly called Power BI
datasets) to use in our Power BI report. A semantic model represents
data that is ready for reporting and acts as an abstraction on top of a
data source. Typically, a semantic model will be purpose built (serving
a specific reporting need) and may have transformations, relationships,
and enrichments like measures to make developing reports easier

1.  Click on **RealTimeWorkspace** on the left-sided navigation menu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image366.png)

2.  To create a semantic model, navigate and click on the Lakehouse
    i.e., **StackLakehouse.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image367.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image368.png)

3.  In the ***StocksLakehouse** page,* click on ***New semantic model***
    in the command bar.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image369.png)

4.  In the **New semantic model** pane **Name** field, enter the name of
    the model as +++***StocksLakehousePredictions+++**,* select the
    **stock_prediction**, and **dim_symbol** tables. Then, click on the
    **Confirm** button as shown in the below image.

![](./media/image370.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image371.png)

5.  When the semantic model opens, we need to define relationships
    between the stock_prediction and dim_symbol tables.

6.  From the **stock_prediction** table, drag the ***Symbol***  field
    and drop it on the ***Symbol***  field in the **dim_Symbol** table
    to create a relationship. The **New relationship** dialog box
    appears.

![](./media/image372.png)

7.  In the **New relationship** dialog box:

- **From table** is populated with **stock_prediction** and the column
  of **Symbol.**

- **To table** is populated with **dim_symbol**  and the column of
  **Symbol.**

- Cardinality: **Many to one (\*:1)**

- Cross filter direction: **Single**

- Leave the box next to **Make this relationship active** selected.

- Select **Ok**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image373.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image374.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image375.png)

## Task 2: Build the report in Power BI Desktop

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: <https://powerbi.microsoft.com/en-us/desktop/> ,
    then press the **Enter** button.

2.  Click on the **Download free** button.

![](./media/image376.png)

9.  In case, **This site is trying to open Microsoft Store** dialog box
    appears, then click on **Open** button.

![](./media/image377.png)

10. Under **Power BI Desktop**, click on the **Get** button.

![A screenshot of a computer Description automatically
generated](./media/image378.png)

11. Now, click on the **Open** button.

![A screenshot of a computer Description automatically
generated](./media/image379.png)

12. Enter your **tenant** credentials and click on the **Next** button.

![A screenshot of a computer Description automatically
generated](./media/image380.png)

13. Enter the **Administrative password** from the **Resources** tab and
    click on the **Sign in** button**.**

![A screenshot of a computer Description automatically
generated](./media/image381.png)

14. In Power BI Desktop, select **New.**

![A screenshot of a computer Description automatically
generated](./media/image382.png)

15. On the *Home* ribbon, click the ***OneLake data hub*** and select
    **KQL Database.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image383.png)

16. On the **OneLake data hub** window, select the **StockDB**  and
    click on the **Connect** button.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image384.png)

17. Enter your tenant credentials and click on the **Next** button.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image385.png)

18. Enter the **Administrative password** from the **Resources** tab and
    click on the **Sign in** button**.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image386.png)

19. In the Navigator page, under **Display Options**, select
    **StockPrice** table, then click on the **Load** button.

![A screenshot of a computer Description automatically
generated](./media/image387.png)

20. In the ***Connection settings*** dialog box, select ***DirectQuery**
    radio button and click on **OK** button.*

![A screenshot of a computer Description automatically
generated](./media/image388.png)

21. On the ***Home*** ribbon, click the ***OneLake data hub*** and
    select **Power BI semantic models** as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image389.png)

22. On the **OneLake data hub** window, select
    the **StockLakehousePredictions** from the list and click on the
    **Connect** button.

![A screenshot of a computer Description automatically
generated](./media/image390.png)

23. In the **Connect to your data** page, select **dim_symbol,
    stock_prediction**, and click on the **Submit** button.

![A screenshot of a computer Description automatically
generated](./media/image391.png)

24. In this case, we can dismiss the **Potential security risk** warning
    by clicking on the **OK** button.

![A screenshot of a computer Description automatically
generated](./media/image392.png)

![A screenshot of a computer Description automatically
generated](./media/image393.png)

25. Click on ***Modeling*** in the command bar, then click on ***Manage
    relationships.***

![A screenshot of a computer Description automatically
generated](./media/image394.png)

26. In the **Manage relationships** pane, select **New** as shown in the
    below image.

![A screenshot of a computer Description automatically
generated](./media/image395.png)

27. Create a new **many-to-many** relationship between
    the **StockPrice** - **symbol**and
    the **stocks_prediction** - **Symbol**(after selecting the
    table, ensure to select the symbol columns in each table). Set the
    cross filter direction to ***Both***, and make sure the cardinality
    is set to ***Many-to-many*.** Then, click on the **OK** button.

> ![A screenshot of a computer Description automatically
> generated](./media/image396.png)

28. In Mange relationships page, click on the **Close** button.

![A screenshot of a computer Description automatically
generated](./media/image397.png)

29. In the **Power BI** page, under **Visualizations**, click on the
    **Line chart** icon to add a **Column chart** to your report.

- On the **Data** pane, expand **StockPrice**  and check the box next
  to **timestamp**. This creates a column chart and adds the field to
  the **X-axis**.

- On the **Data** pane, expand **StockPrice** and check the box next
  to **Price**. This adds the field to the **Y-axis**.

- On the **Data** pane, expand **StockPrice** and check the box next
  to **Symbol**. This adds the field to the **Legend**.

- **Filter**: **timestamp** to ***Relative time** is in the last **15
  minutes**.

![A screenshot of a computer Description automatically
generated](./media/image398.png)

![A screenshot of a computer Description automatically
generated](./media/image399.png)

30. In the **Power BI** page, under **Visualizations**, click on the
    **Line chart** icon to add a **Column chart** to your report.

- On the **Data** pane, expand **StockPrice**  and check the box next
  to **timestamp**. This creates a column chart and adds the field to
  the **X-axis**.

- On the **Data** pane, expand **StockPrice** and check the box next
  to **Price**. This adds the field to the **Y-axis**.

- On the **Data** pane, expand **dim_symbol** and check the box next
  to **Market**. This adds the field to the **Legend**.

- **Filter**: **timestamp** to ***Relative time** is in the last **1
  hour***.

![A screenshot of a computer Description automatically
generated](./media/image400.png)

![A screenshot of a computer Description automatically
generated](./media/image401.png)

![A screenshot of a computer Description automatically
generated](./media/image402.png)

![A screenshot of a computer Description automatically
generated](./media/image403.png)

31. In the **Power BI** page, under **Visualizations**, click on the
    **Line chart** icon to add a **Column chart** to your report.

- On the **Data** pane, expand **Stock_prediction**  and check the box
  next to **predict_time**. This creates a column chart and adds the
  field to the **X-axis**.

- On the **Data** pane, expand **Stock_prediction**  and check the box
  next to **yhat**. This adds the field to the **Y-axis**.

- On the **Data** pane, expand **Stock_prediction** and check the box
  next to **Symbol**. This adds the field to the **Legend**.

- **Filter**: **predict_time** to ***Relative date** in the last **3
  days***.

![A screenshot of a computer Description automatically
generated](./media/image404.png)

![A screenshot of a computer Description automatically
generated](./media/image405.png)

![A screenshot of a computer Description automatically
generated](./media/image406.png)

![A screenshot of a computer Description automatically
generated](./media/image407.png)

32. In the **Power BI** page, under **Data,** right click
    the ***stocks_prediction*** table and select ***New measure.***

![A screenshot of a computer Description automatically
generated](./media/image408.png)

33. Measures are formulas written in the Data Analysis Expressions (DAX)
    language; for this DAX formula, enter +++***currdate = NOW()+++* **

![A screenshot of a computer Description automatically
generated](./media/image409.png)

34. With the prediction chart selected, navigate to the additional
    visualization options i.e., the magnifying glass/chart icon and add
    a **new X-Axis Constant Line***.

![A screenshot of a computer Description automatically
generated](./media/image410.png)

35. Under **Value**, use the formula button **(fx)** to choose a field.

![A screenshot of a computer Description automatically
generated](./media/image411.png)

36. In **Value -Apply settings to** page, click on the dropdown under
    **what field should we base this on?**, then click on the dropdown
    of **stocks_prediction**, select the ***currdate* **measure. Then,
    click on the **OK** button.

![A screenshot of a computer Description automatically
generated](./media/image412.png)

![A screenshot of a computer Description automatically
generated](./media/image413.png)

37. Navigate to the additional visualization options i.e., the
    magnifying glass/chart icon, turn On the **Shade area**.
    ![A screenshot of a computer Description automatically generated](./media/image414.png)

39. Configured the relationships between tables, all visual should cross
    filter; when selecting either a symbol or market on a chart, all
    visuals should react accordingly. As shown in the below image, the
    **NASDAQ** market is selected on the upper-right market chart:

![A screenshot of a computer Description automatically
generated](./media/image415.png)

![A screenshot of a computer Description automatically
generated](./media/image416.png)

39. Click on the **Publish** in the command bar.*

![A screenshot of a computer Description automatically
generated](./media/image417.png)

40. In **Microsoft Power BI Desktop** dialog box, click on the **Save**
    button.

![A screenshot of a computer Description automatically
generated](./media/image418.png)

41. In **Save this file** dialog box, enter the Name as **Prediction
    Report** and select the location. Then, click on the **Save**
    button.

![A screenshot of a computer Description automatically
generated](./media/image419.png)

## Task 3: Clean up resources

1.  Select your workspace, the  ***RealTimeFabricWorkspaceXXX***  from
    the left-hand navigation menu. It opens the workspace item view.

2.  Select the ***...*** option under the workspace name and
    select **Workspace settings**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image420.png)

3.  Select **General** and **Remove this workspace.**

![A screenshot of a computer Description automatically
generated](./media/image421.png)

4.  Open your browser, navigate to the address bar, type or paste the
    following URL:
    [*https://portal.azure.com/*](https://portal.azure.com/), then press
    the **Enter** button.

5.  To delete the fabric account, navigate to **Azure portal Home**
    page, click on **Resource groups**.

![A screenshot of a computer Description automatically
generated](./media/image422.png)

6.  Click on the resource group that you’ve created.

![A screenshot of a computer Description automatically
generated](./media/image423.png)

7.  In the Resource group home page, select the **Delete resource
    group** button

![A screenshot of a computer Description automatically
generated](./media/image424.png)

8.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button.

![A screenshot of a computer Description automatically
generated](./media/image425.png)

![A screenshot of a computer error Description automatically
generated](./media/image426.png)

![A screenshot of a computer Description automatically
generated](./media/image427.png)

9.  Navigate to **Azure portal Home** page, click on **Resource
    groups**.

![A screenshot of a computer Description automatically
generated](./media/image422.png)

10. Click on the resource group that you’ve created.

![A screenshot of a computer Description automatically
generated](./media/image428.png)

11. In the Resource group home page, select the **Delete resource
    group** button

![A screenshot of a computer Description automatically
generated](./media/image429.png)

12. In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button.

![A screenshot of a computer Description automatically
generated](./media/image430.png)

![A screenshot of a computer error Description automatically
generated](./media/image426.png)

![A screenshot of a computer Description automatically
generated](./media/image427.png)

**Summary**

In this lab, you’ve signed up for the Microsoft Fabric trial and
redeemed Azure Pass, followed by configuring permissions and creating
necessary resources within the Azure portal such as Fabric Capacity,
Workspace, and Storage Account. Then, you’ve deployed the stock
generator app via Azure Container Instance using an ARM template to
generate real-time stock data. Additionally, you’ve configured
Eventstream in Microsoft Fabric to ingest data from Azure Event Hubs and
prepared the KQL Database to store this data efficiently. In this lab,
you’ve established a fully functional environment to proceed with
subsequent lab exercises related to real-time analytics and data
processing.

In this lab, you embarked on a comprehensive exploration of stock price
data using Kusto Query Language (KQL). Starting with the creation of a
KQL queryset named "StockQueryset," you executed a series of
increasingly complex queries to analyze various facets of the data. From
viewing sample records to calculating price differentials over time and
identifying significant price gains, each query unveils valuable
insights into the dynamics of stock prices. By leveraging windowing
functions, aggregation techniques, and data partitioning, you gained a
deeper understanding of stock price trends and fluctuations.

Then, you’ve shifted the focus to optimizing Power BI reporting
efficiency, where you configured refresh rates and crafted dynamic
visualizations for real-time dashboards. By configuring refresh rates in
the Power BI admin portal and creating Power BI reports based on the
previously defined KQL queries, you ensured timely updates and enabled
insightful visualization of stock price data. Through tasks like
creating visualizations for percent change and configuring auto-refresh
settings, you harnessed the full potential of Power BI to drive informed
decision-making and enhanced business intelligence capabilities.

In this lab, you’ve configured a comprehensive Lakehouse infrastructure
and implemented data processing pipelines to handle real-time and batch
data streams effectively. You’ve started with the creation of the
Lakehouse environment, the lab progresses to configuring Lambda
architecture for processing hot and cold data paths.

You’ve applied data aggregation and cleansing techniques to prepare the
raw data stored in the Lakehouse for downstream analytics. You’ve built
aggregation tables to summarize data at different levels of granularity,
facilitating efficient querying and analysis. Subsequently, you’ve built
a dimensional model within the Lakehouse, incorporating fact and
dimension tables. You’ve defined the relationships between these tables
to support complex queries and reporting requirements.

Finally, you’ve generated a semantic model to provide a unified view of
the data, enabling the creation of interactive reports using
visualization tools like Power BI. This holistic approach enables
efficient data management, analysis, and reporting within the Lakehouse
environment.

In this lab, you’ve learned how to approach a data science solution in
Fabric by importing and executing a notebook that processed data,
created a ML model of the data using Prophet, and stored the model in
MLflow. Additionally, you’ve evaluated the model using cross validation
and tested the model using additional data.

You’ve followed up on the creation of the ML model by consuming the ML
model, generating predictions, and storing those predictions in the
Lakehouse. Refined the process to build a model and generate predictions
in a single step, streamlining the process of creating predictions and
operationalizing the model generation. Then, you’ve created a report (or
reports) that show the current stock prices with predicted values,
making the data available for business users.

