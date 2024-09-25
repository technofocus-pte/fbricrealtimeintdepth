**Introduction**

Microsoft Fabric Real-Time Analytics is a fully managed,
high-performance, big data analytics platform that makes it easy to
analyze high volumes of data in near real time. The Microsoft Fabric
Real-Time Analytics toolbox gives you an end-to-end solution for data
ingestion, query, visualization, and management.

By analyzing structured, semi-structured, and unstructured data across
time series, and by using Machine Learning, Microsoft Fabric Real-Time
Analytics makes it simple to extract key insights, spot patterns and
trends, and create forecasting models. Microsoft Fabric Real-Time
Analytics is scalable, secure, robust, and enterprise-ready, and is
useful for log analytics, time series analytics, IoT, and
general-purpose exploratory analytics.

**Objective**

- Create **KQL database** in your Fabric enabled Power BI workspace

- Run powerful KQL queries to explore the data via **KQL Queryset**

- Manage KQL Database using control commands and policies

- Visualize data in Fabric **Real Time Dashboards** 

# **Exercise 1: Database Creation, Data Ingestion and Exploration**

This exercise will focus on enabling the participants to Create a KQL
database, and ingest data into the KQL database table - One click
ingestion of historic data , update policy to perform ETL and write some
KQL queries.

## **Task 0: Sync Host environment time**

1.  In your VM, navigate and click in the **Search bar**, type
    **Settings** and then click on **Settings** under **Best match**.

> <img src="./media/image1.png" style="width:4.65394in;height:3.99955in"
> alt="A screenshot of a computer Description automatically generated" />

2.  On Settings window, navigate and click on **Time & language**.

<img src="./media/image2.png" style="width:5.92237in;height:5.21371in"
alt="A screenshot of a computer Description automatically generated" />

3.  On **Time & language** page, navigate and click on **Date & time**.

<img src="./media/image3.png" style="width:6.5in;height:5.27639in"
alt="A screenshot of a computer Description automatically generated" />

4.  Scroll down and navigate to **Additional settings** section, then
    click on **Syn now** button. It will take 3-5 minutes to syn.

<img src="./media/image4.png" style="width:6.5in;height:5.04861in"
alt="A screenshot of a computer Description automatically generated" />

5.  Close the **Settings** window.

<img src="./media/image5.png" style="width:6.48341in;height:5.10915in"
alt="A screenshot of a computer Description automatically generated" />

