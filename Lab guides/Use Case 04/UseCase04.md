**Introduction**

A medallion architecture (also coined by Databricks) is a data design
pattern used to logically organize data. The goal is to incrementally
improve the structure and quality of data as it flows through each layer
of the architecture. Medallion architectures are sometimes also referred
to as "multi-hop" architectures.

Creating a multi layer data platform allow companies to improve data
quality across the layers and at the same time provide for their
business needs. Unstructured and raw data are ingested using scalable
pipelines to output the highest quality enriched data. It contains 3
basic layers.

Bronze layer 

- Contains raw data, sometimes referenced as the data staging area.

- Not accessible to consumers only to engineers.

- May contain data with PII (personal identifiable information).

Silver layer 

- Contains deduplicated, enriched data.

- Accessible to all consumers.

- Contains “anonymized data” (no PII).

- Consumers: Data analysts, Data scientists, Engineers.

Gold layer 

- Contains aggregated data.

- Accessible to all consumers.

- Built for dashboards.

<img src="./media/image1.png" style="width:7.24826in;height:3.00694in"
alt="Architectural Diagram" />

**Objective**

- Build a medallion architecture in MS Fabric Real time analytics

- Use Fabric data pipelines for copying data from an operational DB (SQL
  server with Adventure works sample data)

- Stream events and ingest them into MS Fabric RTA (Real time analytics)
  using EventStream

- Create data transformations in Fabric RTA (Real time analytics)

- Create reports for real time visualizations using RTA (Real time
  analytics) dashboards

## Task 1: Create Fabric Capacity using Azure portal 

Microsoft Fabric is deployed to an Azure Active Directory tenant. Within
each Fabric tenant, Fabric capacities can be created to group resources
for various purposes -- this might be done organizationally (sales,
marketing, development), geographically, or other logical grouping.

