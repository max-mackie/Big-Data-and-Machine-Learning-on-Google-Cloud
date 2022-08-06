# Modernising Data Lakes and Data Warehouses with Google Cloud
## Introduction to Data Engineering
### The role of a data engineer
A data engineer builds data pipelines to get data into a place such as a dashboard, report or ML model from where the business can make data driven decisions.

A data lake brings together data from across the enterprise into a single location. The key considerations that need to be considered when building a data lake are:
* Does your data lake handle all teh types of data you have?
* can it all fit into a cloud storage bucket?
* If you have a RDBMs, you might need to put the data in cloud SQL, a managed database rather than cloud storage.
* Can it elastically scale to meet the demand as your data collected increases - will you run out of disk space (not such a problem with cloud solutions)
* Does it support high throughput ingestion?
* What is the network bandwidth?
* Do you have edge points of presence?
* Is there fine grained access control to objects?
* So users need to seek within a file, or is it enough to get a file as a whole?
* Cloud storage is ablob storage, so you might need to think about the granularity of what you store
* Can other tools connect easily
* How do tehy access the store?

It is important not to lose sight of the fact that the purpose of a data lake is to make data accessible for analytics.

Cloud storage buckets are good options for staging all of your raw data in one place before building transformation pipelines into your data warehouse. Cloud storage buckets are oftwn used to store many different raw data files such as CSVs and JSON before being loaded or queriesd directly from BigQuery as a data warehouse. Other GCP products and services can easilty integrate with your bucket once you have it set up and loaded with data.

### Data Engineering Challenges
As a data engineer you will often find a few problems when building data pipelines e.g.
1. It can be difficult to access the data that you need
2. Data may not have the quality that is required by the analytics or machine learning model
3. The transformations require computational resources that mighjt not be available to you
4. You can run into challenges around query performance and being able to run all of the queries

The first challenge of accessing data may arise because data is scattered across a veriety of marketing products and CRM software. Finding a tool to analyse all this data is difficult as the data comes from different organisations, different tools and different schemas. Data access is so difficult primarily because data in many businesses is siloed by departments and each department creates its own trasnsactional system to support its own business process