## Task 1: Sign in to Power BI account and sign up for the free [Microsoft Fabric trial](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial)

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL:
    [https://app.fabric.microsoft.com/](https://app.fabric.microsoft.com/,)
    then press the **Enter** button.

> <img src="./media/image6.png" style="width:6.5in;height:2.89653in"
> alt="A search engine window with a red box Description automatically generated with medium confidence" />

2.  In the **Microsoft Fabric** window, enter your given credentials,
    and click on the **Submit** button.

> <img src="./media/image7.png" style="width:6.49167in;height:3.11667in"
> alt="A close up of a white and green object Description automatically generated" />

3.  Then, In the **Microsoft** window enter the password and click on
    the **Sign in** button**.**

> <img src="./media/image8.png" style="width:4.50833in;height:3.83333in"
> alt="A login screen with a red box and blue text Description automatically generated" />

4.  In **Stay signed in?** window, click on the **Yes** button.

> <img src="./media/image9.png" style="width:4.58333in;height:3.66667in"
> alt="A screenshot of a computer error Description automatically generated" />

5.  You’ll be directed to Power BI Home page.

<img src="./media/image10.png" style="width:7.17276in;height:3.31357in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Start the Microsoft Fabric trial

Follow these steps to start your Fabric trial.

1.  On **Power BI Home** page, click on the **Account manager** on the
    right side. In the Account manager blade, navigate and
    select **Start trial as shown in the below image.**

<img src="./media/image11.png" style="width:7.22864in;height:1.92083in"
alt="A screenshot of a computer Description automatically generated" />

2.  If prompted, agree to the terms and then select **Start trial**.

> <img src="./media/image12.png" style="width:6.5in;height:1.64167in"
> alt="A screenshot of a computer Description automatically generated" />

3.  Once your trial capacity is ready, you receive a confirmation
    message. Select **Fabric Home Page** to begin working in Fabric.

> <img src="./media/image13.png" style="width:5.54167in;height:2.01667in"
> alt="A screenshot of a computer Description automatically generated" />

4.  Open your Account manager again. Notice that you now have a heading
    for **Trial status**. Your Account manager keeps track of the number
    of days remaining in your trial. You will also see the countdown in
    your Fabric menu bar when you work in a product experience.

> <img src="./media/image14.png" style="width:6.5in;height:3.80833in"
> alt="A screenshot of a computer Description automatically generated" />

## Task 3: Create a workspace 

Before working with data in Fabric, create a workspace with the Fabric
trial enabled.

1.  In the **Microsoft Fabric** home page, select the **Power BI**
    template.

> <img src="./media/image15.png" style="width:6.49167in;height:4.20833in"
> alt="A screenshot of a computer Description automatically generated" />

2.  In the **Power BI Home** page menu bar on the left,
    select **Workspaces** (the icon looks similar to 🗇).

> <img src="./media/image16.png" style="width:6.5in;height:6.23333in"
> alt="A screenshot of a computer Description automatically generated" />

3.  In the Workspaces pane, select **+** **New workspace**.

> <img src="./media/image17.png" style="width:3.32126in;height:6.1625in"
> alt="A screenshot of a computer Description automatically generated" />

4.  In the **Create a workspace tab**, enter the following details and
    click on the **Apply** button.

| **Name** | ***+++Fabric Real-Time AnalyticsXX+++**(*XX can be a unique number) |
|----|----|
| **Advanced** | Under **License mode**, select **Trial** |
| **Default storage format** | **Small dataset storage format** |

> <img src="./media/image18.png"
> style="width:5.3375in;height:6.94583in" />

<img src="./media/image19.png" style="width:5.25417in;height:6.23887in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image20.png" style="width:4.02917in;height:4.92138in"
alt="A screenshot of a computer Description automatically generated" />

5.  Wait for the deployment to complete. It takes 2-3 minutes to
    complete. When your new workspace opens, it should be empty.

> <img src="./media/image21.png" style="width:5.1799in;height:4.4328in"
> alt="A screenshot of a computer Description automatically generated" />

## **Task 4:** [**Create a KQL Database**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-1-create-a-kql-database)

Kusto query language (KQL) is used to query static or streaming data in
a table that is defined in a KQL database. To analyze the sales data,
you must create a table in a KQL database and ingest the data from the
file.

1.  At the bottom left of the Power BI portal, select the **Power
    BI** icon and switch to the **Real-Time Analytics** experience.

> <img src="./media/image22.png" style="width:3.50833in;height:7.5in" />
>
> <img src="./media/image23.png" style="width:6.49167in;height:5.15in" />

2.  In the **New Eventhouse** dialog box, enter the **Eventhouse name**
    as **FabricRTA** (or new database with a name of your choice) and
    click on **Create** button.

**Note**: Remove any extra space before and after **FabricRTA.**

<img src="./media/image24.png" style="width:3.50833in;height:2.25in" />

<img src="./media/image25.png" style="width:6.5in;height:3.97222in"
alt="A screenshot of a computer Description automatically generated" />

## **Task 5: Create a KQL queryset**

The KQL Queryset exists within the context of a workspace. A new KQL
queryset is always associated with the workspace you're using when you
create it.

1.  Select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane.

<img src="./media/image26.png"
style="width:6.21667in;height:5.65833in" />

2.  On the **Synapse Real-Time Analytics Home** page, from the menu bar,
    select the **New** drop-down, and from there select **KQL Queryset**

<img src="./media/image27.png"
style="width:5.06667in;height:6.31667in" />

3.  In the **New KQL Queryset** dialog box, enter the **KQL Query name**
    as +++**querysetXX+++**(or new database with a name of your choice)
    and click on **Create** button.

Note: KQL Query name should be alphanumeric characters, underscores,
periods, and hyphens. Special characters aren't supported.

<img src="./media/image28.png" style="width:3.225in;height:1.975in" />

3.  In **OneLake data hub** pane ,select **FabricRTA** KQL Database and
    click on **Select** button.

<img src="./media/image29.png"
style="width:7.03378in;height:4.27083in" />

<img src="./media/image30.png" style="width:6.5in;height:4.02361in" />

4.  Now you can write a simple KQL query:

> <span class="mark">print "Hello World"</span>

<img src="./media/image31.png"
style="width:6.49167in;height:3.2875in" />

5.  Highlight the line with the code and hit the **Run** button. The
    query will be executed and its result can be seen in the result grid
    at the bottom of the page.

<img src="./media/image31.png"
style="width:6.8875in;height:4.20854in" />

# **Exercise 2: Ingest data from Azure Storage Account**

Data ingestion to KQL Database is the process used to load data records
from one or more sources into a table. Once ingested, the data becomes
available for query.

KQL Database supports several ingestion methods, including Eventstream,
Fabric Pipeline, and Fabric Dataflow. Also available with Azure Data
Factory and Event Hubs.

## **Task 1: Create the raw table - logsRaw**

Ingest data using one-click ingestion from Azure Blob Storage to your
KQL Database.

1.  In KQL queryset pane, replace all the code in the **cell** with the
    following code and click on **▷ Run** button to create a table

**Copy**

.create table logsRaw(

Timestamp:datetime,

Source:string,

Node:string,

Level:string,

Component:string,

ClientRequestId:string,

Message:string,

Properties:dynamic

)

<img src="./media/image32.png"
style="width:7.16724in;height:3.84583in" />

2.  Click on refresh to see the new table on the left.

<img src="./media/image33.png" style="width:6.5in;height:4.33333in" />

<img src="./media/image34.png" style="width:6.5in;height:5.225in" />

## **Task 2: Use the “One-click” User Interface to ingest data from Azure blob storage**

You need to analyze the system logs for Contoso, which are stored in
Azure blob storage.

1.  Select **FabricRTA** d*atabase* in the left navigation pane.

<img src="./media/image35.png" style="width:5.2in;height:6.28333in" />

<img src="./media/image36.png" style="width:6.5in;height:6.24167in" />

2.  In **FabricRTA** database, under the **Home** tab, navigate and
    click on **Get data** in the command bar, then select **Azure
    Storage**.

<img src="./media/image37.png" style="width:6.5in;height:4.55833in" />

3.  Then use the wizard to import the data into a new table by selecting
    the following options:

4.  In the **Get data** tab, select the existing table as **logsRaw**.
    Under the Configure the data source tab enter the **URI :**
    +++<https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014>+++

<img src="./media/image38.png"
style="width:6.97611in;height:4.2125in" />

5.  In the **Get data** tab, click on the **+** and click on **Next**
    button.

<img src="./media/image39.png"
style="width:6.49167in;height:3.95833in" />

6.  In Get data tab under the Inspect the data click on the horizontal
    **ellipses (…)** beside Edit columns, select the **Advanced** and
    check the **Keep table schema.** Click on the **Finish** button**.**

<img src="./media/image40.png"
style="width:7.00659in;height:3.74583in" />

7.  Wait for the Data ingestion to be completed, and click **Close**.

<img src="./media/image41.png"
style="width:7.02917in;height:4.34876in" />

<img src="./media/image42.png"
style="width:7.36038in;height:3.44978in" />

8.  Select **querysetXX** KQL queryset in the left navigation pane.

<img src="./media/image43.png" style="width:6.5in;height:5.3in" />

9.  Replace all the code in the **cell** with the following code and
    click on **▷ Run** button

Copy

<span class="mark">logsRaw</span>

<span class="mark">\| count</span>

<img src="./media/image44.png"
style="width:6.49167in;height:3.51667in" />

The logsRaw table should have 3,834,012 records.

# **Exercise 3: Starting with the basics of KQL**

A *Kusto query* is a read-only request to process data and return
results. The request is stated in plain text that's easy to read,
author, and automate. A Kusto query has one or more query statements and
returns data in a tabular or graph format.

## **Task 1: Journey from SQL to KQL!**

1.  For all the SQL pros out there, KQL Database allows a subset of TSQL
    queries. Try running the following SQL query in web UI. Replace all
    the code in the **cell** with the following code and click on **▷
    Run** button.

**Copy**

<span class="mark">SELECT COUNT() FROM logsRaw</span>

<img src="./media/image45.png"
style="width:6.49167in;height:3.00833in" />

2.  The primary language to interact with Kusto is KQL (Kusto Query
    Language). To make the transition and learning experience easier,
    you can use the explain operator to translate SQL queries to KQL.

3.  Replace all the code in the **cell** with the following code and
    click on **▷ Run** button.

**Copy**

<span class="mark">explain SELECT MAX(Timestamp) AS MaxTimestamp FROM
logsRaw WHERE Level='Error'</span>

<img src="./media/image46.png"
style="width:6.80417in;height:3.63355in" />

Output of the above query will be a corresponding KQL query

logsRaw

\| where (Level == "Error")

\| summarize MaxTimestamp=max(Timestamp)

\| project MaxTimestamp

## **Task 2: Basic KQL queries - explore the data**

In this task, you will see some KQL examples. For this task, we will use
the table logsRaw, which has data we loaded in previous challenge from
storage account.

1.  Execute the queries and view the results. KQL queries can be used to
    filter data and return specific information. Now, you'll learn how
    to choose specific rows of data. The **where** operator filters
    results that satisfy a certain condition.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run** button.

> **Copy**

<span class="mark">logsRaw</span>

<span class="mark"> \| where Level=="Error"</span>

<span class="mark">\| take 10</span>

<img src="./media/image47.png"
style="width:7.26042in;height:3.1875in" />

The take operator samples any number of records from our table without
any order. In the above example, we asked to provide 10 random records.

3.  Find out how many records are in the table. Replace all the code in
    the **cell** with the following code and click on **▷ Run** button.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| summarize count() // or: count</span>

<img src="./media/image48.png"
style="width:6.49167in;height:3.20833in" />

4.  Find out the minimum and maximum Timestamp. Replace all the code in
    the **cell** with the following code and click on **▷ Run** button.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| summarize min(Timestamp), max(Timestamp)</span>

<img src="./media/image49.png"
style="width:7.14892in;height:2.94583in" />

5.  Our dataset has trace records written by Contoso's DOWNLOADER
    program, which downloads files from blob storage as part of its
    business operations. Replace all the code in the **cell** with the
    following code and click on **▷ Run** button.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| where Component == "DOWNLOADER"</span>

<span class="mark">\| take 10</span>

<img src="./media/image50.png"
style="width:7.08091in;height:2.95417in" />

6.  Select the **Properties** column is dynamic. The dynamic data type
    is special in that it can take on any value of other data types, as
    well as arrays and property bags (dictionaries).

<img src="./media/image51.png"
style="width:6.49167in;height:2.38333in" />

7.  The dynamic type is extremely beneficial when it comes to storing
    JSON data, since KQL makes it simple to access fields in JSON and
    treat them like an independent column: just use either the dot
    notation (dict.key) or the bracket notation (dict\["key"\]).

8.  The extend operator adds a new calculated column to the result set,
    during query time. This allows for the creation of new standalone
    columns to the result set, from the JSON data in dynamic columns.
    Replace all the code in the **cell** with the following code and
    click on **▷ Run** button.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| where Component == "DOWNLOADER"</span>

<span class="mark">\| take 100</span>

<span class="mark">\| extend originalSize=Properties.OriginalSize,
compressedSize=Properties.compressedSize</span>

<img src="./media/image52.png"
style="width:7.42495in;height:3.74583in" />

## [Task 3: Explore the table and columns](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-2-explore-the-table-and-columns-)

After subscripting a dynamic object, it is necessary to cast (convert)
the value to a simple type in order to utilize them (for example, if you
want to summarize the sizes of all the OriginalSize, you should convert
the dynamic type to a numeric type, like long).

1.  This below query to get the table that is shown in the image below
    (we want to convert the OriginalSize and CompressedSize columns to
    long)

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

> logsRaw
>
> \| extend originalSize=Properties.OriginalSize,
> compressedSize=Properties.compressedSize
>
> \| getschema

<img src="./media/image53.png"
style="width:7.14583in;height:4.51653in" />

## [**Task 4: Keep the columns of your interest**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-3-keep-the-columns-of-your-interest-) 

You are investigating an incident and wish to review only several
columns of the dataset.

1.  A query to get only specific desired columns: Timestamp,
    ClientRequestId, Level, Message. Take arbitrary 10 records.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

> logsRaw
>
> \| project Timestamp, ClientRequestId, Level, Message
>
> \| take 10

<img src="./media/image54.png"
style="width:7.38897in;height:3.22083in" />

## [Task 5: Filter the output](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-4-filter-the-output-)

You are investigating an incident that occurred within a specific time
frame.

1.  Write a query to get only specific desired
    columns: Timestamp, ClientRequestId, Level, Message. Take all the
    records between 2014-03-08 01:00 and 2014-03-08 10:00.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**
>
> logsRaw
>
> \| where Timestamp between (datetime(2014-03-08 01:00)..
> datetime(2014-03-08 10:00))
>
> \| project Timestamp, ClientRequestId, Level, Message
>
> \| take 10

<img src="./media/image55.png"
style="width:7.38402in;height:3.1375in" />

## [Task 6: Sorting the results](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-5-sorting-the-results-)

Your system generated an alert indicating a significant decrease in
incoming data. You want to check the traces of the "INGESTOR_EXECUTER"
\[sic\] component of the program.

1.  Write a query that returns 20 sample records in which
    the Component column equals the word "INGESTOR_EXECUTER" \[sic\].

2.  Once done, rewrite the query to take the top 1 records by the value
    of rowCount (for the "INGESTOR_EXECUTER" \[sic\] records).

3.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

> <span class="mark">logsRaw</span>
>
> <span class="mark">\| where Component == 'INGESTOR_EXECUTER'</span>
>
> <span class="mark">\| extend
> rowCount=toint(Properties.rowCount)</span>
>
> <span class="mark">\| where isnotempty(rowCount)</span>
>
> <span class="mark">\| sort by rowCount</span>
>
> <span class="mark">\| top 10 by rowCount desc</span>

<img src="./media/image56.png"
style="width:7.34766in;height:3.4375in" />

## [Task 7: Data profiling](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-6-data-profiling-)

1.  As part of the incident investigation, you want to
    extract format and rowCount from INGESTOR_EXECUTER \[sic\]
    component. Rename the calculated fields to fileFormat and rowCount
    respectively. Also, Make Sure Timestamp, fileFormat and rowCount are
    the first 3 columns.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**
>
> <span class="mark">logsRaw</span>
>
> <span class="mark">\| where Component == 'INGESTOR_EXECUTER'</span>
>
> <span class="mark">\| extend rowCount=toint(Properties.rowCount),
> fileFormat=tostring(Properties.format)</span>
>
> <span class="mark">\| project Timestamp, fileFormat, rowCount,
> ClientRequestId, Component, Level, Message</span>
>
> <span class="mark">\| take 10</span>

<img src="./media/image57.png"
style="width:7.38945in;height:3.12083in" />

## [**Task 8: Total number of records**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-7-total-number-of-records-)

The system comprises of several "components", but you don't know their
names or how many records were generated by each.

1.  Write a query to find out how many records were generated by each
    component. Use the Component column.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**
>
> <span class="mark">logsRaw</span>
>
> <span class="mark">\| summarize count() by Component</span>
>
> <img src="./media/image58.png" style="width:6.5in;height:7.29167in"
> alt="A screenshot of a computer Description automatically generated" />

## [**Task 9: Aggregations and string operations **](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-8-aggregations-and-string-operations-)

You assume that the incident being investigated has a connection to the
ingestion process run by Contoso's program.

1.  Write a query to find out how many records contain the
    string '**ingestion'** in the Message column. Aggregate the results
    by **Level.**

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

> <span class="mark">logsRaw</span>
>
> <span class="mark">\| where Message has "ingestion"</span>
>
> <span class="mark">\| summarize count() by Level</span>

<img src="./media/image59.png"
style="width:6.49167in;height:4.15833in" />

## [**Task 10: Render a chart **](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-9-render-a-chart-)

1.  Write a query to find out how many total records are present
    per Level (aggregated by Level) and render a piechart.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**
>
> <span class="mark">logsRaw</span>
>
> <span class="mark">\| summarize count() by Level</span>
>
> <span class="mark">\| render piechart</span>
>
> <img src="./media/image60.png"
> style="width:6.49167in;height:3.80833in" />

## [**Task 11: Create bins and visualize time series **](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-3-task-10-create-bins-and-visualize-time-series-)

1.  Write a query to show a timechart of the number of records in 30
    minute bins (buckets). Each point on the timechart represent the
    number of logs in that bucket.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**
>
> <span class="mark">logsRaw</span>
>
> <span class="mark">\| summarize count() by bin(Timestamp, 30m)</span>
>
> <span class="mark">\| render timechart</span>

<img src="./media/image61.png" style="width:6.49167in;height:3.15in" />

# [Exercise 4: Explore and Transform Data](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-4-explore-and-transform-data)

In this exercise we will explore 3 capabilities of Data Explorer

- **User-defined functions** are reusable KQL subqueries that can be
  defined as part of the query itself (ad-hoc functions), or persisted
  as part of the database metadata (stored functions - reusable KQL
  query, with the given name). Stored functions are invoked through a
  name, are provided with zero or more input arguments (which can be
  scalar or tabular), and produce a single value (which can be scalar or
  tabular) based on the function body.

- **Update Policy** is like an internal ETL. It can help you manipulate
  or enrich the data as it gets ingested into the source table (e.g.
  extracting JSON into separate columns, creating a new calculated
  column, joining the newly ingested records with a static dimension
  table that is already in your database, etc). For these cases, using
  an update policy is a very common and powerful practice.

> Each time records get ingested into the source table, the update
> policy's query (which we'll define in the update policy) will run on
> them (**only on newly ingested records** - other existing records in
> the source table aren’t visible to the update policy when it runs),
> and the results of the query will be appended to the target table.
> This function's output schema and target table schema should exactly
> match.