If a Fabric Trial is available, we recommend taking advantage of that
opportunity to try Microsoft Fabric for a period of time (currently 60
days) with no commitment. To see if you are in a trial or eligible for a
trial, visit the [Fabric
portal](https://github.com/microsoft/FabricRTIWorkshop/blob/main/kql/createAll.kql).
If you are able to log in or presented the option to start a trial, you
should be all set!

To create a Fabric capacity outside of a trial environment, create a new
resource from the Azure portal, and search for Fabric.

1.  From the Azure portal home page, click on **Azure portal menu**
    represented by three horizontal bars on the left side of the
    Microsoft Azure command bar as shown in the below image.

> <img src="./media/image2.png" style="width:6.5in;height:3.19167in"
> alt="A screenshot of a computer Description automatically generated" />

2.  Navigate and click on **+ Create a resource**.

> <img src="./media/image3.png" style="width:4.35417in;height:4.6875in"
> alt="A screenshot of a computer Description automatically generated" />

3.  On **Create a resource** page, in the **Search services and
    marketplace** search bar, type+++ **Fabric+++**, then select
    **Microsoft fabric**.

> <img src="./media/image4.png" style="width:6.5in;height:5.19167in" />

4.  In the **Marketplace** page, navigate to the **Microsoft Fabric**
    section, click on the **Create** button dropdown, then select
    **Microsoft Fabric** as shown in the image.

> <img src="./media/image5.png" style="width:6.49167in;height:5.49167in"
> alt="A screenshot of a computer Description automatically generated" />

5.  In the **Create Fabric capacity** window, under the **Basics** tab,
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

> <img src="./media/image6.png" style="width:6.5in;height:6.55833in"
> alt="A screenshot of a computer screen Description automatically generated" />
>
> <img src="./media/image7.png" style="width:6.49167in;height:6.475in"
> alt="A screenshot of a computer screen Description automatically generated" />
>
> <img src="./media/image8.png"
> style="width:6.49167in;height:6.43333in" />

6.  Once the Validation is succeeded, click on the **Create** button.

> <img src="./media/image9.png" style="width:6.49167in;height:5.88333in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image10.png" style="width:5.87917in;height:3.80278in"
> alt="A screenshot of a computer Description automatically generated" />

7.  After the deployment is completed, click on the **Go to resource**
    button.

> <img src="./media/image11.png"
> style="width:6.49167in;height:3.19167in" />
>
> <img src="./media/image12.png" style="width:6.97249in;height:3.64193in"
> alt="A screenshot of a computer Description automatically generated" />

## **Task 1: Create a Fabric workspace**

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: +++<https://app.fabric.microsoft.com/>+++ then
    press the **Enter** button. In the **Microsoft Fabric Home** page,
    navigate and click on **Power BI** tile.

> <img src="./media/image13.png" style="width:6.49167in;height:6.30833in"
> alt="A screenshot of a computer Description automatically generated" />

2.  In the **Power BI Home** page left-sided navigation menu, navigate
    and click on **Workspaces** as shown in the below image.

> <img src="./media/image14.png" style="width:6.5in;height:6.85833in"
> alt="A screenshot of a computer Description automatically generated" />

3.  In the Workspaces pane, click on **+** **New workspace button**

> <img src="./media/image15.png" style="width:4.41667in;height:7.7in"
> alt="A screenshot of a computer Description automatically generated" />

4.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

| **Name**                   | ***+++RTI-Medallion+++***        |
|----------------------------|----------------------------------|
| **Advanced**               | Select **Fabric Trial**          |
| **Default storage format** | **Small dataset storage format** |

> <img src="./media/image16.png"
> style="width:6.05833in;height:7.03333in" />
>
> <img src="./media/image17.png"
> style="width:6.13333in;height:7.05833in" />
>
> <img src="./media/image18.png" style="width:6.5in;height:4.43958in"
> alt="A screenshot of a computer Description automatically generated" />

## Task 2: Create a new Eventhouse

1.  On the bottom left experience switcher, select **Real-Time
    Intelligence**.

> <img src="./media/image19.png"
> style="width:5.24196in;height:6.32765in" />

2.  On the **Real-Time Intelligence** home page, select **Eventhouse**.
    Name the Eventhouse +++ **RTIdemo***+++* and click on the **Create**
    button.

> <img src="./media/image20.png"
> style="width:5.84591in;height:5.19886in" />
>
> <img src="./media/image21.png"
> style="width:3.57569in;height:2.13611in" />

3.  When provisioning is complete, the eventhouse **System
    overview** page is shown.

> <img src="./media/image22.png" style="width:6.5in;height:4.25069in"
> alt="A screenshot of a computer Description automatically generated" />

## Task 3: Turn on OneLake availability

1.  From the **System overview** page, select the **RTIdemo** KQL
    database you created in the previous task.

<img src="./media/image23.png"
style="width:7.23674in;height:4.60182in" />

2.  In the **Database details** section, select the **pencil icon** next
    to **OneLake availability**.

<img src="./media/image24.png"
style="width:7.14695in;height:3.91099in" />

3.  Toggle the button to **Active** and select **Done**.

<img src="./media/image25.png"
style="width:3.81042in;height:7.24236in" />

<img src="./media/image26.png" style="width:6.5in;height:4.23056in"
alt="A screenshot of a computer Description automatically generated" />

## **Task 4: Create a lakehouse**

1.  Now, click on **RTI-Medallian** on the left-sided navigation pane.

> <img src="./media/image27.png" style="width:6.5in;height:5.25in" />

2.  In the **Real-Time Intelligence** page, navigate to **New** section
    and click on, select **Lakehouse** to create a lakehouse.

<img src="./media/image28.png"
style="width:6.06042in;height:7.01528in" />

3.  In the **New lakehouse** dialog box, enter
    +++**RTIDemoLakehouse+++**in the **Name** field, click on the
    **Create** button and open the new lakehouse.

> **Note**: Ensure to remove space before **RTIDemoLakehouse**.
>
> <img src="./media/image29.png"
> style="width:3.03056in;height:1.78056in" />
>
> <img src="./media/image30.png" style="width:6.5in;height:4.25139in"
> alt="A screenshot of a computer Description automatically generated" />

4.  You will see a notification stating **Successfully created SQL
    endpoint**.

> <img src="./media/image31.png" style="width:3.39196in;height:2.88358in"
> alt="A screenshot of a computer Description automatically generated" />

## **Task 5: Create a new Eventstream**

In this section we will be streaming events (impressions and clicks
events) generated by a notebook. The events will be streamed into an
eventstream and consumed by our Eventhouse KQL Database.

1.  Now, click on **RTI-Medallian** on the left-sided navigation pane.

> <img src="./media/image27.png"
> style="width:5.52462in;height:4.46219in" />

2.  From your workspace, click on ***+** **New \> Eventstream** as shown
    in the below image.* Enter **Eventstream** as
    +++**RTADemoEventStream**+++

> <img src="./media/image32.png"
> style="width:4.68371in;height:7.18194in" />
>
> <img src="./media/image33.png"
> style="width:2.96944in;height:2.35625in" />

3.  In the event stream authoring canvas, select the down arrow on
    the **+New source** tile. Select **Custom App**.

<img src="./media/image34.png"
style="width:6.49236in;height:3.25764in" />

4.  In the **Custom** pane, enter the **SourceApp** as **CustomApp** and
    click on **Add** button.

<img src="./media/image35.png"
style="width:3.16667in;height:6.46944in" />

<img src="./media/image36.png"
style="width:3.00859in;height:1.38345in" />

5.  On the **Eventstream** pane**,** select the **keys** under the
    **Details**, copy the **Event hub name**, **connection
    strings-primarykey** and paste them on a notepad, as you need them
    in the upcoming task

<img src="./media/image37.png"
style="width:6.49236in;height:3.65139in" />

## Task 6: Import Data Generator Notebook

1.  Now, click on **RTI-Medallian** on the left-sided navigation pane.

<img src="./media/image38.png"
style="width:3.88194in;height:4.01421in" />

2.  On the **Real-Time Intelligence** page, from the menu bar, select
    the **New** drop-down, and from there select **Import notebook.**

<img src="./media/image39.png"
style="width:4.30612in;height:4.50694in" />

3.  Select **Upload** from the **Import status** pane that appears on
    the right side of the screen.

<img src="./media/image40.png" style="width:3.11389in;height:1.75in" />

4.  Navigate and select **Generate synthetic events** notebooks from
    **C:\LabFiles**and click on the **Open** button.

<img src="./media/image41.png" style="width:6.5in;height:4.06042in" />

5.  You will see a notification stating **Imported successfully.**

<img src="./media/image42.png"
style="width:7.23652in;height:3.38632in" />

6.  *Then*, select the **Generate synthetic events **notebook.

<img src="./media/image43.png" style="width:6.5in;height:4.08333in" />

**Note**: DO NOT use an InPrivate browser window. Recommend using a
Personal browser window for the Notebook session to connect & run
successfully.

7.  In order for the compute to run the notebook to have the right
    libraries, dropdown an environment and select **New environment**.

<img src="./media/image44.png"
style="width:6.49236in;height:3.62153in" />

8.  In the New Environment pane, enter the name as **RTIDemoEnv** and
    click on the **Create** button.

<img src="./media/image45.png"
style="width:3.01528in;height:1.89375in" />

9.  Select +Add from PyPI

<img src="./media/image46.png"
style="width:6.49236in;height:4.53056in" />

10. Under Public libraries, add the below libraries and click on the
    **Publish** button.

| Library        | Version |
|----------------|---------|
| azure-eventhub | 5.11.6  |
| faker          | 24.2.0  |
| pyodbc         | 5.1.0   |

<img src="./media/image47.png"
style="width:7.31577in;height:3.63636in" />

<img src="./media/image48.png"
style="width:7.40909in;height:3.75249in" />

<img src="./media/image49.png"
style="width:7.07109in;height:2.88068in" />

11. In the **Pending changes** pane, select the **Publish all**.

<img src="./media/image50.png"
style="width:4.31401in;height:5.45833in" />

12. Click on the **Publish** button.

<img src="./media/image51.png"
style="width:3.04514in;height:1.50764in" />

<img src="./media/image52.png"
style="width:3.02526in;height:1.89183in" />

13. Publish the libraries will take 12-13 minutes

<img src="./media/image53.png" style="width:6.5in;height:2.80625in"
alt="A screenshot of a computer Description automatically generated" />

14. *Then*, select the **Generate synthetic events **notebook.

<img src="./media/image54.png"
style="width:5.35625in;height:5.09861in" />

15. Select the **RTIDemoEnv** environment. To start the notebook, run
    the the 1<sup>st</sup> cell.

<img src="./media/image55.png"
style="width:6.49236in;height:3.20486in" />

16. In the 2<sup>nd</sup> cell paste the **connection string of your
    custom app source and eventHunNameevents**(the value that you have
    saved in your notepad in the **Exercise 2\>Task 3\>Step 5)**, select
    the **Run** icon that appears on the left side of the cell.

<img src="./media/image56.png"
style="width:7.31088in;height:2.04861in" />

17. Select and run the 3<sup>rd</sup> ,4<sup>th</sup> cells

<img src="./media/image57.png"
style="width:6.49236in;height:3.21181in" />

<img src="./media/image58.png" style="width:6.5in;height:3.40139in" />

18. The last code cell and it should begin to print the generated
    synthetic events in JSON format. The last code cell its still
    running continue the next steps.

<img src="./media/image59.png" style="width:6.5in;height:3.20625in"
alt="A screenshot of a computer Description automatically generated" />

## Task 7: Define destination in the Eventstream

1.  Now, click on **RTADemoEventStream** on the left-sided navigation
    pane.

<img src="./media/image60.png"
style="width:4.77958in;height:4.85417in" />

2.  In the event stream authoring canvas, select the down arrow on
    the **+New destination** and select **KQL Database** tile.

<img src="./media/image61.png"
style="width:6.49236in;height:3.31806in" />

3.  Select **Direct ingestion** and enter the following information in
    the **KQL Database** pane. Select the **Add and configure** button.

| **Field** | **Value** |
|----|----|
| **Destination name** | *Enter +++RTIDemoEventhouse+++* |
| **Workspace** | Select the workspace in which you created your resources. |
| **KQL Database** | *RTIDemo* |

<img src="./media/image62.png"
style="width:3.32639in;height:6.49331in" />

4.  On the first settings page, select the + and enter the table name as
    **events** table and click on the **Next** button.

<img src="./media/image63.png"
style="width:6.49236in;height:3.79514in" />

5.  The next page allows us to inspect and configure the schema. Be sure
    to change the format from TXT to **JSON**, if necessary. The default
    columns of *eventType,eventide,eventDate,productid and
    userAgent* should be formatted as shown in the below image; then
    click on the ***Finish*** button.

<img src="./media/image64.png"
style="width:5.99306in;height:3.48275in" />

6.  On the **Summary** page, if there are no errors, you’ll see a
    **green checkmark** as shown in the below image, then click on the
    ***Close*** button to complete the configuration

<img src="./media/image65.png"
style="width:7.2822in;height:4.24048in" />

<img src="./media/image66.png"
style="width:7.31338in;height:3.72159in" />

<img src="./media/image67.png"
style="width:7.26086in;height:3.87866in" />

## Task 8: Build the KQL DB schema 

In this task we will create all the tables, functions,
materialized-views, and enable update policies and in our Eventhouse KQL
Database. Two of the tables (product and productCategory) are shortcuts
to our SQL Database and the data is NOT being copied into our KQL
Database.

1.  Now, click on **RTIdemo** on the left-sided navigation pane.Click on
    "Explore your Data".  
    <img src="./media/image68.png" style="width:6.5in;height:2.83056in"
    alt="alt text" />

2.  copy the entire
    [createAll.kql](https://github.com/microsoft/FabricRTIWorkshop/blob/main/kql/createAll.kql)
    file content. Replace all on the "**Explore your data**" by deleting
    lines 1-19 and paste the contents of the createAll.kql file. Click
    **Run.**

> //External tables - shortcuts
>
> // connect to operational Database with external table Product
>
> .create external table products (ProductID: int, ProductNumber:
> string, Name: string)
>
> kind=sql
>
> table=\[SalesLT.Product\]
>
> (
>
> h@'Server=tcp:adxdemo.database.windows.net,1433;Initial
> Catalog=aworks;User Id=sqlread;Password=ChangeYourAdminPassword1'
>
> )
>
> with
>
> (
>
> createifnotexists = true
>
> )
>
> // connect to operational Database with external table ProductCategory
>
> .create external table productCategories (ProductCategoryID: int,
> Name: string)
>
> kind=sql
>
> table=\[SalesLT.ProductCategory\]
>
> (
>
> h@'Server=tcp:adxdemo.database.windows.net,1433;Initial
> Catalog=aworks;User Id=sqlread;Password=ChangeYourAdminPassword1'
>
> )
>
> with
>
> (
>
> createifnotexists = true
>
> )
>
> //BRONZE LAYER
>
> .create table \[Address\]
> (AddressID:int,AddressLine1:string,AddressLine2:string,City: string,
> StateProvince:string, CountryRegion:string, PostalCode: string,
> rowguid: guid, ModifiedDate:datetime)
>
> .create table \[Customer\](CustomerID:int, NameStyle: string, Title:
> string, FirstName: string, MiddleName: string, LastName:
> string,Suffix:string, CompanyName: string, SalesPerson: string,
> EmailAddress: string, Phone: string, ModifiedDate: datetime)
>
> .create table \[SalesOrderHeader\](SalesOrderID: int, OrderDate:
> datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int,
> BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight:
> decimal, TotalDue: decimal, ModifiedDate: datetime)
>
> .create table \[SalesOrderDetail\](SalesOrderID: int,
> SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice:
> decimal , UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate:
> datetime)
>
> //adds a hidden field showing ingestion time
>
> .alter table Address policy ingestiontime true
>
> .alter table Customer policy ingestiontime true
>
> .alter table SalesOrderHeader policy ingestiontime true
>
> .alter table SalesOrderDetail policy ingestiontime true
>
> //SILVER LAYER
>
> .create table \[SilverAddress\]
> (AddressID:int,AddressLine1:string,AddressLine2:string,City: string,
> StateProvince:string, CountryRegion:string, PostalCode: string,
> rowguid: guid, ModifiedDate:datetime, IngestionDate: datetime)
>
> .create table \[SilverCustomer\](CustomerID:int, NameStyle: string,
> Title: string, FirstName: string, MiddleName: string, LastName:
> string,Suffix:string, CompanyName: string, SalesPerson: string,
> EmailAddress: string, Phone: string, ModifiedDate: datetime,
> IngestionDate: datetime)
>
> .create table \[SilverSalesOrderHeader\](SalesOrderID: int, OrderDate:
> datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int,
> BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight:
> decimal, TotalDue: decimal, ModifiedDate: datetime, DaysShipped: long,
> IngestionDate: datetime)
>
> .create table \[SilverSalesOrderDetail\](SalesOrderID: int,
> SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice:
> decimal, UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate:
> datetime, IngestionDate: datetime)
>
> // use update policies to transform data during Ingestion
>
> .create function ifnotexists with (docstring = 'Add ingestion time to
> raw data') ParseAddress (){
>
> Address
>
> \| extend IngestionDate = ingestion_time()
>
> }
>
> .alter table SilverAddress policy update @'\[{"Source": "Address",
> "Query": "ParseAddress", "IsEnabled" : true, "IsTransactional": true
> }\]'
>
> .create function ifnotexists with (docstring = 'Add ingestion time to
> raw data') ParseCustomer (){
>
> Customer
>
> \| extend IngestionDate = ingestion_time()
>
> }
>
> .alter table SilverCustomer policy update @'\[{"Source": "Customer",
> "Query": "ParseCustomer", "IsEnabled" : true, "IsTransactional": true
> }\]'
>
> .create function ifnotexists with (docstring = 'Add ingestion time to
> raw data') ParseSalesOrderHeader (){
>
> SalesOrderHeader
>
> \| extend DaysShipped = datetime_diff('day', ShipDate, OrderDate)
>
> \| extend IngestionDate = ingestion_time()
>
> }
>
> .alter table SilverSalesOrderHeader policy update @'\[{"Source":
> "SalesOrderHeader", "Query": "ParseSalesOrderHeader", "IsEnabled" :
> true, "IsTransactional": true }\]'
>
> .create function ifnotexists with (docstring = 'Add ingestion time to
> raw data') ParseSalesOrderDetail () {
>
> SalesOrderDetail
>
> \| extend IngestionDate = ingestion_time()
>
> }
>
> .alter table SilverSalesOrderDetail policy update @'\[{"Source":
> "SalesOrderDetail", "Query": "ParseSalesOrderDetail", "IsEnabled" :
> true, "IsTransactional": true }\]'
>
> //GOLD LAYER
>
> // use materialized views to view the latest changes in the
> SilverAddress table
>
> .create materialized-view with (backfill=true) GoldAddress on table
> SilverAddress
>
> {
>
> SilverAddress
>
> \| summarize arg_max(IngestionDate, \*) by AddressID
>
> }
>
> .create materialized-view with (backfill=true) GoldCustomer on table
> SilverCustomer
>
> {
>
> SilverCustomer
>
> \| summarize arg_max(IngestionDate, \*) by CustomerID
>
> }
>
> .create materialized-view with (backfill=true) GoldSalesOrderHeader on
> table SilverSalesOrderHeader
>
> {
>
> SilverSalesOrderHeader
>
> \| summarize arg_max(IngestionDate, \*) by SalesOrderID
>
> }
>
> .create materialized-view with (backfill=true) GoldSalesOrderDetail on
> table SilverSalesOrderDetail
>
> {
>
> SilverSalesOrderDetail
>
> \| summarize arg_max(IngestionDate, \*) by SalesOrderDetailID
>
> }
>
> .create materialized-view with (backfill=true) GoldDailyClicks on
> table events
>
> {
>
> events
>
> \| where eventType == "CLICK"
>
> \| extend dateOnly = substring(todatetime(eventDate).tostring(), 0,
> 10)
>
> \| summarize count() by dateOnly, eventType
>
> }
>
> .create materialized-view with (backfill=true) GoldDailyImpressions on
> table events
>
> {
>
> events
>
> \| where eventType == "IMPRESSION"
>
> \| extend dateOnly = substring(todatetime(eventDate).tostring(), 0,
> 10)
>
> \| summarize count() by dateOnly, eventType
>
> }
>
> //change batching policy time to 10 seconds to reflect ingestion
> changes faster
>
> .alter tables (Address, Customer, SalesOrderHeader, SalesOrderDetail,
> SilverAddress,SilverCustomer, SilverSalesOrderHeader,
> SilverSalesOrderDetail) policy ingestionbatching
>
> \`\`\`
>
> {
>
> "MaximumBatchingTimeSpan" : "00:00:10",
>
> "MaximumNumberOfItems" : 20,
>
> "MaximumRawDataSizeMB": 300
>
> }
>
> \`\`\`

3.  Replace all on the "Explore your data" by deleting lines 1-19 and
    paste the contents of the createAll.kql file.

4.  Click
    Run<img src="./media/image69.png" style="width:6.82494in;height:3.92361in"
    alt="alt text" />

> **Note:** createAll.kql file code is available in Labfiles

<img src="./media/image70.png"
style="width:6.49236in;height:4.81042in" />

5.  Click **Save as KQL queryset**, name it +++**createAll**+++.

> <img src="./media/image71.png"
> style="width:6.49236in;height:4.87847in" />
>
> <img src="./media/image72.png"
> style="width:3.20486in;height:1.46944in" />

6.  Your tables, functions, shortcuts and materialized views should
    appear on the database pane on the left.

<img src="./media/image73.png" style="width:6.88391in;height:3.59566in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image74.png" style="width:6.94444in;height:2.83268in"
alt="alt text" />

## Task 9: Data Pipeline 

In this task we will demonstrate how to use Fabric Data Factory pipeline
to copy data from our SQL DB into our Eventhouse KQL DB
via **batch** ingestion. This type of ingestion can be a one-time thing
or scheduled to run periodically.

1.  Now, click on **RTI-Medallian** on the left-sided navigation pane.

> <img src="./media/image27.png" style="width:3.29167in;height:4.18056in"
> alt="A screenshot of a computer Description automatically generated" />

2.  On the **Real-Time Intelligence** page, from the menu bar, select
    the **New** drop-down, and from there select **Datapipeline.**

> <img src="./media/image75.png"
> style="width:4.40972in;height:4.92167in" />

3.  In the **New lakehouse** dialog box, enter +++ **Copy Address table
    +++**in the **Name** field, click on the **Create** button and open
    the New pipeline.

> <img src="./media/image76.png"
> style="width:2.69722in;height:2.03056in" />

4.  In the Build a data pipeline to organize and move your data pane
    Select the **Pipeline Activity** and select **Copy data** activity.

<img src="./media/image77.png" style="width:6.5in;height:3.84861in" />

5.  From the **Source** tab, select the Connection dropbox and select
    the "More" option.

<img src="./media/image78.png"
style="width:5.58333in;height:4.4446in" />

6.  On the **Get data** window, navigate and select **Azure SQL
    database**, then click on the **Continue** button.

<img src="./media/image79.png"
style="width:6.49236in;height:3.01528in" />

7.  Choose **Azure SQL Database**, click Continue

<img src="./media/image80.png" style="width:6.5in;height:3.13056in"
alt="alt text" />

8.  Enter the following Connection settings and click on **Connect**
    button.

    - Server: +++**adxdemo.database.windows.net+++**

    - Database: +++**aworks**+++

    - Username: +++**sqlread**+++

    - Password: +++**ChangeYourAdminPassword1**+++

<img src="./media/image81.png"
style="width:7.28509in;height:3.63826in" />

<img src="./media/image82.png"
style="width:7.32882in;height:3.69886in" />

<img src="./media/image83.png" style="width:6.5in;height:3.18611in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image84.png" style="width:6.5in;height:4.88264in"
alt="A screenshot of a computer Description automatically generated" />

9.  From the **Source** tab, select Connection Type as **Azure SQL
    Database** , select Database as **aworks** and Set Table
    to **SalesLT.Address** from the pick list. Select the **Test
    connection**. <img src="./media/image85.png"
    style="width:5.41667in;height:4.23472in" />

> <img src="./media/image86.png" style="width:4.85001in;height:4.03545in"
> alt="A screenshot of a computer Description automatically generated" />
>
> <img src="./media/image87.png"
> style="width:6.49236in;height:4.97708in" />

10. Click on **Destination** tab, set Connection to **RTIdemo**, and
    select Table **Address** from the pick list.

<img src="./media/image88.png" style="width:6.5in;height:3.81806in" />

11. From the ribbon, select **Run**.

<img src="./media/image89.png"
style="width:4.26459in;height:2.79167in" />

12. In the **Save and run?** dialog box, click on **Save and run**
    button.

<img src="./media/image90.png"
style="width:3.04514in;height:1.88611in" />

13. The **Output** tab should appear in a few seconds to monitor the
    progress.

<img src="./media/image91.png" style="width:6.5in;height:3.84236in"
alt="A screenshot of a computer Description automatically generated" />

14. The pipeline will run until you see Pipeline status
    ✅ **Succeeded**.

<img src="./media/image92.png" style="width:6.5in;height:3.70486in"
alt="A screenshot of a computer Description automatically generated" />

15. Click the **Run** button again to run it a second time. We are
    running the pipeline **twice** to show how we are deduplicating
    rows.

<img src="./media/image93.png"
style="width:6.83182in;height:3.53977in" />

16. After the pipeline ran twice successfully, let's check the data we
    copied.

<img src="./media/image94.png" style="width:7.11705in;height:3.73265in"
alt="A screenshot of a computer Description automatically generated" />

17. Now, click on **createAll** on the left-sided navigation pane.

<img src="./media/image95.png" style="width:6.49236in;height:4.5in" />

18. In the **KQL Queryset,** click the **+** sign to open a new tab to
    write a query. Copy the following query and click **Run**.

> +++Address++

<img src="./media/image96.png" style="width:6.5in;height:3.2in"
alt="A screenshot of a computer Description automatically generated" />

19. Notice 900 records are returned at the top right of the result grid.
    However, in the Azure SQL DB (source) we have 450 rows. This is
    because the pipeline was ran twice. ie. 450 x 2 = 900.

<img src="./media/image97.png"
style="width:6.49236in;height:3.32569in" />

20. In the **KQL Queryset,** click the **+** sign to open a new tab to
    write a query. Copy the following query and click **Run**.

> +++**SilverAddress**+++

<img src="./media/image98.png"
style="width:6.49236in;height:3.98472in" />

<img src="./media/image99.png" style="width:6.5in;height:3.28056in" />

21. Notice there are 900 rows and 1 additional
    column **IngestionDate** towards the right. This transformation
    occurred automatically in real-time by the KQL
    function parseAddress(). The function was defined in the [Update
    Policy](https://learn.microsoft.com/fabric/real-time-intelligence/table-update-policy?WT.mc_id=javascript-76678-cxa) when
    we
    ran [createAll.kql](https://github.com/microsoft/FabricRTIWorkshop/blob/main/kql/createAll.kql) database
    script (see lines 41-45).

<img src="./media/image100.png"
style="width:7.11806in;height:3.67133in" />

You can use .show table SilverAddress policy update to view
the **Policy** settings. Also, .show functions will show
the **Body** definition of all functions and .show
materialized-views will show the **Query**, health and more.

22. In the **KQL Queryset,** click the **+** sign to open a new tab to
    write a query. Copy the following query and click **Run**.

> +++**GoldAddress**+++

<img src="./media/image101.png"
style="width:6.8125in;height:3.63251in" />

<img src="./media/image102.png"
style="width:6.91099in;height:3.5564in" />

23. Notice there are 450 rows, since the Gold layer uses materialized
    views based on the maximum IngestionDate to show only
    the **latest** ingested rows. This transformation occurred
    automatically in real-time by
    the [materialized-view](https://portal.azure.com/?WT.mc_id=javascript-76678-cxa).
    The view was defined in
    the [createAll.kql](https://learn.microsoft.com/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview) database
    script (see lines 64-68).

<img src="./media/image103.png"
style="width:7.09163in;height:3.74432in" />

24. Repeat the steps for
    the **Customer**, **SalesOrderHeader** and **SalesOrderDetail** tables.

## Task 10: Edit the Data Pipeline** **

1.  Now, click on **Copy Address** on the left-sided navigation pane.

<img src="./media/image104.png"
style="width:4.42361in;height:3.74176in" />

2.  From the **General** tab, rename the activity to +++Address+++.

<img src="./media/image105.png"
style="width:6.49236in;height:5.26528in" />

3.  Copy/Paste or duplicate the "Address" Copy Data activity onto the
    canvas three-times (3x). One for each additional targeted tables,
    then **change the Name, Source, & Destination** accordingly.

    - Customer

<img src="./media/image106.png"
style="width:5.52778in;height:4.68385in" />

<img src="./media/image107.png"
style="width:4.875in;height:3.92031in" />

- Sales Order Header

<img src="./media/image108.png" style="width:6.5in;height:5.12847in" />

- Sales Orde Detail

<img src="./media/image109.png"
style="width:6.49236in;height:3.86389in" />

4.  Notice how the Copy Activity Mapping for
    the **SalesOrderDetail** automatically maps sql data
    type money to decimal data type in KQL. You can see this by clicking
    Import schemas under the Mapping tab of the Copy Data activity.

<img src="./media/image110.png" style="width:6.5in;height:3.67431in" />

5.  Right-click and deactivate the "Address" Copy Data activity,

> <img src="./media/image111.png"
> style="width:5.64031in;height:3.44444in" />
>
> <img src="./media/image112.png" style="width:5.72706in;height:4.23411in"
> alt="A screenshot of a computer Description automatically generated" />

6.  Then **Run** the pipeline **twice** to execute the same scenario for
    the additional three Copy Data activities. Note, they will run in
    parallel.

<img src="./media/image113.png"
style="width:6.49236in;height:3.90139in" />

<img src="./media/image114.png"
style="width:3.00764in;height:2.05278in" />

<img src="./media/image115.png" style="width:6.5in;height:3.75903in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image116.png" style="width:6.5in;height:4.18681in"
alt="A screenshot of a computer Description automatically generated" />

## Task 11: Real-Time Dashboard 

In this task, we will build a real-time dashboard to visualize the
streaming data and set it to refresh every 30 seconds.

1.  Now, click on **RTI-Medallian** on the left-sided navigation pane.

> <img src="./media/image27.png" style="width:3.29167in;height:4.18056in"
> alt="A screenshot of a computer Description automatically generated" />

2.  On the **Real-Time Intelligence** page, from the menu bar, select
    the **New** drop-down, and from there select **More options.**

<img src="./media/image117.png"
style="width:5.77292in;height:6.13611in" />

3.  Scroll down and choose **Real-Time Dashboard** under the **Real-Time
    Intelligence**.

<img src="./media/image118.png"
style="width:6.49236in;height:3.55278in" />

4.  In the **New Real-Time Dashboard** dialog box, enter +++ **RTA
    Dashboard+++**in the **Name** field, click on the **Create** button
    and open the new Real-Time Dashboard.

> <img src="./media/image119.png"
> style="width:2.80278in;height:1.75764in" />

5.  In the **RTA Dashboard** page , Click **+ Add tile**.

<img src="./media/image120.png"
style="width:6.49236in;height:4.23472in" />

6.  Click **+ Data source**.

<img src="./media/image121.png"
style="width:4.66667in;height:3.29447in" />

7.  Set the **Database** to "**RTADemo**" & click Create.

<img src="./media/image122.png"
style="width:6.52778in;height:3.81584in" />

8.  In **Create new** **data source** tab, enter Data source name as
    **RTIdemo** and click on **Create**.

<img src="./media/image123.png"
style="width:2.11111in;height:4.52536in" />

9.  Set Time rage parameter at the top left to **Last 7 days**. This
    parameter is referenced by the query in the where clause by using
    fields \_startTime and \_endTime.

10. Copy the following query and click **Run**.

> //Clicks by hour
>
> events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "CLICK"
>
> \| summarize date_count = count() by bin(eventDate, 1h)
>
> \| render timechart
>
> \| top 30 by date_count

Note: All queries are available in this script
file [dashboard-RTA.kql](https://github.com/microsoft/FabricRTIWorkshop/blob/main/dashboards/RTA%20dashboard/dashboard-RTA.kql)
is available in Labfiles folder

<img src="./media/image124.png" style="width:6.49236in;height:5.96181in"
alt="A screenshot of a computer Description automatically generated" />

11. Click on the **+Add visual**

<img src="./media/image125.png" style="width:6.49236in;height:4.98472in"
alt="A screenshot of a computer Description automatically generated" />

12. In the **Visual formatting** tab, set Tile name to +++Click by
    hour+++, set Visual type to **Area chart.** Click on the **Apply
    changes**.

> <img src="./media/image126.png" style="width:6.5in;height:3.36397in"
> alt="A screenshot of a computer Description automatically generated" />

9.  While editing the dashboard, click **Manage** on the top left, and
    dropdown the Viewing and click **Editing**.

<img src="./media/image127.png"
style="width:7.37573in;height:2.98611in" />

13. Select **Parameters**

<img src="./media/image128.png" style="width:6.5in;height:4.03852in"
alt="A screenshot of a computer Description automatically generated" />

14. Edit the "Time range" parameter by setting the Default value
    to **Last 7 Days**, click Close & Done.

<img src="./media/image129.png"
style="width:7.15059in;height:3.76705in" />

<img src="./media/image130.png"
style="width:5.93056in;height:4.32501in" />

<img src="./media/image131.png" style="width:5.12333in;height:4.13831in"
alt="A screenshot of a computer Description automatically generated" />

15. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image132.png"
style="width:5.59722in;height:4.89362in" />

16. In the query editor, **paste** the following code, then click on
    **Run** to execute the query.

> **Copy:**
>
> //Impressions by hour
>
> events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "IMPRESSION"
>
> \| summarize date_count = count() by bin(eventDate, 1h)
>
> \| render timechart
>
> \| top 30 by date_count

<img src="./media/image133.png" style="width:6.5in;height:4.54514in" />

17. Select **+Add visual**

> <img src="./media/image134.png"
> style="width:5.97917in;height:5.28926in" />

18. In the **Visual formatting** tab, set Tile name to +++ Impressions
    by hour+++, set Visual type to **Area chart.** Click on the **Apply
    changes**.

> <img src="./media/image135.png"
> style="width:6.81604in;height:3.55492in" />

<img src="./media/image136.png" style="width:6.86453in;height:3.60314in"
alt="A screenshot of a computer Description automatically generated" />

19. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image137.png"
style="width:7.22626in;height:3.60038in" />

20. In the query editor, **paste** the following code, then click on
    **Run** to execute the query.

> **Copy:**
>
> //Impressions by location
>
> events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "IMPRESSION"
>
> \| join external_table('products') on \$left.productId ==
> \$right.ProductID
>
> \| project lon =
> toreal(geo_info_from_ip_address(ip_address).longitude), lat =
> toreal(geo_info_from_ip_address(ip_address).latitude), Name
>
> \| render scatterchart with (kind = map) //, xcolumn=lon,
> ycolumns=lat)

<img src="./media/image138.png"
style="width:6.97917in;height:4.26206in" />

21. Select **+Add visual**

<img src="./media/image139.png"
style="width:6.07711in;height:4.03472in" />

22. In the **Visual formatting** tab, set Tile name to +++ Impressions
    by location+++, set Visual type to **Map.** Click on the **Apply
    changes**.

<img src="./media/image140.png"
style="width:7.18273in;height:3.72159in" />

<img src="./media/image141.png" style="width:6.5in;height:3.41736in"
alt="A screenshot of a computer Description automatically generated" />

23. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image142.png"
style="width:6.49236in;height:4.09861in" />

24. In the query editor, **paste** the following code, then click on
    **Run** to execute the query.

> **Copy**
>
> //Average Page Load time
>
> events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "IMPRESSION"
>
> //\| summarize average_loadtime = avg(page_loading_seconds) by
> bin(eventDate, 1h)
>
> \| make-series average_loadtime = avg(page_loading_seconds) on
> eventDate from \_startTime to \_endTime+4h step 1h
>
> \| extend forecast = series_decompose_forecast(average_loadtime, 4)
>
> \| render timechart

<img src="./media/image143.png" style="width:6.5in;height:2.81806in" />

25. Select **+Add visual**

<img src="./media/image144.png"
style="width:6.49236in;height:2.63611in" />

26. In the **Visual formatting** tab, set Tile name to +++**Average Page
    Load time**+++, set Visual type to **Time chart.** Click on the
    **Apply changes**.

> <img src="./media/image145.png" style="width:6.5in;height:3.34097in" />
>
> <img src="./media/image146.png" style="width:6.05025in;height:4.17399in"
> alt="A screenshot of a computer Description automatically generated" />

27. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image147.png"
style="width:6.49306in;height:4.51389in" />

28. In the query editor, **paste** the following code, then click on
    **Run** to execute the query.

> **Copy**
>
> //Clicks, Impressions, CTR
>
> let imp = events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "IMPRESSION"
>
> \| extend dateOnly = substring(todatetime(eventDate).tostring(), 0,
> 10)
>
> \| summarize imp_count = count() by dateOnly;
>
> let clck = events
>
> \| where eventDate between (\_startTime..\_endTime) and eventType ==
> "CLICK"
>
> \| extend dateOnly = substring(todatetime(eventDate).tostring(), 0,
> 10)
>
> \| summarize clck_count = count() by dateOnly;
>
> imp
>
> \| join clck on \$left.dateOnly == \$right.dateOnly
>
> \| project selected_date = dateOnly , impressions = imp_count , clicks
> = clck_count, CTR = clck_count \* 100 / imp_count
>
> <img src="./media/image148.png" style="width:6.5in;height:4.13611in" />

29. Select **+Add visual**

<img src="./media/image149.png" style="width:6.49236in;height:4.87847in"
alt="A screenshot of a computer Description automatically generated" />

30. In the **Visual formatting** tab, set Tile name to +++
    **Impressions**+++, set Visual type to **Stat and** set data to
    **Impressions.** Click on the **Apply changes**.

<img src="./media/image150.png" style="width:7.26144in;height:3.73674in"
alt="A screenshot of a computer Description automatically generated" />

31. Click the 3-dots (...) at the top right of the tile you just created
    to **Duplicate** it two more times.

<img src="./media/image151.png"
style="width:7.28472in;height:3.79024in" />

32. Name the 2nd one **Clicks**, set the Data value column
    to **clicks**, then Apply changes.

<img src="./media/image152.png" style="width:6.5in;height:3.4375in" />

<img src="./media/image153.png"
style="width:6.83917in;height:3.43056in" />

<img src="./media/image154.png"
style="width:6.49306in;height:3.43056in" />

<img src="./media/image155.png" style="width:6.5in;height:3.35903in"
alt="A screenshot of a computer Description automatically generated" />

33. Click the 3-dots (...) at the top right of the tile

<img src="./media/image156.png"
style="width:7.0625in;height:3.53125in" />

34. Name the 3rd **Click Through Rate**, set the Data value column
    to **CTR**, then Apply changes.

<img src="./media/image157.png"
style="width:7.30132in;height:3.76389in" />

<img src="./media/image158.png"
style="width:6.49306in;height:3.35417in" />

<img src="./media/image159.png" style="width:7.27in;height:3.63889in" />

<img src="./media/image160.png"
style="width:7.273in;height:3.88515in" />

35. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image161.png"
style="width:6.48611in;height:3.34028in" />

36. In the query editor, **paste** the following code, then click on
    **Run** to execute the query

Copy

//Avg Page Load Time Anomalies

events

\| where eventDate between (\_startTime..\_endTime) and eventType ==
"IMPRESSION"

\| make-series average_loadtime = avg(page_loading_seconds) on eventDate
from \_startTime to \_endTime+4h step 1h

\| extend anomalies = series_decompose_anomalies(average_loadtime)

\| render anomalychart

<img src="./media/image162.png" style="width:6.49306in;height:3in" />

37. Select **+Add visual**

<img src="./media/image163.png" style="width:6.5in;height:3.02083in" />

38. In the **Visual formatting** tab, set Tile name to +++ Avg Page Load
    Time Anomalies+++, Click on the **Apply changes**.

<img src="./media/image164.png"
style="width:7.43626in;height:3.77778in" />

<img src="./media/image165.png" style="width:6.5in;height:4.69306in" />

39. While editing the dashboard, click **Home** on the top left, and
    select **New title** to proceed with the next visuals.

<img src="./media/image166.png" style="width:6.5in;height:4.77778in" />

40. In the query editor, **paste** the following code, then click on
    **Run** to execute the query

**Copy**

//Strong Anomalies

events

\| where eventDate between (\_startTime..\_endTime) and eventType ==
"IMPRESSION"

\| make-series average_loadtime = avg(page_loading_seconds) on eventDate
from \_startTime to \_endTime+4h step 1h

\| extend anomalies = series_decompose_anomalies(average_loadtime,2.5)

\| mv-expand eventDate, average_loadtime, anomalies

\| where anomalies \<\> 0

\| project-away anomalies

<img src="./media/image167.png" style="width:6.5in;height:3.77778in" />

<img src="./media/image168.png"
style="width:6.49306in;height:3.95833in" />

41. In the **Visual formatting** tab, set Tile name to +++ Strong
    Anomalies+++, set Visual type to **Map.** Click on the **Apply
    changes**.

<img src="./media/image169.png"
style="width:6.49306in;height:3.375in" />

<img src="./media/image170.png" style="width:6.5in;height:3.46389in"
alt="A screenshot of a computer Description automatically generated" />

42. While editing the dashboard, click **Manage** \> **Auto refresh**.

<img src="./media/image171.png"
style="width:6.48611in;height:3.31944in" />

43. Set it to **Enabled**, and **Default** refresh rate to **30
    seconds**, click Apply

<img src="./media/image172.png"
style="width:7.26709in;height:3.8125in" />

44. Click **Home** and then **Save**.

<img src="./media/image173.png"
style="width:6.49306in;height:3.68056in" />

<img src="./media/image174.png"
style="width:6.49306in;height:3.4375in" />

##  Task 12: Reflex** **

1.  Now, click on **RTA Dashboard** on the left-sided navigation pane.

<img src="./media/image175.png"
style="width:6.49306in;height:4.375in" />

2.  In the **RTA Dashboard** pane, select **Home** tab and dropdown the
    **set alert** in **Click by hour**

<img src="./media/image176.png"
style="width:6.49306in;height:3.91667in" />

3.  In the Set alert tab**, Select Condition +++Becomes greater
    than+++,**Set Value to **250,**Action **choose Message me in
    Teams.** Select the **item to Create a new item** and click
    **Create.**

<img src="./media/image177.png"
style="width:2.90972in;height:5.77778in" />

## Task 13: Stop the notebook** **

> At this point you've completed the lab, so you may stop running the
> notebook.

1.  Open the notebook "Generate synthetic events" from your workspace
    and click **Stop** on the last code cell if its still running.

<img src="./media/image178.png" style="width:3.25in;height:5.69444in" />

2.  You can click **Cancel All** on the top menu or click the stop
    red-square button to Stop session. These only appear when your
    session is active or the notebook is running.

<img src="./media/image179.png" style="width:6.5in;height:3.26389in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image180.png" style="width:6.5in;height:2.83056in"
alt="alt text" />

## Task 14: Clean up resources

1.  Select your workspace, the **RTI-Medallian** from the left-hand
    navigation menu. It opens the workspace item view.

<img src="./media/image181.png"
style="width:4.73611in;height:5.875in" />

2.  Select the ***...*** option under the workspace name and
    select **Workspace settings**.

<img src="./media/image182.png" style="width:6.5in;height:2.95833in" />

3.  Select **General** and **Remove this workspace.**

<img src="./media/image183.png" style="width:6.5in;height:6.21667in"
alt="A screenshot of a computer Description automatically generated" />

4.  Open your browser, navigate to the address bar, type or paste the
    following URL:
    [<u>https://portal.azure.com/</u>](https://app.fabric.microsoft.com/),
    then press the **Enter** button.

5.  To delete the fabric account, navigate to **Azure portal Home**
    page, click on **Resource groups**.

<img src="./media/image184.png" style="width:7.14135in;height:4.12917in"
alt="A screenshot of a computer Description automatically generated" />

6.  Click on the resource group that you’ve created.

<img src="./media/image185.png" style="width:6.5in;height:3.975in"
alt="A screenshot of a computer Description automatically generated" />

7.  In the Resource group home page, select the **Delete resource
    group** button

<img src="./media/image186.png" style="width:7.33067in;height:3.60417in"
alt="A screenshot of a computer Description automatically generated" />

8.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button.

<img src="./media/image187.png" style="width:5.41667in;height:7.125in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image188.png" style="width:2.94167in;height:1.6in"
alt="A screenshot of a computer error Description automatically generated" />

<img src="./media/image189.png" style="width:4.35038in;height:1.89183in"
alt="A screenshot of a computer Description automatically generated" />
