**Introduction**

Real-Time Intelligence in Microsoft Fabric is a powerful tool to extract
insights and visualize your data in motion. Real-Time Intelligence
provides an end-to-end solution for event-driven scenarios, streaming
data, and data logs.

In this use case , you'll learn how to set up and use the main features
of Real-Time Intelligence with a sample set of data.

The sample data you'll use in this tutorial is a set of bicycle data,
containing information about bike ID, location, timestamp, and more.
You'll learn how to set up resources, ingest data, set alerts on the
data, and visualize the data to extract insights.

**Objective**

- Set up your environment

- Get data in the Real-Time hub

- Transform events

- Publish an event stream

- Create a KQL query

- Create a Real-Time dashboard

- Explore data visually in the Real-Time dashboard

- Create a Power BI report from a KQL query

- Set an alert on the event stream

# Exercise 1: Environment Setup

## Task 1: Create Fabric Capacity using Azure portal 

Microsoft Fabric is deployed to an Azure Active Directory tenant. Within
each Fabric tenant, Fabric capacities can be created to group resources
for various purposes -- this might be done organizationally (sales,
marketing, development), geographically, or other logical grouping.

If a Fabric Trial is available, we recommend taking advantage of that
opportunity to try Microsoft Fabric for a period of time (currently 60
days) with no commitment. To see if you are in a trial or eligible for a
trial, visit the [Fabric portal](https://app.fabric.microsoft.com/). If
you are able to log in or presented the option to start a trial, you
should be all set!

To create a Fabric capacity outside of a trial environment, create a new
resource from the Azure portal, and search for Fabric.

1.  Open your browser, navigate to the address bar, type or paste the
    following URL:
    [<u>https://portal.azure.com/</u>](https://portal.azure.com/), then
    press the **Enter** button.

2.  From the Azure portal home page, click on **Azure portal menu**
    represented by three horizontal bars on the left side of the
    Microsoft Azure command bar as shown in the below image.

> <img src="./media/image1.png" style="width:6.5in;height:3.19167in"
> alt="A screenshot of a computer Description automatically generated" />

3.  Navigate and click on **+ Create a resource**.

> <img src="./media/image2.png" style="width:4.35417in;height:4.6875in"
> alt="A screenshot of a computer Description automatically generated" />

4.  On **Create a resource** page, in the **Search services and
    marketplace** search bar, type+++ **Fabric+++**, then select
    **Microsoft fabric**.

> <img src="./media/image3.png" style="width:6.5in;height:5.19167in" />

5.  In the **Marketplace** page, navigate to the **Microsoft Fabric**
    section, click on the **Create** button dropdown, then select
    **Microsoft Fabric** as shown in the image.

> <img src="./media/image4.png" style="width:6.49167in;height:5.49167in"
> alt="A screenshot of a computer Description automatically generated" />

6.  In the **Create Fabric capacity** window, under the **Basics** tab,
    enter the following details and click on the **Review+create**
    button.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 74%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Subscription</strong></th>
<th><blockquote>
<p>Select the assigned subscription</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Resource group</strong></td>
<td>Click on <strong>Create new</strong>&gt; enter
+++<strong>RealtimeFabricXXX+++</strong>(XXX can be a unique number, you
can add more digits after XXX to make the name unique)</td>
</tr>
<tr class="even">
<td><strong>Capacity name</strong></td>
<td>+++<strong>realtimefabriccapacityXXX</strong>+++( XXX can be a
unique number, you can add more digits after XXX to make the name
unique)</td>
</tr>
<tr class="odd">
<td><strong>Region</strong></td>
<td>Select <strong>West US 3</strong></td>
</tr>
<tr class="even">
<td><strong>Size</strong></td>
<td>select <strong>F64</strong> SKU</td>
</tr>
</tbody>
</table>

> <img src="./media/image5.png" style="width:6.5in;height:6.55833in"
> alt="A screenshot of a computer screen Description automatically generated" />
>
> <img src="./media/image6.png" style="width:6.49167in;height:6.475in" />
>
> <img src="./media/image7.png"
> style="width:6.49167in;height:6.43333in" />

7.  Once the Validation is succeeded, click on the **Create** button.

> <img src="./media/image8.png" style="width:6.49167in;height:5.88333in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image9.png" style="width:5.87917in;height:3.80278in"
> alt="A screenshot of a computer Description automatically generated" />

8.  After the deployment is completed, click on the **Go to resource**
    button.

> <img src="./media/image10.png"
> style="width:6.49167in;height:3.19167in" />
>
> <img src="./media/image11.png"
> style="width:6.97249in;height:3.64193in" />

## **Task 2: Create a Fabric workspace**

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: https://app.fabric.microsoft.com/ then press the
    **Enter** button. In the **Microsoft Fabric Home** page, navigate
    and click on **Power BI** tile.

> <img src="./media/image12.png" style="width:6.49167in;height:6.30833in"
> alt="A screenshot of a computer Description automatically generated" />

2.  In the **Power BI Home** page left-sided navigation menu, navigate
    and click on **Workspaces** as shown in the below image.

> <img src="./media/image13.png" style="width:6.5in;height:6.85833in"
> alt="A screenshot of a computer Description automatically generated" />

3.  In the Workspaces pane, click on **+** **New workspace button**

> <img src="./media/image14.png" style="width:4.41667in;height:7.7in"
> alt="A screenshot of a computer Description automatically generated" />

4.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

| **Name** | ***+++ RealTimeWorkspaceXX +++**(*XX can be a unique number) |
|----|----|
| **Advanced** | Under **License mode**, select **Trial** |
| **Default storage format** | **Small dataset storage format** |

> <img src="./media/image15.png" style="width:5.33333in;height:7.05in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image16.png" style="width:5.25417in;height:6.23887in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image17.png" style="width:4.02917in;height:4.92138in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image18.png" style="width:6.5in;height:4.27222in"
> alt="A screenshot of a computer Description automatically generated" />

## Task 3: Create an eventhouse

1.  On the bottom left experience switcher, select **Real-Time
    Intelligence**.

> <img src="./media/image19.png"
> style="width:5.13863in;height:5.87083in" />

2.  On the **Real-Time Intelligence** home page, select **Eventhouse**.
    Name the Eventhouse +++**Tutorial***+++* and click on the **Create**
    button.

> <img src="./media/image20.png" style="width:6.5in;height:4.125in" />
>
> <img src="./media/image21.png" style="width:3.65in;height:2.14167in" />

3.  When provisioning is complete, the eventhouse **System
    overview** page is shown.

> <img src="./media/image22.png" style="width:6.5in;height:4.18333in"
> alt="A screenshot of a computer Description automatically generated" />

## Task 4: Turn on OneLake availability

1.  From the **System overview** page, select the **Tutorial** KQL
    database you created in the previous task.

<img src="./media/image23.png"
style="width:6.49167in;height:4.58333in" />

2.  In the **Database details** section, select the **pencil icon** next
    to **OneLake availability**.

<img src="./media/image24.png"
style="width:6.49167in;height:3.45833in" />

3.  Toggle the button to **Active** and select **Done**.

<img src="./media/image25.png"
style="width:3.84167in;height:7.26667in" />

<img src="./media/image26.png"
style="width:7.37548in;height:3.47262in" />

# Exercise 2: Get data in the Real-Time hub

## Task 1: Create an event stream

1.  From the navigation bar, select **Real-Time hub**.

> <img src="./media/image27.png" style="width:6.5in;height:5.68333in" />

2.  In the **Real-Time hub** page , Select and dropdown the **+ Get
    events** and Select **Use Sample data**.

<img src="./media/image28.png" style="width:6.49167in;height:4.15in" />

1.  In **Connect** tab, under **Sample data** enter the **Source name**
    as +++***TutorialSource+++*** and in ***Sample data***
    select ***Bicycles (Reflex compatible**)*.

2.  Under the **Stream details, e**dit the **Eventstream name** by
    selecting the pencil icon and entering ***TutorialEventstream*.**
    Select **Next**.

<img src="./media/image29.png"
style="width:7.40753in;height:3.67083in" />

3.  In the **Review and create tab, r**eview the event stream details
    and select **Create source**.

<img src="./media/image30.png"
style="width:7.44877in;height:3.72917in" />

4.  A new event stream named ***TutorialEventstream*** is created.

5.  Select **Open Eventstream** from the notification that appears after
    creating the event stream.

<img src="./media/image31.png" style="width:7.42979in;height:3.62917in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image32.png" style="width:7.18283in;height:3.34892in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Transform events

1.  In the **Eventstream** page, select **Edit** from the menu ribbon.

<img src="./media/image33.png"
style="width:7.36069in;height:4.12917in" />

1.  In the event stream authoring canvas, select the down arrow on
    the **Transform events or add destination** tile. Select **Manage
    fields**. The tile is renamed to ***Manage_fields***.

<img src="./media/image34.png"
style="width:6.7375in;height:4.32445in" />

2.  Select the **pencil** icon on the ***Manage_fields*** tile.

<img src="./media/image35.png"
style="width:7.35174in;height:4.09583in" />

3.  In the ***Manage fields*** pane, in **Operation name**,
    nter +++***TutorialTransform***+++ and Select **Add all fields**

<img src="./media/image36.png"
style="width:3.18333in;height:5.95833in" />

4.  In the ***Manage fields*** pane, Select **+ Add field**.

<img src="./media/image37.png" style="width:3.25in;height:6.03333in" />

5.  In the ***Manage fields*** pane, dropdown the Field and from
    the **Built-in Date Time Function** dropdown,
    select **SYSTEM.Timestamp()**

<img src="./media/image38.png" style="width:3.18333in;height:6.025in" />

7.  Enter ***Timestamp*** as the **Field name** andSelect **Add**.

<img src="./media/image39.png" style="width:3.18333in;height:5.825in" />

8.  In **Manage fields** tab, Select **Save**.

<img src="./media/image40.png"
style="width:3.21667in;height:6.00833in" />

9.  The *TutorialTransform* tile now displays but with an error, because
    the destination isn't set.

<img src="./media/image41.png"
style="width:7.03445in;height:4.17858in" />

## Task 3: Create a destination

1.  Hover over the right edge of the ***TutorialTransform*** tile and
    select the **green plus** icon.

<img src="./media/image42.png" style="width:6.5in;height:3.63333in" />

2.  Select **Destinations** \> **KQL Database**.

<img src="./media/image43.png"
style="width:7.24583in;height:4.24146in" />

3.  A new tile is created entitled ***KQLDatabase1*.**

<img src="./media/image44.png"
style="width:7.06463in;height:3.1875in" />

4.  Select the **pencil icon** on the ***KQLDatabase1*** tile.

<img src="./media/image45.png"
style="width:7.3519in;height:3.37083in" />

5.  Enter the following information in the **KQL Database** pane and
    select the **Save** button.

| **Field** | **Value** |
|----|----|
| **Destination name** | *Enter +++TutorialDestination+++* |
| **Workspace** | Select the workspace in which you created your resources. |
| **KQL Database** | *Tutorial* |
| **Destination table** | *Create new* - enter +++***TutorialTable+++* **as table name and click on **Done** button |
| **Input data format** | *JSON* |

<img src="./media/image46.png" style="width:5.875in;height:6.88333in" />

<img src="./media/image47.png"
style="width:3.20833in;height:6.03333in" />

<img src="./media/image48.png" style="width:7.32208in;height:4.50277in"
alt="A screenshot of a computer Description automatically generated" />

6.  From the menu ribbon, select **Publish**.

<img src="./media/image49.png"
style="width:7.40761in;height:3.47083in" />

7.  The event stream is now set up to transform events and send them to
    a KQL database.

<img src="./media/image50.png"
style="width:7.11231in;height:3.90645in" />

# Exercise 3: Query streaming data in a KQL queryset

In this exercise, you learn how to query your streaming data in a [KQL
queryset](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/create-query-set).
You create a KQL queryset, write a KQL query, and visualize the data in
a time chart.

## Task 1: Create a KQL queryset

1.  From the navigation bar, select the KQL database you created in
    exercise1\> task3, named ***Tutorial*.**

<img src="./media/image51.png" style="width:5.675in;height:6.71667in" />

2.  Verify that the data is flowing into the database by viewing
    the **Size** tile in the database details page. The values in this
    tile should be greater than zero. If the values in the **Size** tile
    are zero, select **Refresh** from the menu ribbon.

<img src="./media/image52.png"
style="width:7.05417in;height:4.29227in" />

3.  From the menu ribbon, select **New related item** and choose **KQL
    Queryset**.

<img src="./media/image53.png"
style="width:7.06525in;height:4.3625in" />

4.  Enter the name for the KQL
    Queryset: +++***TutorialQueryset+++* **and select **Create**.

> <img src="./media/image54.png" style="width:3.275in;height:1.94167in" />

5.  Select the ***Tutorial* **database as the data source for the KQL
    queryset, then select **Connect**.

<img src="./media/image55.png"
style="width:7.22083in;height:4.37514in" />

6.  A new KQL queryset is created and opens in the KQL Queryset editor.
    It's connected to the *Tutorial* database as a data source, and is
    pre-populated with several general queries.

<img src="./media/image56.png"
style="width:7.17903in;height:4.00829in" />

## Task 2: Write a KQL query

The name of the table you created in a previous step is *TutorialTable*.
Use this (case-sensitive) name as the data source for your query.

1.  In the query editor, delete the pre-populated queries and enter the
    following query. Then press **Shift + Enter** to run the query or
    **▷ Run** button and review the output.

> **KustoCopy**
>
> TutorialTable
>
> \| take 10

<img src="./media/image57.png"
style="width:7.19129in;height:3.6668in" />

This query returns 10 arbitrary records from the table. What information
about the data can you see at a glance? Notice that one of the columns
is named *No_Bikes*. This column contains the number of empty docks at a
bike station. This is a field you may be concerned with if you're
tracking the availability of bikes at a station.

2.  To see the data in a more visual way, use the **render** operator.
    Run the following query:

3.  Replace all the code in the **cell** with the following code and
    click on **▷ Run** button and review the output.

> **KustoCopy**
>
> TutorialTable
>
> \| where Neighbourhood == "Chelsea"
>
> \| project Timestamp, No_Bikes
>
> \| render timechart

This query creates a time chart that shows the number of bikes in the
Chelsea neighborhood as a time chart.

<img src="./media/image58.png"
style="width:7.32377in;height:3.62311in" />

# Exercise 4: Create a Real-Time Dashboard

## Task 1: Create a Real-Time Dashboard

1.  In your KQL queryset, copy/paste, and run the following query. This
    query returns a column chart showing the most recent number of bikes
    by *BikepoointID*.

2.  Replace all the code in the **cell** with the following code and
    click on **▷ Run** button and review the output.

> **KustoCopy**
>
> TutorialTable
>
> \| summarize arg_max(Timestamp, No_Bikes) by BikepointID
>
> \| sort by BikepointID
>
> \| render columnchart with (ycolumns=No_Bikes, xcolumn= BikepointID)

<img src="./media/image59.png"
style="width:7.34946in;height:3.52462in" />

3.  Now, click on **RealTimeWorkspace** on the left-sided navigation
    pane as shown in the below image.

<img src="./media/image60.png"
style="width:5.8625in;height:4.59968in" />

4.  In the **RealTime workspace** page, navigate to **New** section and
    click on, select **More options** 

<img src="./media/image61.png" style="width:6.5in;height:6.43958in" />

5.  Under the Real-Time Intelligence , select **Real-Time Dashboard**
    and **New Real-Time Dashboard**  dialog box, enter
    +++**TutorialDashboard+++**in the **Name** field, click on the
    **Create** button and open the new **Real-Time Dashboard**

<img src="./media/image62.png" style="width:6.5in;height:3.72708in" />

<img src="./media/image63.png"
style="width:3.25764in;height:1.96181in" />

<img src="./media/image64.png" style="width:6.5in;height:5.10694in" />

<img src="./media/image65.png"
style="width:6.49236in;height:5.89375in" />

6.  Select **Pin to dashboard**.

<img src="./media/image66.png" style="width:6.5in;height:3.70486in" />

<img src="./media/image67.png"
style="width:3.92917in;height:3.73139in" />

<img src="./media/image68.png"
style="width:7.37011in;height:4.50947in" />

7.  Enter the following information and click on **Create** button

| **Field**                         | **Value**                   |
|-----------------------------------|-----------------------------|
| **Create new tile**               | *In a new dashboard*        |
| **Dashboard name**                | *TutorialDashboard*         |
| **Tile name**                     | *Recent bikes by Timepoint* |
| **Open dashboard after creation** | *Selected*                  |

<img src="./media/image69.png"
style="width:6.14583in;height:3.97151in" />

<img src="./media/image70.png"
style="width:5.50764in;height:4.12847in" />

The new dashboard, *TutorialDashboard*, opens with the *Recent bikes by
Bikepoint* tile.

<img src="./media/image71.png" style="width:6.5in;height:4.44167in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Add a new tile to the dashboard

1.  On the top menu bar, toggle from **Viewing** mode
    to **Editing** mode.

<img src="./media/image72.png"
style="width:6.96509in;height:2.44129in" />

2.  Select **New tile**

<img src="./media/image73.png"
style="width:6.49236in;height:3.37153in" />

3.  In the query editor, enter the following query:

> KustoCopy
>
> TutorialTable
>
> \| where Neighbourhood == "Chelsea"

<img src="./media/image74.png"
style="width:7.34653in;height:5.68571in" />

4.  From the menu ribbon, Select **Apply changes**. A new tile is
    created.

<img src="./media/image75.png"
style="width:7.34708in;height:2.27462in" />

<img src="./media/image76.png" style="width:7.35759in;height:3.34865in"
alt="A screenshot of a computer Description automatically generated" />

5.  Rename the tile by selecting the **More menu \[...\]** on the top
    right corner of the tile \> **Rename tile**.

<img src="./media/image77.png"
style="width:7.32908in;height:2.73674in" />

6.  Enter the new name ***Chelsea bikes*** to rename the tile.

<img src="./media/image78.png"
style="width:7.3322in;height:2.92614in" />

## Task 3: Explore the data visually by adding an aggregation

1.  On the new **Chelsea bikes** tile, select the **Explore** icon.
    <img src="./media/image79.png"
    style="width:0.26669in;height:0.27502in" />

<img src="./media/image80.png"
style="width:7.09376in;height:2.82765in" />

<img src="./media/image81.png" style="width:7.00964in;height:3.35279in"
alt="A screenshot of a computer Description automatically generated" />

2.  Select **+ Add** \> **Aggregation**.

<img src="./media/image82.png"
style="width:7.06617in;height:3.10038in" />

3.  Select **+ Add grouping**.

<img src="./media/image83.png"
style="width:7.3866in;height:3.26705in" />

4.  Select **Group by** \> *Street*.

<img src="./media/image84.png"
style="width:7.29523in;height:3.18371in" />

5.  Select **Apply**.

<img src="./media/image85.png"
style="width:7.31826in;height:4.64583in" />

<img src="./media/image86.png"
style="width:7.4164in;height:3.53151in" />

6.  Notice that the query elements are updated to include the
    green **count() by Street** aggregation. The resulting table changed
    to show the total count of bike locations by street.

<img src="./media/image86.png"
style="width:7.12505in;height:3.39272in" />

7.  Close the **Explore** pane and select **Save** from the menu ribbon
    to save the changes.

<img src="./media/image87.png"
style="width:7.35106in;height:3.16856in" />

<img src="./media/image88.png"
style="width:7.39601in;height:2.99432in" />

<img src="./media/image89.png" style="width:3.38363in;height:1.05843in"
alt="A screenshot of a computer Description automatically generated" />

# Exercise 5: Create a Power BI report

A Power BI report is a multi-perspective view into a semantic model,
with visuals that represent findings and insights from that semantic
model. In this section, you use a KQL query output to create a new Power
BI report.

## Task 1: Build a Power BI report

1.  From the navigation bar, select the KQL queryset you created in a
    previous step, named ***TutorialQueryset*.**

<img src="./media/image90.png" style="width:6.5in;height:4.62847in" />

2.  Copy and paste the following query into the query editor. The output
    of this query is used as the semantic model for building the Power
    BI report.

> **KustoCopy**
>
> TutorialTable
>
> \| summarize arg_max(Timestamp, No_Bikes, No_Empty_Docks,
> Neighbourhood, Lat=todouble(Latitude), Lon=todouble(Longitude)) by
> BikepointID

<img src="./media/image91.png"
style="width:7.12311in;height:3.79497in" />

3.  Select **Power BI**. The Power BI report editor opens with the query
    result available as a data source named **Kusto Query Result**.

<img src="./media/image92.png"
style="width:7.31317in;height:2.95644in" />

<img src="./media/image93.png"
style="width:6.49236in;height:4.37847in" />

## Task 2: Enable azure maps visuals sign in

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: <https://app.fabric.microsoft.com/home> then
    press the **Enter** button. In Microsoft Fabric page, click on
    **Power BI**.

<img src="./media/image94.png" style="width:6.5in;height:4.18333in"
alt="A screenshot of a computer Description automatically generated" />

2.  On right side of Power BI home page, click on the **Settings** icon.

<img src="./media/image95.png" style="width:4.44496in;height:4.68371in"
alt="A screenshot of a computer Description automatically generated" />

In case, you did not see the **Settings** icon, then click on the
horizontal ellipsis on the top right side of the page, then navigate and
click on **Settings** as shown in the below image.

3.  In Settings pane, scroll down to Governance and insights, then click
    on Admin portal .

<img src="./media/image96.png" style="width:6.5in;height:3.94167in" />

<img src="./media/image97.png" style="width:6.5in;height:5.15833in" />

6.  In **Admin portal** pane, select **Tenant settings**, scroll down to
    **Integration settings** section, click on **Use Azure Maps
    visual**, then enable it using the **toggle** button. After **Use
    Azure Maps visual** were Enabled, click on the **Apply** button.

<img src="./media/image98.png"
style="width:6.49236in;height:4.54514in" />

<img src="./media/image99.png" style="width:6.5in;height:5.06042in" />

7.  You will see a notification stating - **Applying changes.** It will
    take approximately 15 minutes for implemting the tenant settings.

<img src="./media/image100.png" style="width:3.55031in;height:0.79174in"
alt="A screenshot of a computer Description automatically generated" />

8.  In **Admin portal** pane, select **Tenant settings**, scroll down to
    **Integration settings** section, click on **Map and filled map
    visuals**, then enable it using the **toggle** button. After **Use
    Map and filled map visuals** were Enabled, click on the **Apply**
    button.

<img src="./media/image101.png" style="width:6.5in;height:4.49236in" />

9.  You will see a notification stating - **Applying changes.** It will
    take approximately 15 minutes for implemting the tenant settings.

<img src="./media/image102.png" style="width:4.6254in;height:1.40012in"
alt="A white rectangular box with black text Description automatically generated" />

## Task 3: Add visualizations to the report

1.  In the report editor,
    select **Visualizations** \> **Map** icon. <img src="./media/image103.png"
    style="width:0.30978in;height:0.32125in" />

> <img src="./media/image104.png"
> style="width:6.97046in;height:4.67083in" />

2.  Drag the following fields from **Data** \> **Kusto Query Result** to
    the **Visualizations** pane.

    - **Lat** \> **Latitude**

    <!-- -->

    - **Lon** \> **Longitude**

    <!-- -->

    - **No_Bikes** \> **Bubble size**

    <!-- -->

    - **Neighbourhood** \> **Add drill-through fields here**

<img src="./media/image105.png"
style="width:7.32753in;height:5.00417in" />

3.  In the report editor, select **Visualizations** \> **Stacked column
    chart** icon. <img src="./media/image106.png"
    style="width:0.25836in;height:0.25002in" />

<img src="./media/image107.png"
style="width:6.49167in;height:4.23333in" />

4.  Drag the following fields from **Data** \> **Kusto Query Result** to
    the **Visualizations** pane.

    - **Neighbourhood** \> **X-axis**

    <!-- -->

    - **No_Bikes** \> **Y-axis**

    <!-- -->

    - **No_Empty_Docks** \> **Y-axis**

<img src="./media/image108.png"
style="width:7.2375in;height:4.88995in" />

## Task 4: Save the report

1.  In the top left corner of the ribbon, select **File** \> **Save**.

> <img src="./media/image109.png" style="width:6.49167in;height:4.45in" />

2.  Enter the name +++***TutuorialReport+++***. Choose your workspace,
    and set sensitivity as Public. Select **Continue**.

<img src="./media/image110.png"
style="width:3.08333in;height:2.44167in" />

3.  Select **Open the file in Power BI to view, edit, and get a
    shareable link.**

<img src="./media/image111.png"
style="width:7.0375in;height:4.82417in" />

<img src="./media/image112.png" style="width:7.14628in;height:3.83273in"
alt="A screenshot of a computer screen Description automatically generated" />

# Exercise 6: Set an alert on your event stream

In this part of the tutorial, you learn how to set an alert on your
event stream to receive a notification in Teams when the number of bikes
falls below a certain threshold.

## Task 1: Set an alert on the event stream

1.  From the left navigation bar, select **Real-Time hub**.

<img src="./media/image113.png"
style="width:4.33333in;height:5.84167in" />

2.  Select the event stream you created in the previous task
    named ***TutorialEventstream*.** The event stream details page
    opens.

<img src="./media/image114.png"
style="width:7.18581in;height:3.0625in" />

3.  In the Real-Time Intelligence page , select **Set alert.**

<img src="./media/image115.png"
style="width:7.29603in;height:3.2125in" />

4.  A new pane opens. Fill in the fields as follows and
    select **Create**.

| **Field**         | **Value**                                    |
|-------------------|----------------------------------------------|
| **Condition**     |                                              |
| Check             | On each event when                           |
| Field             | No_Bikes                                     |
| Condition         | Is less than                                 |
| Value             | 5                                            |
| **Action**        | **Message me in Teams**                      |
| **Save location** |                                              |
| Workspace         | The workspace in which you created resources |
| Item              | Create a new item                            |
| New item name     | **+++Tutorial-Reflex+++**                    |

<img src="./media/image116.png" style="width:2.83333in;height:7.25in" />

<img src="./media/image117.png"
style="width:2.75833in;height:7.225in" />

The alert is set and you receive a notification in Teams when the
condition is met.

<img src="./media/image118.png" style="width:2.76691in;height:4.76708in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Clean up resources

1.  Select your workspace, the  **RealTimeWorkspaceXXX**  from the
    left-hand navigation menu. It opens the workspace item view.

<img src="./media/image119.png" style="width:4.45833in;height:5.1in" />

2.  Select the ***...*** option under the workspace name and
    select **Workspace settings**.

<img src="./media/image120.png"
style="width:7.3995in;height:2.47917in" />

3.  Select **General** and **Remove this workspace.**

<img src="./media/image121.png" style="width:6.5in;height:6.21667in" />

4.  Open your browser, navigate to the address bar, type or paste the
    following URL:
    [<u>https://portal.azure.com/</u>](https://portal.azure.com/), then
    press the **Enter** button.

5.  To delete the fabric account, navigate to **Azure portal Home**
    page, click on **Resource groups**.

<img src="./media/image122.png"
style="width:7.14135in;height:4.12917in" />

6.  Click on the resource group that you’ve created.

<img src="./media/image123.png" style="width:6.5in;height:3.975in" />

7.  In the Resource group home page, select the **Delete resource
    group** button

<img src="./media/image124.png"
style="width:7.33067in;height:3.60417in" />

8.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button.

<img src="./media/image125.png"
style="width:5.41667in;height:7.125in" />

<img src="./media/image126.png" style="width:2.94167in;height:1.6in" />

<img src="./media/image127.png" style="width:4.35038in;height:1.89183in"
alt="A screenshot of a computer Description automatically generated" />