## [**Task 1: User defined Function (Stored Functions)**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-4-task-1-user-defined-function-stored-functions-)

1.  Create a stored functions, named ManiputatelogsRaw, that will
    contain the code below. Make sure the function works.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| where Component in (</span>

<span class="mark">  'INGESTOR_EXECUTER',</span>

<span class="mark">  'INGESTOR_GATEWAY',</span>

<span class="mark">  'INTEGRATIONDATABASE',</span>

<span class="mark">  'INTEGRATIONSERVICEFLOWS',</span>

<span class="mark">  'INTEGRATIONSERVICETRACE')</span>

<img src="./media/image62.png"
style="width:7.41531in;height:3.8125in" />

## [**Task 2: Create an update policy**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab1.md#challenge-4-task-2-create-an-update-policy-)

In this task, we will use an update policy to filter the raw data in
the logsRaw table (the source table) for ingestion logs, that will be
ingested into the new table ingestionLogs that we’ll create.

1.  Build the target table,replace all the code in the **cell** with the
    following code and click on **▷ Run cell** button and review the
    output.

**Copy**

.<span class="mark">create table ingestionLogs (</span>

<span class="mark">Timestamp: datetime,</span>

<span class="mark">Source: string,</span>

<span class="mark">Node: string,</span>

<span class="mark">Level: string,</span>

<span class="mark">Component: string,</span>

<span class="mark">ClientRequestId: string,</span>

<span class="mark">Message: string,</span>

<span class="mark">Properties: dynamic)</span>