The second challenge is that cleaning formatiing and getting the data ready for insights requies you to build ETL pipelines. ETL pipelines are usually necessary to ensure data accuracy and quality. The clean and transformed data is typically stored in a data warehouse rather than a data lake. A data warehouse is a consolidated place to store the data, and the data and all the data are easily joinable and queryable. Unlike a data lake where the data is in the raw format in the data warehouse, the data is stored ina way that makes it efficient to query. 
![image](https://user-images.githubusercontent.com/80007111/183134715-87032197-ef90-4431-b045-c2b410f4ffaa.png)

The third challenge arises when large amounts of data consolidation and clean up are required. In an on premise system, data engineers will need to manage server and cluster capacity to make sure enough capacity is available for the ETL jobs. The problem is that the compute needed for these jobs is not constant over time. Therefore when trafic is low you are wasting money and when it is high jobs take too long. Once data is in the warehouse you need to optimise the queries that users are running to make the most efficient use of compute resources. If you are managing an on prmise data analytics cluser, you will be responsible for choosing a query engine and keeping it up to data as well as provisioning anymore servers for additional capacity

### Introduction to BigQuery
BigQuery is Google Cloud's petabytes scale serverless data warehouse. 
* The BigQuery Service replaces the typical hardware setup for a traditional data **warehouse**. 
* Data sets are collections of tables that can be divided along business lines or a given analytical domain. 
  * Each dataset is tied to a Google Cloud project. 
* A data lake might contain files in cloud storage or Google drive or even transactional data from Cloud big table. BigQuery can define a schema and issue queries directly on external data as Federated Data Sources. 
* Database tables and views function the same way in big query as they do in a traditional data warehouse. Allowing big query to support queries written in a standard SQL dialect
* Identity and access management is used to grant permission to perform specific actions in BigQuery. This replaces the SQL grant and revoke statements that are used to manage access permissions in traditional SQL databases.
![image](https://user-images.githubusercontent.com/80007111/183138761-deed6600-d734-498a-95fb-b99135d11bdb.png)

Cloud allows data engineers to spend less time managing harware and enabling scale.
![image](https://user-images.githubusercontent.com/80007111/183140239-cb1100b9-388b-41ca-85e3-8bb26b33b741.png)

You don't need to provision resources before using BigQuery. Unlike many RD BMS systems, BigQuery allocates storage and query resources dynamically based on your usage patterns. 
* Storage resources are allocated as you consume them and deallocated as you remove data or drop tables. 
* Query resources are allocated according to query type and complexity. Each query uses some number of slots, which are units of computation that comprise a certain amount of CPU and RAM
![image](https://user-images.githubusercontent.com/80007111/183140453-6f87f758-6d4d-4761-b472-528cd8ebae25.png)


### Data Lakes and Data Warehouses
First Raw data gets replicated and stored in a data lake. Then In order to make that data usable ETL pipelines are used to make the data usable and store this more usable data in a data warehouse. 
![image](https://user-images.githubusercontent.com/80007111/183240663-a9e67fa3-98a0-4c75-a39c-3cce38b2277a.png)

The key considerations when deciding between data warehouse options are:
* Will the data warehouse be fed by a batch pipeline or by a streaming pipeline? 
* Does the warehouse need to be accurate up to the minute or is it enough to load data into it once a day or once a week? 
* Will the data warehouse scale to meet my needs? 
* Many cluster-based data warehouses will set per cluster concurrent query limits. 
  * Will those query limits cause a problem? 
  * Will the cluster size be large enough to store and traverse your data? 
* How is the data organized, cataloged, and access controlled? 
* Will you be able to share access to the data to all your stakeholders? 
* What happens if they want to query the data? 
* Who will pay for the querying? 
* Is the warehouse designed for performance? 
* Again, carefully consider concurrent query performance, and whether that performance comes out of the box or whether you need to go around creating indexes and tuning the data warehouse. 
* Finally, what level of maintenance is required by your engineering team? 

Traditional data warehouses are hard to manage and operate. 
* They were designed for a batch paradigm of data analytics, 
* and for operational reporting needs. 
* The data in the data warehouse was meant to be used by only a few management folks for reporting purposes.
BigQuery is a modern data warehouse that changes the conventional mode of data warehousing.
![image](https://user-images.githubusercontent.com/80007111/183241023-6d596e2b-5f82-4e4d-8d92-66ec3190cf3d.png)

We looked at getting data into BigQuery by running ETL pipelines, but, you can also to treat BigQuery as a query engine and allow it to query the data in place. 
* For example, you can use BigQuery to directly query database data in Cloud SQL. That is managed relational databases like PostgreSQL, MySQL, and SQL Server. You can also use BigQuery to directly query files on Cloud Storage as long as these files are in formats like CSV or parquet. 

The real power comes when you can leave your data in place and still join it against other data in the data warehouse.

### Transactional Databases vs Data Warehouses
In this section, well explore the differences between databases and data warehouses and the Google Cloud solutions for each workload

If you have SQL Server MySQL or PostgresSQL as your relational database, you can migrate it to Cloud SQL, which is Google Cloud's fully managed relational database solution.
*  Cloud SQL delivers high performance and scalability with up to 64 terabytes of storage capacity, 60,000 IOPS and 624 gigabytes of RAM per instance. Cloud sequal can handle growing needs with storage auto-scale

"Why not simply use Cloud SQL for reporting workflows?
* When considering Cloud SQL, be aware that Cloud SQL is optimized to be a database for transactions or rights, and BigQuery is a data warehouse optimized for reporting workloads (mostly reads).
* Cloud SQL databases are RECORD-based storage, meaning the entire record must be opened on disk. Even if you just selected a single column in your query. BigQuery is COLUMN based storage, which allows for really wide reporting schemas.

![image](https://user-images.githubusercontent.com/80007111/183244355-aa0c426f-9d43-40ed-a0ad-d181a6e5bd96.png)
* operational systems, like our relational databases, are our raw data sources on the left.
* upstream data sources get gathered together in a single consolidated location in our Data Lake, which is designed for durability and high availability.
* Once in the data lake, the data often needs to be processed via transformations that then output the data into our data warehouse, where it is ready for use by downstream teams
  * downstream teams include ML team that may build a pipeline to get features for their models
  * An engineering team may be using our data as part fo their data warehouse
  * BI team may want to build dashboards using some of out data

### Partner Effectively with Other Data Teams
 There are many data teams that rely on your data warehouse and partnerships with data engineering to build and maintain new data pipelines. The three most common clients are,
* The Machine Learning Engineer, 
* The Data or BI analyst 
* Other data engineers.

The machine learning Engineer

They will often partner with data engineering teams to build pipelines and data sets for use in their models. Two common questions you may get asked are:
* How long does it take for a transaction to make it from raw data all the way into the data warehouse
  * They're asking this because any data that they train their models on must also be available at prediction time as well.
  * If there is a long delay in collecting and aggregating the raw data, it will impact the ML team's ability to create useful models 
* How difficult would it be to add more columns or rows of data into certain datasets
  * the ML team relies on teasing out relationships between the columns of data and having a rich history to train models on.
  * You will earn the trust of your partner ML teams by making your data sets easily discoverable, documented and available to ML teams to experiment on quickly.
A unique feature of Big Query is that you can create high performing machine learning models directly in Big Query, using just sequel by using Big Query ML

BI and Data Analyst Teams
* BI and data analyst teams rely on good clean data to query for insights and build dashboards. 
* These teams need,
  * data sets that have clearly defined schema definitions
  * The ability to quickly preview rows 
  * the performance to scale too many concurrent dashboard users
One of the Google Cloud products that helps manage the performance of dashboards is big Query BI Engine
* BI Engine is a fast in memory analysis service that is built directly into BigQuery and available to speed up your business intelligence applications.

Other Data Engineers

Other data engineers will rely on the up time and performance of your data warehouse and pipelines for their downstream data lakes and data warehouses. They will often ask;
* How can you ensure that the data pipeline we depend on will always be available
* We are noticing high demand for certain really popular datasets. How can you monitor and scale the health of your entire data ecosystem

One Popular way is to use the built in cloud monitoring of all resources on Google Cloud. you can set up alerts and notifications for metrics like query count or bytes of data processed. So you can better track usage and performance. Cloud audit logs can be used to view actual query job information to see granular level details about which queryies were executed and by whom.

### Manage data access and governance
ngineering team will be asked to set up data access policies and overall governance of how data is to be used.
* This includes critical topics such as privacy and security
A data governance model should outline
* who should and should not have access
* How is personally identifiable information like phone numbers or email addresses handled
* basic tasks like how will our end users discover the different data sets we have for analysis?

One solution for data governance is the Cloud data Catalog and the Data Loss Prevention API. 
* The Data Catalog makes all the metadata about your data sets available to search for your users.
  * group data sets together with tags flag certain columns as sensitive, etc.
  * Data Catalog provides a single unified user experience for discovering those data sets quickly.
* Data Catalog provides a single unified user experience for discovering those data sets quickly.

### Buld Production-Ready Pipelines
Once the data lakes and data warehouses are set up and governance policy is in place it is time to productionalise the operation and automate and monitor as much as possible
* It has to be an end-to-end and scalable data processing system.

Common questions that should be asked at this stage are:
* How can we ensure pipeline health and data cleanliness
* How do we productionalise these pupelines to minimise maintenace and maximise up-time?
* How do we respond and adapt to changing schemas and business needs
* Are we using the latest data engineering tools and best practaces

### Recap
![image](https://user-images.githubusercontent.com/80007111/183258857-003055da-6591-467a-93db-c277ba1d28dc.png)
https:github.com/gregsramblings/google-cloud-4-words

### Using BigQuery to do Analysis
```sql
WITH bicycle_rentals AS (
  SELECT
    COUNT(starttime) as num_trips,
    EXTRACT(DATE from starttime) as trip_date
  FROM `bigquery-public-data.new_york_citibike.citibike_trips`
  GROUP BY trip_date
),
rainy_days AS
(
SELECT
  date,
  (MAX(prcp) > 5) AS rainy
FROM (
  SELECT
    wx.date AS date,
    IF (wx.element = 'PRCP', wx.value/10, NULL) AS prcp
  FROM
    `bigquery-public-data.ghcn_d.ghcnd_2015` AS wx
  WHERE
    wx.id = 'USW00094728'
)
GROUP BY
  date
)
SELECT
  ROUND(AVG(bk.num_trips)) AS num_trips,
  wx.rainy
FROM bicycle_rentals AS bk
JOIN rainy_days AS wx
ON wx.date = bk.trip_date
GROUP BY wx.rainy
```
* Above shows how queries can use different datasets withing a query
