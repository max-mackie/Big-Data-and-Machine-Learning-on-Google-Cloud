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

## Building Data Lakes
### Introduction to Data Lakes
A data lake is a place where you can securely store various types of data of all scales for processing and analytics.
![image](https://user-images.githubusercontent.com/80007111/183259977-da98e8f4-7e4c-4c31-9a35-a202774a5e69.png)

Components of a Data Engineering ecosystem
* Data sources
* Data sinks
  * Central data lake repository
  * Data warehouse
* Data pipelines (batch and streaming)
* High-level orchestration workflows

Construction analogy
1. Raw materials need to be brought to the job site (into the data lake)
2. Materials need to be cut and transformed for purpose and stored (pipelines to data sinks)
3. The actual building  is hte new insight or ML model etc.
4. The supervisor directs all aspects and teams on the project (workflow orchestration)

Here you can see that the data lake is the first line of defense in an enterprise data enviroment. It is general aka "give me whatever data at whatever volumne, variety, formats and velocity that you got, i can take it" The data lake generally serves as a single consolidated place for all the raw data like a durable staging area.

The suite of big data products on Google Cloud
![image](https://user-images.githubusercontent.com/80007111/183260392-d31aeab1-82b9-4dc6-9a1b-55420131bb72.png)

Data lake vs data warehouse
* A data lake is a capture of every aspect of your business operation. The data is tored in its natural/raw format, usually as object blobs or files
  * Retain all data in its native format
  * Support all data types and all users
  * Adapt to changes easily
  * Tends to be application-specific
* In contrast, a data warehouse typically has the following characteristics
  * Typically loaded only after a use case is defined
  * Processed/organised/transformed
  * Provide faster insights
  * Current/historical data for reporting
  * Tends to have consistent schema shared across applications 

### Data Storage and ETL Options on Google Cloud
Storage options for your data on Google Cloud
* Cloud Storage
  * catch all storage
* Cloud SQL
  * relational databases
* Cloud Spanner
  * relational databases
* Firestore
  * NoSQL data
* Cloud Bigtable
  * NoSQL data

The path your data takes to get to the cloud depends on 
* Where your data is now
* How big your data is
* Where it has to go (data sink)
* How much transformation is needed

The method you use to load data depends on how much transformation is needed

Extract and Load (EL)
* Used when data can be imported as is into a system. e.g. when importing data from a database where the source and the target have the same schema
Extract Transform and Load (ETL)
* Used when the transformations you want to perform are small and do not greatly reduce the amount of data that you have.
* Eg in BigQuery you could load the data then use SQL to transform the data and write a new table
Extract Load and Transform (ETL)
* when you want to extract the data, apply a bunch of processing to it, and then load it into the Cloud product. 
* This is usually what you pick when this transformation is essential or if this transformation greatly reduces the data size, 
  * so by transforming the data before loading it into the Cloud, you might be able to greatly reduce the network bandwidth that you need.
* If you have your data in some binary proprietary format and you need to convert it before loading, then you need ETL as well. 
* ETL is a data integration process in which transformation takes place in an intermediate service before it is loaded into the target. 
  * For example, the data might be transformed in a data pipeline like dataflow before being loaded into BigQuery.
  
BigQuery is slightly unique as you may end up not even loading the data into BigQuery and still can query off of it. 
* Avro, ORC, and Parquet files are all now supported for federated querying.

### Building a Data Lake using Cloud Storage
Qualities that Cloud Storage contributes to data engineering solutions:
* Persistence
  * Data in cloud storage persists beyond the lifetime of VSs or clusters. It is also relatively inexpensive compared to compute so it can be advantagous to cashe the results of previous computations in cloud storage. Cloud storage is an boject store so it stores and retrieves binary objects without regard to what data is contained in the objects
* Durability
  * Data stored in cloud storage will stay there forever 
* Strong consistency
  *  You can share data globaly but it is encrypted and completely controlled and private if you want it to be. (might not apply here?)
* Availability
  * I tis a global service, and you can reach the data from anywhere
* High throughput  
  * Data is served up with moderate latency and high throughput.

As a data engineer you need to understand how cloud storage accomplishes these apperently contradictory qualities and when and how to employ them.

A lot of cloud storages amazing properties have to do with the fact that it is an object store and other features are build on top of that. The 2 main entities in cloud storage are 
* Buckets
  * Buckets are containers for objects
  * They are identified in a single global unique name space so every bucket name is unique
  * Buckets are associated with a particular region or with multiple regions. 
    * Choosing a region close to where the data will be processed will reduce latency, and if processing data within the region, you will save on network egress charges
* Objects
  * Objects exist inside of buckets and not apart from them
  * When an Bucket is stored, Cloud Storage replicates it and if an object is ever lost or corrupted it replaces it with the copy
    * This is how cloud storage achieves great reliability 
  * When an object i sretrieved, it is served from the closes replica to the requester 
    * This is how low latency is acheived
  * Multiple requesters could be retrieving the objects at the same time from different replicas
    * This is how high throughput is achieved
  * Metadata is information about the object. Additional cloud stroage features use the metadata for purposes such as access control, compression, encryption and lifecycle management
  ![image](https://user-images.githubusercontent.com/80007111/183284184-4938d077-6a92-45ce-9e0f-1d212bd9d499.png)

You may have a variety of storage requirements for a multitude of use cases. Cloud storage offers different classes to cater for these requirements, and these are based on how often data is accessed.
![image](https://user-images.githubusercontent.com/80007111/183284256-ebc37770-54e1-4817-83e6-2a04d534c60c.png)

Cloud Storage uses the bucket name and object name to simulate a file system
![image](https://user-images.githubusercontent.com/80007111/183285065-c188909f-a1f8-4248-9dd8-68e671e2ae8c.png)

The forward slashes are just characters in the name. If this path we're in a file system, it would appear as a set of nested directories, beginning with "de" class. 
* Now, for all practical purposes, it works like a file system. But there are some differences. 
  * For example, imagine that you wanted to move all the files in the 0 2 directory to the 03 directory inside the modules directory. you have to search through all objects in the bucket looking for all objects that contain 02 in the right position. For large buckets this can take a long time.
    * During the move there will be list inconsistency with some files in the old directory and some in hte new directory

A best practice when nameing is to avoid the use of sensitve information as part of bucket name. Because bucket names are in a global name space. 

Cloud storage can be accessed using a file access method that allows you, for example, to use a copy command from a local file directly to cloud storage. Use the tool G Sutil to do this. Cloud storage can also be accessed over the Web. The site, storage dot cloud dot google dot com, uses TLS https to transport your data, which protects credentials as well as data in transit.

object management features
* You can set a retention policy on all objects in the bucket, 
  * e.g. the object should expire after 30 days. 
* You can also use versioning so that multiple versions of an object are tracked and available if necessary. 
* You might even set up lifecycle management to automatically move objects that haven't been accessed in 30 days to near line and after 90 days to cold line.

### Secure Cloud Storage
Securing Your Data Lake running on Cloud Storage is of paramount importance. Cloud Storage implements two completely separate but overlapping methods of controlling access to objects. 
* IAM Policy
  * IAM is standard across the Google Cloud.
  * It is set at the bucket level and applies uniform access rules to all objects within a bucket#
  * IAM provides project roles and bucket roles including
    * Bucket reader
    * Bucket writer
    * Bucket owner
    * The ability to create or change access control lists is an IAM bucket role and the ability to create and delete buckets and to set IAM Policy is a project level role.
* Access Control Lists.
  * These can be applied at the bucket level or on an individual object so provides more fine-grained access control.
  * When buckets are created you are offered to the option of disabling access lists and only ising IAM
  
All data in Google Cloud is encrypted at rest and in transit. There is no way to turn this encryption off. The encryption is done by Google using encryption keys that we manage, Google Managed Encryption Keys or GMEK. 2 levels of encryption are used:
* First, the data is encrypted using a data encryption key. 
* Then the data encryption key itself is encrypted using a key encryption key or KEK. 
  * The KEKs are automatically rotated on a schedule, and the current KEK stored in Cloud KMS, Cloud Key Management Service.
  
This is automatic behaviour but you can manage the KEK yourself.
![image](https://user-images.githubusercontent.com/80007111/183289719-200f7897-8c69-4f77-b5a6-13c171447532.png)
* Which data encryption option you use depends on business, legal and regulatory requirements. Please talk to your company's legal counsel.

The fourth encryption option is client-side encryption.
* Client-side encryption simply means that you have encrypted the data before it is uploaded and have to decrypt the data yourself before it is used. 
  * Cloud Storage still performs GMEK, CMEK or CSEK encryption on the object. It has no knowledge of the extra layer of encryption you may have added.
 
Specialist use cases supported by cloud storage:
* Cloud storage supports logging of data access and these logs are immutable.
* For audit purposes, you can place a hold on an object and all operations that could change or delete the object are suspended until the hold is released.
* You can also lock a bucket and no changes or deletions can occur until the lock is released.
* Finally, there is the lock retention policy previously discussed and it continues to remain in effect and prevent deletion whether a bucket lock or object hold are enforce or not.
  * Data locking is different from an encryption. Where encryption prevents someone from understanding the data, locking prevents them from modifying the data.
* Decompressive coding
* Requester pays
* Signed URLs for anonymous sharing
* Period expirations
* Composite objects

### Store all sorts of data types
Cloud Storage is not the only choice when it comes to storing data on Google Cloud
![image](https://user-images.githubusercontent.com/80007111/183290577-9b763ec1-d39b-4512-9edd-39fc9c0e89e1.png)
Cloud Storage
* Even though it has low-latency it is not low enough to support high-frequency writes
* For transactional workloads use Cloud SQL or Firestore depending on whether you want to use AQL or NoSQL.
* You dont want to use it for analytics unstructured data as you will spend significant amount of compute parsing data depending on the latency required
  * It is better to use Cloud Bigtable or BigQuery for analytics workloads on structured data depending on the latency required.

Transactional vs Analytics Workloads
![image](https://user-images.githubusercontent.com/80007111/183292898-c4b26935-a2ab-4fdc-aa3e-27a05c9e3831.png)
* Transactional systems 
  * Transactional systems are write-heavy (80% writes and 20% reads)
  * These tend to be operational systems that require updating frequently (e.g. inventory system)
* Analytical systems
  * These can be periodically populated from the operation systems.
    * e.g. this could be used once a day to generate a daily sales report
  * Such a report will have to read a bunch of data but wont be required to write much
  
OLAP systems (Online Analytical Processing)
OLTP system (Online Transaction Processing)
* Data engineers build the pipelines to populate the OLAP system from the OLTP system
  * One simple way might be to export the database as a file and load it into the data warehouse (EL)
* On Google Cloud the data warehouse tends to be BigQuery
  * There is a limit to the size of the data that you can load directly to BigQuery as network might be a bottlenexk
  * Instead we can load first into Cloud Storage then load to BigQuery.
    * This is because Cloud Storage supports multithreaded resumable loads (gsutil -m)
    * This will also be faster due to the high throughput it offers
* For transactional workloads there are a few options for relational databases.
  * CloudSQL
    * This is the defaultchoice 
    * It is much more cost effective
  * Cloud Spanner
    * But if you require a globally distributed database then use Cloud Spanner
    * The true time capability of Spanner is very appealing for this kind of use case
    * You may also choose Spanner if your database is too big to fit into a single CloudSQL
      * If the database is many gigabytes you need a distributed database
    * The scalability of Spanner is very appealing for this use case
    
* For analytical work the defualt choice is BigQuery as it is the most cost effective
  * However if you require high throughput inserts more than millions of rows per second or you require a low latency on the order of milliseconds then use Cloud Bigtable.

### Cloud SQL as a relational Data Lake
Cloud SQL is the default choice for OLTP (online transaction processing)
* Clouds SQL is a service that delivers fully managed relational databases.
* It handes tasks such as
  * applying patches and updates 
  * Managing backups 
  * Configuring replications.

Cloud SQL can be used with App Engine using standard drivers. You can configure a Cloud SQL instance to follow an App Engine application

Compute Engine instances can be authorised to access Cloud SQL instances using an external IP address. Cloud SQL instances can be configured with a preferred zone.

Cloud SQL can be used with external applications and clients. Standard tools can be used to administer databases. External read replicas can be configured

Security is managed
* Google security
  * Cloud SQL customer data is encrypted when on Google's internal networks and when stored in database tables, temporary files and backups. 
  * Every Cloud SQL instance includes a network firewall allowing you to control network access to your database instance by granting access.
* Managed backups
  * upto 7 backups for each instance
* Vertical scaling (read and write)
  * You can vertically scale Cloud SQL just increase your machine size scale up to 64 processor cores and more than 100 gigabytes of RAM.
* Horizontal scaling (read)
  * Horizontally you can quickly scale out with read replicas. Google Cloud SQL supports three read replica scenarios. 
    * Cloud SQL instances replicating from a Cloud SQL primary instance. 
    * Cloud SQL instances replicating from an external primary instance. 
    * External, MySQL instances replicating from a Cloud SQL primary instance.
* Automatic replicaiton
* If you need horizontal read write scaling consider Cloud Spanner

In the special case of Fail Over, Cloud SQL supports this
* Clouds SQL instances can be configured with a fail over replica in a different zone in the same region.
  * In the event of a data center outage, a Cloud SQL instance will automatically become available in another zone.
* If the primary database has issue not caused by outage then fail over doesnt occur hut it can be initiated manually
  * In this case the failover replica is charged as a separate instance
  * Existing connections to the instance are closed
  * The replica becomes the primary
* You can use the failover replica as a read replica to offload read operations from the primary

Fully managed vs Serverless
* By fully managed, we mean that the service runs on hardware that you can control. (You can SSH into a Cloud SQL instance) But Google helps you manage the instance by automating backups and setting up fail over instances, etc.
* Serverless is the next step up. You can treat a serverless product as just an API that you are calling. You pay for using the product but don't have to manage any servers.

modern serverless data management architecture
![image](https://user-images.githubusercontent.com/80007111/183374172-bd31dafb-7509-4b4d-ba76-8445859f171a.png)

* Big Query is serverless, so are Pub/Sub for asynchronous messaging and DataFlow for parallel data processing.
  * You can think of cloud storage as being serverless as well. Sure, cloud storage uses discs, but you never actually interact with the hardware. 
  * One of the unique things about Google Cloud is that you can build a data processing pipeline of well designed components, all of which are fully serverless. 
* Data process, on the other hand, is fully managed. It helps you run spark and hadoop workloads without having to worry about setup. 
* Given the choice between doing a brand new project on Big Query or data flow, which are serverless and data processing, which is fully managed. 
  * All other things being equal, choose the serverless product.

### Loading Taxi Data into Google CLoud SQL 2.5
#### Activate Google Cloud Shell
Google Gloud shell is a virtual machine that is loaded with development tools it offers 5gb home directory and provides command-line access to your GCP resources. To launch go to the top right of GCP concole and click the Open Cloud Shell button.
* gcloud is the command-line tool for GCP and it comes pre installed on Cloud Shell
  * To list the current account name use: _gcloud auth list_
  * To list the project ID use: _gcloud config list project_
  * For full documentation see https://cloud.google.com/sdk/gcloud
 
#### Preparing your environment
Create environment variables that will be used later in the lab for your project ID and the storage bucket that will contain your data
```
export PROJECT_ID=$(gcloud info --format='value(config.project)')
export BUCKET=${PROJECT_ID}-ml
```

#### Create a Cloud SQL instance
1. To create a Cloud SQL instance use this code:
```
gcloud sql instances create taxi \
    --tier=db-n1-standard-1 --activation-policy=ALWAYS
```
2. Set a root password for the Cloud SQL instance:
---
gcloud sql users set-password root --host % --instance taxi \
 --password Passw0rd
---
3. When prompted for the password type Passw0rd and press enter this will update root password
4. Now create an environment variable with the IP address of the Cloud Shell:
```
export ADDRESS=$(wget -qO - http://ipecho.net/plain)/32
```
5. Whitelist the Cloud Shell instance for management access to your SQL instance:
```
gcloud sql instances patch taxi --authorized-networks $ADDRESS
```
6. Get the IP address of your Cloud SQL instance by running:
```
MYSQLIP=$(gcloud sql instances describe \
taxi --format="value(ipAddresses.ipAddress)")
```
* you can check the variable MYSQLIP: echo $MYSQLIP
  * You should get an IP address as an output

7. Create the taxi trips table by logging into the mysql command line interface
```
mysql --host=$MYSQLIP --user=root \
      --password --verbose
```
8. Pasete the following content into the command line to create the schema for the trips table
```
create database if not exists bts;
use bts;
drop table if exists trips;
create table trips (
  vendor_id VARCHAR(16),		
  pickup_datetime DATETIME,
  dropoff_datetime DATETIME,
  passenger_count INT,
  trip_distance FLOAT,
  rate_code VARCHAR(16),
  store_and_fwd_flag VARCHAR(16),
  payment_type VARCHAR(16),
  fare_amount FLOAT,
  extra FLOAT,
  mta_tax FLOAT,
  tip_amount FLOAT,
  tolls_amount FLOAT,
  imp_surcharge FLOAT,
  total_amount FLOAT,
  pickup_location_id VARCHAR(16),
  dropoff_location_id VARCHAR(16)
);
```
9. In the mysql command line interface chaeck teh import by entering the following commands:
```
describe trips;
```
10. Query the trips table:
```
select distinct(pickup_location_id) from trips;
```
* this should return an emplty set as there is no data in the dataset
* enter exit to leave the mysql interactive console

#### Add data to Cloud SQL instance
In this section we will copy the new your city taxi trips CSV that is stored on Cloud Storage locally
1. Run the following in the command line:
```
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_1.csv trips.csv-1
gsutil cp gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_2.csv trips.csv-2
```
2. Connect to the mysql interactive console to load local infile data:
```
mysql --host=$MYSQLIP --user=root  --password  --local-infile
```
3. In the mysql interactive console select the database
```
use bts;
```
5. Load the local CSV file data using local-infile:
```sql
LOAD DATA LOCAL INFILE 'trips.csv-1' INTO TABLE trips
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(vendor_id,pickup_datetime,dropoff_datetime,passenger_count,trip_distance,rate_code,store_and_fwd_flag,payment_type,fare_amount,extra,mta_tax,tip_amount,tolls_amount,imp_surcharge,total_amount,pickup_location_id,dropoff_location_id);
```
```sql
LOAD DATA LOCAL INFILE 'trips.csv-2' INTO TABLE trips
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(vendor_id,pickup_datetime,dropoff_datetime,passenger_count,trip_distance,rate_code,store_and_fwd_flag,payment_type,fare_amount,extra,mta_tax,tip_amount,tolls_amount,imp_surcharge,total_amount,pickup_location_id,dropoff_location_id);
```

#### Chekcing for data integrity
Whenever data is imported from a source it is always important to check for data integrity
e.g. lets dig into the trip distance column
```sql
select
  max(trip_distance),
  min(trip_distance)
from
  trips;
```

## Week 2: Building a Data Warehouse
### The Modern Data Warehouse
An enterprise data warehouse should consolidate data from many sources.
* A data warehouse imposes a schema, a data lake is just raw data.
* An enterprise data warehouse brings the data together, and makes it available for querying and data processing.
* All data in a data warehouse should be available for querying

What makes a data warehouse modern
* Businesses data requirements continue to grow. You want to make sure the data warehouse can deal with data sets that don't fit into memory. You want a single data warehouse that can scale from gigabytes to petabytes of data 
* You want the data warehouse to be serverless and fully NoOps. Removine the responisbilities of limited clusters and having to maintain indexes allows data analysts to carry out ad hoc queries faster
* It must support rich visulisation and reporting. It should Idealy seamlessly plug into whichever visulisation or reporting tool your business wants
* It should integrate with an exosystem of processing tools for building ETL pipelines
* Pipelines should be capable of constantly refreshing data in the warehouse in order to keep it up to data. You must be able to stream data and not rely on batch updates
* It has to support machine learning without moving the data out of the warehouse.
* It should be possible to impose enterprise grade security like data exfiltration constraints
* It should be able to share data and queries with collaboratiors

### introduction to BigQuery
BigQuery has many capabilities that make it an ideal data warehouse
* all of the above list about the modern data warehouse

* BigQuery tables are column-orientated compared to traditional or DBM tables, which are row-orientated. 
  * Row-orientated tables are efficient for making updates to data contained in fields for OLTP systems. 
  * Row-orientated tables are necessary because OLTP systems have frequent updates. 
  * Analytics is slow and row-orientated tables because they have to read all the fields in a row. And depending on the kind of indexing or key, they may have to read extra rows and fields to find the information that is requested in the query. 
* BigQuery, however, is an OLAP system, it's meant for analytics. 
  * BigQuery tables are immutable and are optimized for reading and appending data, BigQuery tables are not optimized for updating. 
  * BigQuery leverages the fact that most queries involve few columns, and so it only reads the columns required for the query. Thus making bigquery much more efficient since it is column-orientated.

BigQuery is implemented in two parts, a storage engine and an analytic engine as illustrated.
![image](https://user-images.githubusercontent.com/80007111/183470961-be552744-08a0-4f97-8897-96379c061a7d.png)
* BigQuery data is physically stored on Google's distributed file system, called colossus, which ensures durability by using a raseur?? encoding to store redundant chunks of the data on multiple physical disks. 
* Moreover, the data is replicated to multiple data centers, here are a couple of the optimizations that capacitor does, 
  * capacitor runs lengthen codes on the data so that it can reduce the amount of data needed to be read. 
  * It also reorders the data to make it more conducive for run length encoding, reordering the data is also called dictionary encoding.

You don't need to provision resources before using BigQuery, unlike many or DBMS systems, 
* BigQuery allocate storage and query resources dynamically based on your usage patterns. 
* Storage resources are allocated as you consume them and de allocated as you remove data or drop tables. 
* Query resources are allocated according to query type and complexity, 
  * each query uses some number of slots, which are units of computation that comprise a certain amount of CPU and RAM.

BigQuery is implemented using a micro service architecture, so there are no virtual machines to configure and maintain. 

Under the hood analytics throughput is measured in BigQuery slots, 
* a BigQuery slot is a unit of computational capacity required to execute sequel queries. 
* BigQuery automatically calculates how many slots are required at each stage in the query, depending on size and complexity. 
* A BigQuery slot is a combination of CPU, memory, and networking resources, it also includes a number of supporting technologies and sub services. 
  * Note that each slot doesn't necessarily have the same specification during query execution, some slots may have more memory than others or more CPU or more IO.

### Demo
Find the demo script on github at training-data-analyst/courses/data-engineering/demos/bigquery_scale.md
* github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/bigquery_scale.....

### Get started with BigQuery
How BigQuery organises data
* BigQuery organizes data tables into units called datasets these datasets are scoped to your Google Cloud project
  * By structuring data multiple scopes (datasets, projects and tables) you can structure information logically
    * e.g. separate tables into datasets pertaining to differrent analytical domains and isolating datasets from each other using project level scoping accoring to business needs.
* The project is what the billing is associated with.
* Access control is through IAM (Identity Access Management) and is at the dataset/table view (or column level)
* As with Cloud Storage, BigQuery storage encrypts data at rest and over the wire using Google-managed encryption keys.
* Authentication is through IAM and so it's possible to use Gmail addresses or Google Workspace accounts for this task.
* BigQuery provides predefined roles for controlling access to resources (custom roles can be specified)
  * An important aspect of operating a data warehouse is allowing shared but controlled ac ess against the same data to different groups of users