<img src="./media/image63.png"
style="width:7.07581in;height:2.6625in" />

2.  Create a function for the update policy, replace all the code in the
    **cell** with the following code and click on **▷ Run cell** button
    and review the output.

**Copy**

> <span class="mark">.create function ingestionComponents(){</span>
>
> <span class="mark">logsRaw</span>
>
> <span class="mark">\| where Component has_any ('INGESTOR_EXECUTER',
> 'INGESTOR_GATEWAY', 'INTEGRATIONDATABASE','INTEGRATIONSERVICEFLOWS',
> 'INTEGRATIONSERVICETRACE', 'DOWNLOADER')</span>
>
> <span class="mark">}</span>

<img src="./media/image64.png"
style="width:7.35349in;height:2.7375in" />

3.  Create the update policy(Fill in the blanks), replace all the code
    in the **cell** with the following code and click on **▷ Run
    cell** button and review the output.

**Copy**

> <span class="mark">.alter table ingestionLogs</span>
>
> <span class="mark">    policy update  @'\[{ "IsEnabled":
> true,"Source": "logsRaw", "Query": "ingestionComponents()",
> "IsTransactional": true, "PropagateIngestionProperties": false}
> \]'</span>

<img src="./media/image65.png"
style="width:7.15349in;height:1.9125in" />

4.  Update policy can transform and move the data from source table from
    the time it is created. It cannot look back at already existing data
    in source table. We will ingest new data into logsraw table and see
    new data flowing into ingestionLogs table

5.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

6.  copy **operationIds** and paste them in a notepad , and then
    **Save** the notepad to use the information in the upcoming step.

**Copy**

// Note, the following .ingest commands set creationTime to 2014 as you
may notice in the file path.

// This param allows to backfill the table with historical data and
index it according ot the creationTime setting.

.execute database script \<\|

.ingest async into table logsRaw
(h'https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014/03/08/00/data.csv.gz')
with (format='csv', creationTime='2024-03-08T00:00:00Z');

.ingest async into table logsRaw
(h'https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014/03/08/01/data.csv.gz')
with (format='csv', creationTime='2024-03-08T01:00:00Z');

.ingest async into table logsRaw
(h'https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014/03/08/02/data.csv.gz')
with (format='csv', creationTime='2024-03-08T02:00:00Z');

.ingest async into table logsRaw
(h'https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014/03/08/03/data.csv.gz')
with (format='csv', creationTime='2024-03-08T03:00:00Z');

.ingest async into table logsRaw
(h'https://adxsamplefiles.blob.core.windows.net/publiccsvsamples/logsbenchmark-onegb/2014/03/08/04/data.csv.gz')
with (format='csv', creationTime='2024-03-08T04:00:00Z');

<img src="./media/image66.png" style="width:6.49167in;height:3.575in" />

**Note:** The above command does not complete immediately. Because we
used the async parameter, the output of the above query will
be operationIds. The progress of the query can be checked by used the
below command

7.  Check progress of the commands, replace all the code in the **cell**
    with the following code and click on **▷ Run cell** button and
    review the output.

Copy

.show operations

\| where OperationId == ' OperationId '

**Note:** Replace the operationIds which you have saved in Step 6

<img src="./media/image67.png"
style="width:6.49167in;height:2.73333in" />

8.  Make sure the data is transformed correctly in the destination
    table. Replace all the code in the **cell** with the following code
    and click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">ingestionLogs</span>

<span class="mark">\| count</span>

<img src="./media/image68.png"
style="width:6.06667in;height:3.96667in" />

Check if the count of ingestionLogs table is 111,740.

**Note:** If the count is not matching for ingestionLogs table, it means
that one of the above .ingest commands have throttled or failed.

9.  Please run the following command to clean **ingestionLogs** table.
    Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">.clear table ingestionLogs data</span>

<img src="./media/image69.png" style="width:6.5in;height:4.79167in" />

10. Now then run the above **.ingest** commands one by one and this will
    result in 93,648 count in ingestionLogs table.

11. Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

> **Copy**

<span class="mark">.alter table ingestionLogs policy update</span>

<span class="mark">@'\[{ "IsEnabled": true, "Source": "logsRaw",
"Query": "ingestionComponents()"}\]'</span>

<span class="mark">.set-or-append logsRaw \<\| logsRaw \| take
100000</span>

<span class="mark">ingestionLogs \| take 10</span>

<img src="./media/image70.png"
style="width:7.38566in;height:3.99583in" />

#  [**Exercise 5: Advanced KQL, Policies, and Visualization**](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab2.md#lab-2-advanced-kql-policies-and-visualization)

#  [Caching and Retention Policies](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab2.md#challenge-5-how-long-will-my-data-be-kept---caching-and-retention-policies)

Among the different policies you can set to the KQL Database, two
policies are of particular importance:

- Retention policy (retention period)

- Cache policy (cache period)

First, a policy is used to enforce and control the properties of the KQL
Database.

- The **retention** policy: the time span, in days, for which it’s
  guaranteed that the data is kept available for querying. The time span
  is measured from the time that the records are ingested. When the
  period expires, the records will not be available for querying any
  more.

> In other words, the retention policy defines the period during which
> data is retained and available to query, measured since ingestion
> time. Note that a large retention period may impact the cost.

- The **cache** policy: the time span, in days, for which to keep
  recently ingested data (which is usually the frequently queried data)
  available in the hot cache rather than in long term storage (this is
  also known as cold tier. Specifically, it is Azure blob storage). Data
  stored in the hot cache is actually stored in local SSD or the RAM of
  the machine, very close to the compute nodes.

> Therefore, more readily available for querying. The availability of
> data in the hot cache improves query performance but can potentially
> increase the cluster cost (as more data is being stored, more VMs are
> required to store it). In other words, the caching policy defines the
> period during which data is kept in the hot cache.
>
> All the data is always persisted in the cold tier, for the duration
> defined in the retention policy. Any data whose age falls within the
> hot cache policy will also be stored in the hot cache. If you query
> data from cold cache, it’s recommended to target a small specific
> range in time (“point in time”) for the queries to be efficient.

## [**Task 1: Change the retention policy via commands **](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab2.md#challenge-5-task-1-change-the-retention-policy-via-commands-)

Database policies can be overridden per table using a KQL control
command. A database cache and retention policies can be edited from the
Fabric UI. Tables policies can be managed via a KQL command.

1.  Select ***querysetXX*** KQL queryset in the left navigation pane.

<img src="./media/image71.png"
style="width:5.22083in;height:5.72953in" />

2.  Always use KQL commands to alter the policies of the entire
    Database/tables. Table level policy takes precedence over database
    level which takes precedence over cluster level.

3.  Run the below query to find the total hours is the retention policy
    of **ingestionLogs** table

4.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

Copy

<span class="mark">.alter table ingestionLogs policy retention</span>

<span class="mark">\`\`\`</span>

<span class="mark">{</span>

<span class="mark">"SoftDeletePeriod": "10:12:00:00",</span>

<span class="mark">"Recoverability": "Enabled"</span>

<span class="mark">}</span>

<span class="mark">\`\`\`</span>

<img src="./media/image72.png"
style="width:7.07353in;height:2.62083in" />

# [Exercise 6: Metadata objects handling using Control Commands](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab2.md#challenge-6-metadata-objects-handling-using-control-commands)

## **Task 1: .show/diagnostic logs/Insights**

Control commands are requests to the service to retrieve information
that is not necessarily data in the database tables, or to modify the
service state, etc. In addition, they can be used to manage Azure Data
Explorer. The first character of the KQL text determines if the request
is a control command or a query. Control commands must start with the
dot (.) character, and no query may start with that character.

- The .show queries command returns a list of queries that have reached
  a final state, and that the user invoking the command has access to
  see.

- The .show commands command returns a table of the admin commands that
  have reached a final state. The TotalCpu column is the value of the
  total CPU clock time (User mode + Kernel mode) consumed by this
  command.

- The .show journal command returns a table that contains information
  about metadata operations that are done on the Azure Data Explorer
  database. The metadata operations can result from a control command
  that a user executed, or internal control commands that the system
  executed, such as drop extents by retention

- The .show tables details command returns a set that contains the
  specified table or all tables in the database with a detailed summary
  of each table's properties.

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

**Copy**

> <span class="mark">// 1. Using pipe: Count how many tables are in the
> database-in-scope:</span>
>
> <span class="mark">.show tables</span>
>
> <span class="mark">\| count</span>
>
> <span class="mark">// 2. Using semicolon: Count how many tables are in
> the database-in-scope:</span>
>
> <span class="mark">.show tables;</span>
>
> <span class="mark">\$command_results</span>
>
> <span class="mark">\| count</span>
>
> <span class="mark">// 3. Using semicolon, and including a let
> statement:</span>
>
> <span class="mark">.show tables;</span>
>
> <span class="mark">let
> useless=(n:string){strcat(n,'-','useless')};</span>
>
> <span class="mark">\$command_results \| extend
> LastColumn=useless(TableName)</span>

<img src="./media/image73.png"
style="width:7.29961in;height:4.32917in" />

## **Task 2: Use .show queries**

As part of an incident investigation, you need to find out how many
queries were executed in the past 3 hours.

1.  To count the number of queries that were run, in the past 3 hours
    execute the below query

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

**Copy**

> <span class="mark">.show queries</span>
>
> <span class="mark">\| where StartedOn \>ago(7d)</span>
>
> <span class="mark">\| summarize count() by User</span>

<img src="./media/image74.png"
style="width:6.95765in;height:4.37083in" />

## **Task 3: Use .journal commands**

1.  To show the details of the function that you created earlier,
    replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

**Copy**

> <span class="mark">.show journal</span>
>
> <span class="mark">\| where EventTimestamp \> ago(1h) and Event
> =='ADD-FUNCTION'</span>
>
> <span class="mark">\| project Event, EventTimestamp,
> ChangeCommand</span>

<img src="./media/image75.png"
style="width:7.45087in;height:2.1875in" />

## **Task 4: Use .show commands**

1.  To show the details of commands that you ran, in the past 4 hours,
    replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">.show commands</span>

<span class="mark">\| where StartedOn \>ago(4h)</span>

<span class="mark">\| summarize count() by User</span>

<img src="./media/image76.png"
style="width:6.86402in;height:4.07083in" />

## **Task 5: Table details and size**

1.  Use control command to show details on **ingestionLogs** tables in
    the database.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

Copy

<span class="mark">.show extents</span>

<span class="mark">\| summarize format_bytes(sum(OriginalSize),2,'MB'),
format_bytes(sum(ExtentSize),2,'MB') by TableName</span>

<span class="mark">.show tables details</span>

<img src="./media/image77.png"
style="width:7.25326in;height:3.57083in" />

# [Exercise 7: Going more advanced with KQL](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/Lab2.md#challenge-7-going-more-advanced-with-kql)

## **Task 1: Declaring variables and using 'let' statements**

You can use the let statement to set a variable name equal to an
expression or a function, or to create views (virtual, temporary, tables
based on the result-set of another KQL query).

let statements are useful for:

- Breaking up a complex expression into multiple parts, each represented
  by a variable.

- Defining constants outside of the query body for readability.

- Defining a variable once and using it multiple times within a query.

1.  Use 2 let statements to
    create **LogType** and **TimeBucket **variables with the following
    values:

- LogType = 'Warning'

- TimeBucket = 1m

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">let LogType = 'Warning';</span>

<span class="mark">let TimeBucket = 1m;</span>

<span class="mark">logsRaw</span>

<span class="mark">\| summarize count() by Level = LogType,
bin(Timestamp,TimeBucket)</span>

<span class="mark">\| render timechart</span>

<img src="./media/image78.png"
style="width:6.49167in;height:3.60833in" />

## **Task 2: Use the search operator**

You received an alert early in the morning regarding multiple Timeouts
in your system. You want to quickly search the traces without using
specific columns or table names.

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

Copy

<span class="mark">search "Exception=System.Timeout" \| count</span>

<img src="./media/image79.png"
style="width:6.8375in;height:4.40619in" />

## **Task 3: Parse Key-Value pairs strings into separate columns**

As part of an incident investigation, you need to look at
the **INGESTOR_GATEWAY** records (Component == 'INGESTOR_GATEWAY').You
need to use the *Message* column, which contains the message of the
trace, representing the information in a key/value form.

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

Copy

<span class="mark">logsRaw</span>

<span class="mark">\| where Component == "INGESTOR_EXECUTER"</span>

<span class="mark">//\| parse-kv Properties as (size: int, format:
string, rowCount: int, cpuTime: string , duration: string) //bug:
cpuTime, duration truncated</span>

<span class="mark">\| take 20</span>

<span class="mark">\| evaluate bag_unpack(Properties)</span>

<span class="mark">ingestionLogs</span>

<span class="mark">\| where Component == "INGESTOR_EXECUTER"</span>

<span class="mark">\| take 20</span>

<span class="mark">\| parse-kv Message as
(IngestionCompletionEvent:string, path:string) with (pair_delimiter='
file', kv_delimiter=':')</span>

<img src="./media/image80.png"
style="width:7.42733in;height:3.57083in" />

## **Task 4: Nulls are important in timeseries analysis (Compare summarize and make-series)**

In this task, calculate the average size of data ingested per 30 min by
the node 'Engine000000000378'. Use Component as 'INGESTOR_EXECUTER'.
File size is available in the 'Properties' column. Render it as a
timechart.

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">logsRaw</span>

<span class="mark">\| where Component=='INGESTOR_EXECUTER' and Node
=='Engine000000000378'</span>

<span class="mark">\| extend size=tolong(Properties.size)</span>

<span class="mark">\| summarize avg(size) by bin(Timestamp, 1h),
Node</span>

<span class="mark">\| render timechart</span>

<span class="mark">logsRaw</span>

<span class="mark">\| where Component=='INGESTOR_EXECUTER' and Node
=='Engine000000000378'</span>

<span class="mark">\| extend size=tolong(Properties.size)</span>

<span class="mark">\| make-series avg(size) default=0 on Timestamp step
1h by Node</span>

<span class="mark">\| render timechart</span>

<img src="./media/image81.png"
style="width:7.29748in;height:4.0375in" />

## **Task 5: Anomaly detection**

Anomaly detection lets you find outliers/anomalies in the data.

Let's find out any file size anomalies by summarizing the average of
file sizes in 1-minute intervals.

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">ingestionLogs</span>

<span class="mark">\| extend size=tolong(Properties.size)</span>

<span class="mark">\| make-series avg(size) default=0 on Timestamp step
10m</span>

<span class="mark">\| extend
anom=series_decompose_anomalies(avg_size)</span>

<span class="mark">\| render anomalychart with
(anomalycolumns=anom)</span>

<span class="mark">ingestionLogs</span>

<span class="mark">\| extend size=tolong(Properties.size)</span>

<span class="mark">\| make-series avg(size) default=0 on Timestamp step
10m</span>

<span class="mark">\| extend anom=series_decompose_anomalies(avg_size,
0.5)</span>

<span class="mark">\| mv-expand Timestamp, avg_size, anom</span>

<span class="mark">\| where anom \<\> 0</span>

<img src="./media/image82.png" style="width:6.49167in;height:5.375in" />

2.  To get a tabular format of the detected anomalies, you can use
    the mv-expand operator to expand the multi-value dynamic array of
    the anomaly detection component (AnomalyFlags, AnomalyScore,
    PredictedUsage) into multiple match records, and then filter by
    positive and negative deviations from expected usage (where
    AnomalyFlags != 0).

3.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">ingestionLogs</span>

<span class="mark">\| where Component == "INGESTOR_EXECUTER"</span>

<span class="mark">\| extend fileSize=tolong(Properties.size)</span>

<span class="mark">\| make-series ActualSize=avg(fileSize) on Timestamp
step 1min // Creates the time series, listed by data type</span>

<span class="mark">\| extend(AnomalyFlags, AnomalyScore, PredictedSize)
= series_decompose_anomalies(ActualSize, -1) // Scores and extracts
anomalies based on the output of make-series</span>

<span class="mark">\| mv-expand ActualSize to typeof(double), Timestamp
to typeof(datetime), AnomalyFlags to typeof(double),AnomalyScore to
typeof(double), PredictedSize to typeof(long) // Expands the array
created by series_decompose_anomalies()</span>

<span class="mark">\| where AnomalyFlags != 0 // Returns all positive
and negative deviations from expected usage</span>

<span class="mark">\| project Timestamp,ActualSize =
format_bytes(ActualSize, 2),PredictedSize = format_bytes(PredictedSize,
2), AnomalyScore, AnomalyFlags // Defines which columns to return</span>

<span class="mark">\| sort by abs(AnomalyScore) desc // Sorts results by
anomaly score in descending ordering</span>

<img src="./media/image83.png"
style="width:7.3511in;height:3.67083in" />

# **Exercise 8: Visualization**

## **Task 1: Find the anomaly value**

1.  Create a Timechart using following query. Observe that we
    used \_startTime? and \_endTime. These 2 are parameters from
    TimeRange filter in ADX Dashboard with which we can filter the
    minimum and maximum time of our data.

2.  Use the below example query as reference to add Timestamp filter
    with \_startTime and \_endTime filter to queries in task 1 and task
    2.

3.  The following 2 tasks use the timefilter
    between 2014-03-08T00:00:00 and 2014-03-08T10:00:00

> <span class="mark">ingestionLogs</span>
>
> <span class="mark">\| where Timestamp between (todatetime(\_startTime)
> .. todatetime(\_endTime))</span>
>
> <span class="mark">\| summarize count() by bin(Timestamp, 10m),
> Component</span>

4.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">ingestionLogs</span>

<span class="mark">\| where Timestamp between (datetime(2014-03-08
01:00).. datetime(2014-03-08 10:00))</span>

<span class="mark">\| summarize count() by bin(Timestamp, 10m),
Component</span>

<img src="./media/image85.png"
style="width:7.33296in;height:5.12083in" />

5.  Parameterize (add Timefilter) and render an Anomaly chart using the
    following Anomaly detection query. The chart should show values
    between 2014-03-08T00:00:00 and 2014-03-08T10:00:00.

6.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

ingestionLogs

\| where Timestamp between (datetime(2014-03-08 01:00)..
datetime(2014-03-08 10:00))

\| summarize count() by bin(Timestamp, 10m), Component

\| render timechart

<img src="./media/image86.png"
style="width:7.34008in;height:3.60417in" />

7.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

> let bucket=10m;
>
> logsRaw
>
> \| where Timestamp between(datetime(2014-03-08 01:00)..
> datetime(2014-03-08 10:00))
>
> \| make-series count() on Timestamp step bucket by Level
>
> \| extend anom = series_decompose_anomalies(count\_)
>
> \| render anomalychart with (anomalycolumns=anom)

<img src="./media/image87.png"
style="width:7.31434in;height:3.60417in" />

## **Task 2: Find the warning percentage**

1.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output.

**Copy**

<span class="mark">ingestionLogs</span>

<span class="mark">\| where Timestamp between(datetime(2014-03-08
01:00).. datetime(2014-03-08 10:00))</span>

<span class="mark">\| summarize count() by Level</span>

<span class="mark">\| render piechart</span>

<img src="./media/image88.png"
style="width:6.62083in;height:3.93855in" />

# Exercise 9: Eventstreams

## Task 1: Create Eventstream

1.  Select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane.

<img src="./media/image89.png"
style="width:3.97917in;height:3.85984in" />

2.  On the **Synapse Real-Time Analytics Home** page, from the menu bar,
    select the **New** drop-down, and from there select **Eventstream.**
    Name the Eventstream +++***RTA_EventStream**+++* and click on the
    **Create** button.

<img src="./media/image90.png" style="width:6.5in;height:6.7in" />

<img src="./media/image91.png" style="width:3.05in;height:2.55833in" />

<img src="./media/image92.png" style="width:6.5in;height:3.16944in" />

3.  On the Eventstream, select **New source** and select **Custom
    App*.***

<img src="./media/image93.png" style="width:6.5in;height:3.08333in" />

4.  On the **Custom App** configuration page, enter the source name as
    +++**RTAcustom+++** and click on **Add** button

<img src="./media/image94.png"
style="width:3.23333in;height:6.23333in" />

5.  On the **Eventstream** pane**,** select the **keys** under the
    Details ,copy the **connection strings-primarykey** and paste them
    on a notepad, as you need them in the upcoming task

<img src="./media/image95.png"
style="width:6.49167in;height:4.13333in" />

## Task 2: Import LogSteamSimulator notebook

1.  Now, select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane

<img src="./media/image96.png" style="width:6.49167in;height:3.925in" />

2.  On the **Synapse Real-Time Analytics Home** page, from the menu bar,
    select the **New** drop-down, and from there select **Import
    notebook.**

<img src="./media/image97.png" style="width:6.5in;height:6.575in" />

3.  Select **Upload** from the **Import status** pane that appears on
    the right side of the screen.

<img src="./media/image98.png" style="width:3.14167in;height:2.35in" />

4.  Navigate and select **LogSteamSimulator** notebooks from
    **C:\LabFiles**and click on the **Open** button.

<img src="./media/image99.png"
style="width:6.49167in;height:2.90833in" />

5.  You will see a notification stating **Imported successfully.**

6.  *Then*, select the ***LOgStreamSimulator* **notebook.

<img src="./media/image100.png"
style="width:7.25902in;height:3.42917in" />

7.  To start the notebook, in the 1<sup>st</sup> cell paste the
    **connection string of your custom app source**(the value that you
    have saved in your notepad in the **Exercise 9\>Task 1\>Step 5)**,
    select the **Run** icon that appears on the left side of the cell.

<img src="./media/image101.png" style="width:6.5in;height:3in" />

8.  To installs required library, select and run the 2<sup>nd</sup> ,
    3<sup>rd</sup> cells.

<img src="./media/image102.png"
style="width:7.25417in;height:4.17184in" />

<img src="./media/image103.png"
style="width:7.35486in;height:3.02917in" />

9.  To sends the data, select and run the 4<sup>th</sup> ,5<sup>th</sup>
    cells.

<img src="./media/image104.png"
style="width:6.49167in;height:4.15833in" />

**Note:**  If you get AnalysisException: \[PATH_NOT_FOUND\] Path does
not exist, replace code cell 1 with [backup URL
location](https://github.com/microsoft/FabricRTA-in-a-Day/blob/main/assets/blobURIbackup.md#lab-3) for SampleCsv,

ConnectionString = ''

SampleCsv =
'https://iotmonitoringsa26915.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/00/data.csv.gz?sp=r&st=2024-04-22T13:43:07Z&se=2099-04-22T21:43:07Z&spr=https&sv=2022-11-02&sr=b&sig=F6VFt7bUTBXJZVXTN1Ln9rSuzfaEmiq5dxxEQ1C%2F%2Bpk%3D'

Important Note: If either of the last code cells in step 4 fail with
error file does not exist, then **refresh the browser page** and
click **stop session** to get a new session (not reconnect to existing
session), then click **Run All** again.

Errors with **pip's depency resolver** are **okay**, proceeding code
cells should run successfully.

⚠️ If you get ImportError: cannot import name 'Buffer' from
'typing_extensions', replace code cell 2 with: pip install
azure-eventhub==5.11.5 --upgrade --force and click **Run All** again.

<img src="./media/image105.png"
style="width:7.37857in;height:4.30417in" />

10. Select ***RTA-Eventstream*** in the left navigation pane.

<img src="./media/image106.png"
style="width:3.00833in;height:6.425in" />

11. Open eventstream artifact, preview the data stream

<img src="./media/image107.png"
style="width:7.0875in;height:3.99411in" />

## Task 3: Send data from the Eventstream to the KQL database

1.  Our data should be arriving into our Eventstream, and we'll now
    configure the data to be ingested into the KQL database we created
    in the above task. On the Eventstream, click on ***New
    destination***, then navigate and click on **KQL Database**.

<img src="./media/image108.png"
style="width:7.25482in;height:3.12917in" />

2.  On the KQL settings, select ***Direct ingestion***. While we have
    the opportunity to process event data at this stage, for our
    purposes, we will ingest the data directly into the KQL database.
    Set the destination name to +++***RTA-Destination+++***, then select
    your **workspace** and KQL database created in the above task, then
    click on **Add and Configure** button.

<img src="./media/image109.png"
style="width:3.85833in;height:6.68333in" />

3.  On the first settings page, select **logsRaw** table and click on
    the **Next** button.

<img src="./media/image110.png"
style="width:7.22917in;height:4.18921in" />

4.  The next page allows us to inspect and configure the schema. Be sure
    to change the format from TXT to **JSON**, if necessary. The default
    columns of *symbol*, *price*, and *timestamp* should be formatted as
    shown in the below image; then click on the ***Finish*** button.

<img src="./media/image111.png"
style="width:7.03681in;height:4.11911in" />

5.  On the **Summary** page, if there are no errors, you’ll see a
    **green checkmark** as shown in the below image, then click on the
    ***Close*** button to complete the configuration.

<img src="./media/image112.png"
style="width:6.97857in;height:4.07083in" />

<img src="./media/image113.png" style="width:7.39346in;height:3.35628in"
alt="A screenshot of a computer Description automatically generated" />

6.  Now, select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane.

<img src="./media/image114.png"
style="width:4.44583in;height:4.82851in" />

7.  On the **Synapse Real-Time Analytics Home** page, Select **KQL
    Queryset.**

<img src="./media/image115.png"
style="width:7.38125in;height:4.34358in" />

8.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

logsRaw

\| take 10

<img src="./media/image116.png"
style="width:6.49167in;height:3.64167in" />

9.  Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

//rate

logsRaw

\| where ingestion_time() \> ago(1d)

\| make-series count() default=0 on ingestion_time() step 1m

\| render timechart

<img src="./media/image117.png"
style="width:7.30099in;height:3.57083in" />

10. Replace all the code in the **cell** with the following code and
    click on **▷ Run cell** button and review the output

//lag

logsRaw

\| where ingestion_time() \>ago(1m)

\| summarize m=max(ingestion_time())

\| project lag=now()-m

<img src="./media/image118.png"
style="width:7.29903in;height:3.3875in" />

# Exercise 10: Data Activator

## Task 1: Create Reflex Item

1.  On right side of Power BI home page, click on the **Settings** icon.

2.  In **Settings** pane, scroll down to **Governance and insights**,
    then click on **Admin portal** .

<img src="./media/image119.png" style="width:4.65in;height:7.275in" />

3.  In **Admin portal** pane, select **Tenant settings**, scroll down to
    **Microsoft Fabric** section, click on **Data Activator**, then
    enable it using the **toggle** button. After **Data Activator** were
    Enabled, click on the **Apply** button.

<img src="./media/image120.png"
style="width:7.1398in;height:5.7375in" />

4.  Now, select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane

<img src="./media/image121.png"
style="width:5.7125in;height:5.18452in" />

5.  At the bottom left of the Power BI portal, select
    the **Real-Time-Intelligence** icon and switch to the **Data
    Activator** experience

<img src="./media/image122.png" style="width:6.5in;height:6.75in" />

6.  Select **Reflex(Preview).**

<img src="./media/image123.png"
style="width:7.255in;height:6.04583in" />

7.  Select ***RTA_Eventstream*** in the left navigation pane.

<img src="./media/image124.png"
style="width:5.05417in;height:4.82321in" />

8.  On the ***StockEventstream*** page, add a new output by clicking on
    the **+ (plus)** symbol on the **StockEventStream** object, and
    select ***Reflex*** as shown in the below image.

<img src="./media/image125.png"
style="width:7.36549in;height:2.89583in" />

9.  Configure the Reflex as follows and then click on the ***Add***
    button**:**

- Destination name: +++**Reflex_RTA+++**

- Workspace: **RealTimeWorkspace** (or the name of your workspace)

- Select Reflex and click on **Done** button.

<img src="./media/image126.png"
style="width:3.54167in;height:6.55833in" />

10. You will get a notification that the destination “Reflex” was
    **Successfully added**.

<img src="./media/image127.png"
style="width:7.30069in;height:3.58006in" />

11. After the Reflex is added, open the Reflex by clicking the ***Data
    preview*** at the bottom of the page as shown in the below image.

<img src="./media/image128.png"
style="width:7.301in;height:3.57083in" />

## Task 2: Configure the object

1.  Now, select ***Fabric Real-Time AnalyticsXX*** workspace in the left
    navigation pane

<img src="./media/image96.png" style="width:6.49167in;height:3.925in" />

2.  In the *Fabric Real-Time AnalyticsXX pane**, ***Select **Reflex**

<img src="./media/image129.png"
style="width:6.49167in;height:3.725in" />

<img src="./media/image130.png" style="width:7.26461in;height:4.40611in"
alt="A screenshot of a computer Description automatically generated" />

**Note:** If the data does not reflect, please refresh the browser.

3.  In **StockEventStram-Reflex** window, enter the following details in
    the ***Assign your data** *pane*.* Then, click on ***Save** and
    select **Save and go to design mode***.

- Object name -+++ **EngineNode+++**

- Assign key column **- Node**

- Assign properties - select **Level**

<img src="./media/image131.png"
style="width:7.33646in;height:4.6125in" />

<img src="./media/image132.png"
style="width:4.025in;height:6.58333in" />

4.  

<img src="./media/image133.png" style="width:7.3143in;height:3.46648in"
alt="A screenshot of a computer Description automatically generated" />

5.  To add a new trigger, in the top navigation bar, click on the ***New
    Trigger*** button. In the **Unsaved change** dialog box, click on
    the **Save** button

<img src="./media/image134.png"
style="width:7.27873in;height:4.6625in" />

6.  Dropdown the **Select a property or event column**, select
    **Existing property** and select **Level** property for the trigger.

<img src="./media/image135.png"
style="width:7.26235in;height:3.77083in" />

<img src="./media/image136.png" style="width:7.39857in;height:3.31434in"
alt="A screenshot of a computer Description automatically generated" />

7.  **Define the condition to detect** pane, drop down the **change by**
    , select **Common** and select **Change to**

<img src="./media/image137.png"
style="width:7.29837in;height:4.30417in" />

8.  The conditions are met when Level property value changes from
    "Information" to "Warning".

<img src="./media/image138.png"
style="width:7.39808in;height:3.57083in" />

9.  Click on **Save** button

<img src="./media/image139.png"
style="width:6.49167in;height:2.05833in" />

<img src="./media/image140.png" style="width:7.37747in;height:3.40656in"
alt="A screenshot of a computer Description automatically generated" />

10. Scroll down, click on the dropdown beside **Act** and select
    **Email**.

<img src="./media/image141.png"
style="width:7.27174in;height:3.37917in" />

11. Then, click on **Send me a test alert**.

<img src="./media/image142.png"
style="width:7.4156in;height:3.84583in" />

**Important Note:**  Users having a trial account won't receive
notifications.

## Task 3: Clean up resources

1.  Select your workspace, the **Fabric Real-Time AnalyticsXX** from the
    left-hand navigation menu. It opens the workspace item view.

<img src="./media/image143.png"
style="width:4.95833in;height:5.33333in" />

2.  Select the ***...*** option under the workspace name and
    select **Workspace settings**.

<img src="./media/image144.png"
style="width:7.06968in;height:3.15417in" />

3.  Select **General** and **Remove this workspace.**

<img src="./media/image145.png" style="width:6.5in;height:4.38333in" />

4.  Click on **Delete** in the warning that pops up.

<img src="./media/image146.png" style="width:5.85051in;height:1.62514in"
alt="A white background with black text Description automatically generated" />

5.  Wait for a notification that the Workspace has been deleted, before
    proceeding to the next lab.

<img src="./media/image147.png" style="width:6.5in;height:2.15208in"
alt="A screenshot of a computer Description automatically generated" />

**Summary**

This practical lab focuses designing, creating, and configuring KQL
database. Once the KQL database is created, this lab enlists the steps
to ingest data into KQL database and tables using One Click ingestion,
basic KQL queries, explore and transform the data. The lab also covered
Caching and Retention Policies, metadata objects handling using control
commands and Visualize data in Fabric Real Time Dashboards 
