# Google Cloud Big Data and Machine Learning Fundamentals
## Course Introduction
        -----------------------------------------
                2. Big Data/ML Products
        -----------------------------------------
         1. Compute Power | Storage | Networking
        -----------------------------------------
                   0. Security
        -----------------------------------------
You can think of the google cloud infrastructure in terms of these 3 layers. At the base is security and networking which lay the foundations to support all of googles infrastructure and applications. Then we have compute and storage which are decoupled so they can scale independently based on needs and on top we have big data and ML products which enable us to perform tasks to ingest, store, process, and deliver business insights, data pipelines, and ML models.
## Big Data and Machine Learning on Google Cloud
### Compute
Google offers a range of computing services:
1. Compute Engine
a IaaS offering which provides raw compute, storage and network capabilities organized virtually into resources that are similar to physical data centers. Provides maximum flexibility for those who prefer to manage server instances themselves. 
2.  Google Kubernetes Engine (GKE)
runs containerized applications in a Cloud environment, as opposed to on an individual virtual machine like Compute Engine. A container represents code packaged up with all its dependencies.
3.  App Engine
A fully managed PaaS (platform as a service) that provides access to infrasstructure appliation needs. This allows more resources to be focused on app.iation logic.
4. Cloud Functions
Executes code and responds to events like when a new file is uploaded to cloud storage. It is a completely serverless execution environment, often referred to as Functions as a service
### Storage
For proper scaling capabilities, compute and storage are decouples. (major diference between Cloud and desktop computing)
With the Compute engine you can install and run a database on a virtual machine (like in a datacenter) Alternatively GCP offers fully managed database and storage services;
1. Cloud storage
2. Cloud Bigtable,
3. Cloud SQL
4. Cloud Spanner
5. Firestore


_structured vs unstructured data_

The right option depends on the data type and the business need unstructured data includes documents, images and audio files. This is best suited to Cloud storage which has 4 primary stroage classes
1. Standard Storage - best for requently accessed data (hot data) or data that is only stored for short periods of time
2. Nearline Storage - best for infrequent data (once per month) e.g. backups, archiving, longtail multimedia content
3. Coldline Storage - Once every 90 days
4. Archive Storage - Once per year

Structured data includes information stored in tables. It can be split into transactional workloads and analytical workloads. Transactional workload stem from online transactional processing systems, which are used when fast data inserts and updates are required to build row-based records. This is usually to maintain a system snapchat. They require relatively standardized queries that impact only a few records.  Analytical workloads which stem from online analytical processing systems, which are used when entire datasets need to be read. They often require complex queries.
![image](https://user-images.githubusercontent.com/80007111/182018919-060c4abe-3aeb-45c6-93b8-30f6a5c16682.png)

### Big Data and ML products
Google ML and Big Data products can be divided into 4 general categories along the data to AI workflow
1. Ingestion and process - Pub/Sub, Dataflow, Dataproc & Coud Data Fusion
3. Storage - listed above
3. Analytics - BigQuery, Google data studio and looker
4. Machine Learning

     4.1. ML development platform - Vertex AI, Auto ML, Vertex AI workbench, Tensor Flow

     4.2. AI Solutions - Document AI, Contact Center AI, Retail Product Discovery, Healthcare data engine

### Reading List
* GCP product description cheat sheet - https://googlecloudcheatsheet.withgoogle.com/

### Lab 1

__Public datasets__

Go to `BigQuery > Resources > Add Data > Explore public datasets` to add publically available datasets.

You can query the datasets using SQL syntax:

```
SELECT
  name, gender,
  SUM(number) AS total
FROM
  `bigquery-public-data.usa_names.usa_1910_2013`
GROUP BY
  name, gender
ORDER BY
  total DESC
LIMIT
  10
```

Before you run the query, the query validator in the bottom right will show much data is going to be run.

__Creating your own dataset__

Resources seem to have the following structure:

`Resources > Project > Dataset > Table`

To add your own dataset:

`Resources > click Project ID > Create dataset`
(For example babynames)

Then, we can add a table to this dataset.

`Click dataset name > Create table`

* Source > upload from file
* File format > CSV
* Table name > names_2014
* Schema > name:string,gender:string,count:integer

Creating the table will run a job. The table will be created after the job completes.

Click preview to see some of the data.

Then you can query the table like before.

SQL Syntax for BigQuery: https://cloud.google.com/bigquery/docs/reference/standard-sql/

## Data Engineering for Streaming Data
### Introduction
Streaming Data vs Batch processing:

* Batch processing is when the processing and analysis happens on a set of stored data (e.g payrole/billing system which is processed on a monthly basis)
* Streaming data is a flow of data records generated by various data sources. The processing happens as the data flows througha system (e.g. fraud detection).

Modern data processing has progressed from legacy batch processing towards working with real-time data streams (e.g. you no longer have to download a movie to watch it)

### Big Data Challenges
Data engineers and scientists are facing 4 major challenges, the 4 Vs
1. Variety

* Data can come in from a variety of different sources and in various formats

2. volume

* Volume can range from gigabytes to petabytes. Pipeline code and infrastructure must scale with those changes to avoid grinding to a halt

3. Velocity

* Data often needs to be processed in near real time as soon as it reaches the system. There also needs to be a way of handling data that is late, bad data or needs to be transformed mid flight

4. Veracity

* This refers to the data quality. Because of the multitude of data dimensions resulting from teh different data types and sources, data often comes with inconsistencies and uncertainties

### Message-Oriented Architecture
Data ingestion is where large amounts of streaming data is received. This data normally comes from 1000s different events that are all happening asynchronously (e.g. IoT)

Pub/Sub is a distibuted messaging service that can recieve messages from a variety of device streams. It ensures at least one delivery of received messages to subscribing applications with no provisioning required. Pub/Sub APIs are open, the services global by default, and it offers end to end encryption.

#### Pub/Sub Artitecture
1. Upstream source data comes in from devices all over the globe
2. This data is ingested into pub sub, which is teh first point of contact within the system
3. Pub/Sub reads, stores and broadcasts to any subscribers of this data topic that new messages are available.
4. As a subscriber of Pub/Sub data flow can ingest and transform those messages in an elastic streaming pipeline and output the results into an analytics data warehouse like big query
5. Finally you can connect a data visulization tool like looker or data studio to visulize and monitor the results of a pipeline or an AI or ML tools such as Vertex AI to explore the data, to uncover business insights or help with predictions.

A central element of Pub/Sub is the topic. (topic = radio antenna)
* This is like a radio antenna, whether your radio is playing music or not the antenna itself is always there
* If music is broadcast on a frequency nobody is listening to the stream of music still exists

Similarly a publisher can send data to a topic that has no subscriber to revieve it or a subscriber can be waiting for data from a topic that isnt getting data sent to it (like listening to static)

An operational pipeline has a publisher sending data to a topic that an application is subscribed to. That means that there can be zero, one or more publishers and zero, one or more subscribers related to a topic. These are completely decoupled so they're free to break without affecting their counterparts.

Pub/Sub supports many different inputs and outputs and you can even publish a Pub/Sub event from one topic to another. 



### Designing Streaming Pipelines with Apache Beam

After messages have been captured reliably into a data warehousewe need a pipeline that can match Pub/Sub scale and elasticity to do it. This is where Dataflow comes in, Dataflow creates a pipeline to process both streaming data and batch data.
* Here process refers to the steps to extract, transform and load data (ETL)

During pipeline design phase we need to consider the following questions:

1. Will the pipeline code be compatible with both batch and streaming data or will it need to be refactored?
2. Will the pipeline code software development kit or SDK being used have all the transformations, mid-flight aggregations and windowing and be able to handle late data?
3. Are there existing templates or solutions that should be referenced?

Apache Beam is a popular open source pipeline design solution. It is a unified programming model to define and execute data processing pipelines, including ETL, batch, and stream processing
* Unified means it uses a single programming model for both batch and streaming data
* Its portable meaning it can work on multiple execution environments like Dataflow and Apache Spark amoung others.
Its extensible meaning it allows you to write and share your own connectors and transformation libraries

The Apache Beam software development kit (SDK) is a collection of software development tools in one installable package. It provides a veriety of libraries for transformations and data connectors to sources and sinks

Apache Beam creates a model representation from your code thats protable across many runners
* runners pass off your model for execution on a variety of different possible engines with Dataflow being a popular choice

### Implementing Streaming Pipelines on Cloud Dataflow
An execution engine needs to be identified to implement pipelines such as those made in Apache Beam

When Choosing an implementation method consider the following:
1. how much maintenance overhead is involved? 
2. Is the infrastructure reliable? 
3. How is the pipeline scaling handled? 
4. How can the pipeline be monitored? 
5. Is the pipeline locked into a specific service provider?

Dataflow
* Dataflow is a fully managed service for executing Apache Beam pipelines within the Google Cloud ecosystem
* Since data flow is built on Google's infrastrucure it allows for reliable auto-scaling to meet data pipeline demands
* Dataflow is serverless and NoOps (no operations)
       
       - a NoOps environment is one that doesn't require management from an operations team as maintenance, monitoring and scaling are automated
       
       - Serverless computing is a Cloud computing execution model. so the cloud platform manages infrastructure tasks on behalf of the users. e.g. resource provisioning, performance tuning and ensuring pipeline reliablity
      
When dataflow receives a job it;
1. It starts by optimizing a pipeline model's execution graph to remove any inefficiencies. 
2. It then schedules out distributed work to new workers and scales as needed. 
3. After that, it auto-heals any worker faults. 
4. From there, it automatically re-balances efforts to most efficiently use its workers. 
5. Finally, it outputs data to produce a result. (e.g. output to bigquery)

Dataflow has many templates which cover common use cases across Google Cloud products. These can be broken into 3 sets
* Streaming templates

        - for processing bulk data or batch load data. e.g. Pub/Sub to BigQuery, Pub/Sub to cloud storage, Datastream to BigQuery and Pub/Sub to mongoDB.
        
* Batch templates

        - for processing bulk data or batch load data. e.g. BigQuery to Cloud Storage, Bigtable to Cloud Storage, Cloud Storage to BigQuery and Cloud Spanner to Cloud Storage

* Utility templates

        - Utility templates address activities related to bulk compression, deletion and conversion  


### Visulisation with Looker

Looker supports BigQuery as well as more than 60 SQL database products commonly referred to as dialects. It allows developers to define a semantic modeling layer on top of databases using looker modeling language LookML.
* LookML defines logic and permissions independent from a specific database or a swqual language which frees data engineer from interacting with individual databases to focus more on business logic across an organization
* Looker is 100% web based making it easy to integrate into workflows and share
* Looker also has an API which can be used to embed looker reports in other applications

Looker dashboards can be created to provide straightfowards presentations to help you and your colleagues quickly see a high level business status. To share a looker dashboard you schedule delivery through storage services like Google Drive, slack and dropbox

### Visulisation with Data Studio

DataStudio is integrated into BigQuery making data visulisation possible with just a few clicks. Therefore leveraging data studio doesn't require support from an administrator to establish a data connection.

Three steps needed to create a Data Studio dashboard
1. Choose a template, you can start with either a pre built template or a blank report.
2. link the dashboard to a data source. This may come from big query, a local file, or a google application like google sheets
3. explore your dashboard

### Lab 2: Creating a Streaming Data Pipeline for a Real-Time Dashboard with Dataflow

__Cloud Pub/Sub topics__

Cloud Pub/Sub lets decouples senders and receivers. Senders send messages to a Cloud Pub/Sub _topic_, and receivers subscribe to this topic.

__Create a BigQuery dataset_

Messages published into Pub/Sub will be stored in BigQuery.

1. In Cloud Shell, run `bq mk taxirides` to create a dataset called `taxirides` in BigQuery
2. Now create a table inside the dataset:

```
bq mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
```
OR: navigate to BigQuery on the GCP UI > View actions > create dataset. then fill out dataset details and click create dataset. then click into the newly created dataset and click create table then fill in table details and click create table.

__Create a Cloud Storage bucket__

1. Storage > Create Bucket
2. Name must be globally unique (e.g., project id)

__Setup Cloud Dataflow Pipeline__

1. Navigation > Dataflow
2. Create job from template
3. Type > Cloud Pub/Sub topic to BigQuery template
4. Input topic > projects/pubsub-public-data/topics/taxirides-realtime
5. Output table > qwiklabs-gcp-72fdadec78efe24c:taxirides.realtime
6. Temporary location > gs://qwiklabs-gcp-72fdadec78efe24c/tmp/
7. Click run job

Cloud Dataflow will now show a visualization of the Dataflow job running.

__Analyze streaming data__

You can check the data in BigQuery:

`SELECT * FROM taxirides.realtime LIMIT 10;`

See aggregated per/minute ride data:

```sql
WITH streaming_data AS (

SELECT
  timestamp,
  TIMESTAMP_TRUNC(timestamp, HOUR, 'UTC') AS hour,
  TIMESTAMP_TRUNC(timestamp, MINUTE, 'UTC') AS minute,
  TIMESTAMP_TRUNC(timestamp, SECOND, 'UTC') AS second,
  ride_id,
  latitude, 
  longitude,
  meter_reading,
  ride_status,
  passenger_count
FROM
  taxirides.realtime
WHERE ride_status = 'dropoff'
ORDER BY timestamp DESC
LIMIT 100000

)

# calculate aggregations on stream for reporting:
SELECT 
 ROW_NUMBER() OVER() AS dashboard_sort,
 minute,
 COUNT(DISTINCT ride_id) AS total_rides,
 SUM(meter_reading) AS total_revenue,
 SUM(passenger_count) AS total_passengers
FROM streaming_data
GROUP BY minute, timestamp
```

__Explore in Data Studio__

1. Click `Explore in Data Studio`
2. Set up dimensions and metrics as desired

Once finished, stop the Cloud Dataflow pipeline job.

### Reading List
* https://cloud.google.com/pubsub/docs/
* https://cloud.google.com/dataflow/docs/
* https://developers.google.com/datastudio/
* https://connect.looker.com/ AND https://docs.looker.com/


## Big Data with BigQuery
### Introduction
Bigquery is a fully-managed data warehouse
* A data warehouse is a large store containing petabytes of data gathered from a wide range of sources
* Being fully managed means that it takes care of the underlying infrastructure

BigQuery provides two services in one. Storage and Analytics. It is a serverless solution which means you can use SQL to answer business qauestions in the front end without worrying about infrastructure int eh back end.

BigQuery has a flexable pay as you go pricing model but fixed bill options are also available

Data in BigQuery is encrypted at rest by default without any action required from a customer
* Encryption at rest means encryption is used to protect data that is stored on a disk including solid state drive or backup media

BigQuery has built in machine learning features so you can write ML models directly in BigQuery using SQL. Other professional tools such as vertex AI can also be used to train ML models. You can export data sets directly from BigQuery into vertex AI for a seamless integration across the data to AI lifecycle

What does a typical data warehouse solution architecture look like
![image](https://user-images.githubusercontent.com/80007111/182462421-3763e90c-4f27-4fe5-bf7e-c6cadf987359.png)

* Dataflow is used to process the data this step is know as ETL. 
* BigQuery sits in the middle to link data processing using dataflow and data access through analytics AI and ML tools.
* The job of the analystics engine of BigQuery at the end of the data pipeling is to ingest all the processed data after ETL, store and analyse it and possibly output it for further use such as data visulisation and machine learning.

BigQuery is like a common staging area for data analytics workloads. When your data is there business analysts, BI developers, data scientists and machine learning engineers can be granted access to your data for their own insight

### Storage and Analytics
BigQuerys fully-managed storage and fast SQL-based analytical engine services are connected by Google's high-speed internal network. This is super-fast and allows BigQuery to scale both storage and compute independently based on demand

BigQuery can ingest data sets from various sources - internal data (saved directly in BigQuery), external data, multi-cloud data and public data-sets
* This is fully managed automatically replicated, backed up and set to auto-scale

BigQuery also offers the option to query extenal data sources such as Cloud storage or other google cloud database services such as Spanner or Cloud SQL. Allowing you to bypass BigQuery managed Storage
* This means raw CSV files and Cloud storage or Google sheets can be used to write a query without being ingested by BigQuery first
* Its worth noting, inconsistency might result from saving and processing data seperately. To avoid that risk consider using dataflow to build a streaming data pipeline into BigQuery.
* BigQuery can also ingest data from multi-cloud data which is data stored in multiple cloud services such as AWS or Azure

There are three basic patterns for loading data into BigQuery
1. Batch load operation - where data is loaded into a BigQuery table in a single batch operation
2. Streaming - smaller batches of data are streamed continuously so that data is available for querying in near real-time
3. Generated data - SQL statements are used to insert rows into an existing table or to write the results of a query to a table

BigQuery is also intended for analysing data and helping to make business decisions. It is optimised for running analytical queries over large data sets. The analytics features that are available in BigQuery are;
* Ad hoc analysis using SQL
* BigQuery SQL dialect
* GeoSpatial analytics using geography data types in standard SQL geography functions
* BigQuery ML
* BigQuery BI Engine for building business intelligence dashboards

By default BigQuery runs interactive queries, which means that the queries are executed as needed. It also offers batch queries where each query is queued on your behalf and the query starts when idle resources are availabe

### BigQuery demo - San Francisco bike share

`Cloud console > Big Query > Create dataset > Create table (can upload from storage, etc.)`

When writing queries, use the following format:

`FROM [project (name? or id?)].[dataset].[table]`

E.g.:

````
SELECT COUNT(*) AS total_trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
````

(Click on this query to view the table info)

Click the down arrow > Run selected to just run the selected part of the query

__Architecture__

    ----------------------------                          ----------------------------
      BigQuery Storage Service    <-- Petabit network -->   BigQuery Query Service
    ----------------------------                          ----------------------------
     Project                                                 Run queries
       Dataset A | Dataset B                                 Connectors to other products
       Table 1   | Table 1*                                  
       Table 2   | ...
       ...       |
    ----------------------------                          ----------------------------
     * Tables stored as compressed 
       column in Colossus
     * Supports steams or data ingest


__BigQuery Tips__

* Ctrl/cmd-click on table name: view table
* In details, click on field name to insert it into the query
* Click More > Format to automatically format the query
* Explore in Data Studio > visualize data
* Save query > Save query data in project
* `CREATE OR REPLACE TABLE [dataset].[tablename] AS [SQL QUERY]` to save the data into a table, saving you having to rerun the query every time
* In the above, you could replace TABLE with VIEW, to just store the query itself. Helpful if the data is changing a lot.

### Introduction to BigQuery ML
Building an training ML models can be very time consuming the general steps are:
1. Export data from your data store into an IDE, Integrated Development Environment, such as Jupyter Notebook or Google Colab
2. Transform the data and perform your feature engineering steps before you can feed it into a training model
3. Build the model in tensorflow or similar library and train it locally on a computer or on a virtual machine
4. Improve performace by going back and forth to get more data and create new features (people normally stop this after a few iterations)

BigQuery allows for you to create and execute machine learning models on your structured data sets in BigQuery in just a few minutes using SQL queries. There are a few steps.
1. create a model with a SQL statement
2. write a SQL prediction query and invoke ml.predict
* additional steps might include activities like evaluating the model

BigQuery ML was designed to be simple, like building a model in 2 steps. That simplicity extends to defining the machine learning hyperparameters, which let you tune the model to achieve the best training results
* Hyperparameters are the settings applied to a model before the training starts, e.g. learning rate
* In BigQuery you can mannaully control these or use automatic tunning 

When using structured dataset in BigQuery ML you must choose the appropriate model type which depends on the business goal and datasets
![image](https://user-images.githubusercontent.com/80007111/182475976-31d3d5ff-530d-440c-8776-ffa13c5da01b.png)

* BigQuery supports supervised and unsupervised models
* Supervised models are task driven and identify a goal
* Unsupervised are data driven and identify a pattern

Within a supervised model, if your goal is to classify data like whether an email is spam, use logistic regression. If your goal is to predict a number like shoe sales for the next 3 months, use linear regrassion. Within an unsupervised model if your goal is to identify patterns or clusters and then determine the best way to group them. Like grouping random photos of flowers into categories you should use cluster analysis

Once you have your problem outline, its time to decide on teh best model. 
![image](https://user-images.githubusercontent.com/80007111/182476056-7f8767e8-a082-49d4-baf9-2387621733f8.png)

* categories include classification and regression models
* There are other model options to choose from along with ML Ops.
* Logistic regression is an eample of a classification model
* Linear regression is an example of a regression model

It is recommended that you start with these options then use teh results to benchmark and compare against more complex models such as DNN which may take more time and resources to train and deploy.

In Additon to providing different types of machine learning models, BigQuery supports features to deploy, monitor and manage the ML production called ML Ops
![image](https://user-images.githubusercontent.com/80007111/182476186-dd8b877c-7f29-42db-a739-54fca4658d2c.png)
Ops include;
* importing Tensorflow models for batch prediction
* exporting models from BigQuery ML for online prediction
* Hyperparameter turning using Cloud AI Vizier

### Using BigQuery ML to Predict Customer Lifetime Value
In machine learning you feed in columns of data and let the model figure out the relationship to best predict the label.

A row in the dataset is called an example an observation or an instance

A label is a correct answer which comes from historical data. This is what we use to train the model on in order to predict future data depending on what you want to predict. 
* A label can eaither be a numeric variable which requires a linear regression model 
* Or a categorical variable which requires a logistic regression model

All the other columns in a table execp for the label are call features (or potential features)
* too many features can ruin a model
* understanding the quality of the data in each column and working to get the most features or more history is often the hardest part of a ML project
* Combining or transforming feature columns is a process called feature engineering

BigQuery does much of the hard work for you
* it automaticall one-hot encodes categorical values. This is where categorical data is converted to numeric data to prepare for model training
* From there BigQuery Ml automatically splits the dataset into training and evaluation data

If we train a model on known historical data and are happy with the performace then we can use it to predict our future data sets

### BigQuery ML project phases
1. extract, transform and load data into BigQuery (you can enrich your existing data with other data sources by using SQL joins)
2. Select and preprocess features
3. create the model inside BigQuery
4. Evaluate performance of the model
5. use model to make predictions on new data

### BigQuery ML Key commands
create the model in SQL:
```sql
CREATE OR REPLACE MODEL
        `mydataset.mymodel`
OPTIONS
        ( model_type='linear_reg',
        input_label_cols='sales'
        ls_init_learn_rate=.15,
        l1_reg=1,
        max_iterations=5 ) AS
 ```
 * models have options which you can specify
 * the most important and only required is the model type

Inspect what the model has learned with the ML.WEIGHTS command and filtering on an input column
```sql
SELECT
  category,
  weight
FROM
  UNNEST((
    SELECT
      category_weights
    FROM
      ML.WEIGHTS(MODEL `bracketology.ncaa__model`)
    WHERE
      processed_input = 'seed')) # try other features like 'school_ncaa'
      ORDER BY weight DESC
```
* The output of ML.WEIGHTS is a numerical vallue, and each feature has a weight from negative 1 to 1.
* The value indicates how important the feature is for predicting the result or label
* a value close to 0 shows it is not important. a value close to -1 or 1 shows that it is an important feature

To evaluate the model's performace you can run an ML.EVALUATE command against a trained model
```sql
SELECT
  *
FROM
  ML.EVALUATE(MODEL `bracketology.ncaa_model')
```
* you get different performance metrics depending on the model type you choose

To make batch predictions, you can use the ML.PREDICT command on a trained model and pass through teh dataset you want to make the prediction on
```sql
CREATE OR REPLACE TABLE  `bracketology.predictions` AS (
SELECT * FROM ML.PREDICT(MODEL `bracketology.ncaa_model`,
#predcicting for 2018 tournament games (2017 season)
(SELECT * FROM
`data-to-insights.ncaa.2018_tournament_results`)
)
)
```

__BQML Cheatsheet__

* __Label__: Alias a column as 'label', or specify column(s) in OPTIONS using input_label_cols (reminder: labels are what is currently known in training data, but what you want to predict)
* __Feature__: Table columns used as SQL SELECT statement (`SELECT * FROM ML.FEATURE_INFO(MODEL ``mydataset.mymodel``)` to get info about that column after model is trained)
* __Model__: An object created in BigQuery that resides in BigQuqery dataset
* __Model Types__: Linear regression (predict on numeric field), logistic regression (discrete class -- high or low, spam not spam, etc.) (`CREATE OR REPLACE MODEL <dataset>.<name> OPTIONS(model_type='<type>') AS <training dataset>`)
* __Training Progress__: `SELECT * FROM ML.TRAINING_INFO(MODEL ``mydataset.mymodel```
* __Inspect Weights__: `SELECT * FROM ML.WEIGHTS(MODEL ``mydataset.mymodel``, (<query>))`
* __Evaluation__: `SELECT * FROM ML.EVALUATE(MODEL ``mydataset.mymodel``)
* __Prediction__: `SELECT * FROM ML.PREDICT(MODEL ``mydataet.mymodel``, (<query>))`

## Lab 3

Get the conversion rate:

```sql
WITH visitors AS(
SELECT
COUNT(DISTINCT fullVisitorId) as total_visitors
FROM `data-to-insights.ecommerce.web_analytics`
),

purchasers AS(
SELECT
COUNT(DISTINCT fullVisitorId) as total_purchasers
FROM `data-to-insights.ecommerce.web_analytics`
WHERE totals.transactions IS NOT NULL
)

SELECT
total_visitors,
total_purchasers,
total_purchasers/total_visitors as conversion_rate
FROM visitors, purchasers
```

Find the top 5 selling products:

```sql
SELECT
p.v2ProductName,
p.v2ProductCategory,
SUM(p.productQuantity) AS units_sold,
ROUND(SUM(p.localProductRevenue/1000000),2) AS revenue
FROM `data-to-insights.ecommerce.web_analytics`,
UNNEST(hits) AS h,
UNNEST(h.product) AS P
GROUP BY 1,2
ORDER BY revenue DESC
LIMIT 5;
```

This query:
1. Creates a table containing a row for all of hits `UNNEST(hits)`
2. Creates a table containing all of the products in hits `UNNEST(h.product)`
3. Select the various fields
4. Groups by product name and product category

The `UNNEST` keyword takes an array and returns a table with a single row for each element in the array.
`OFFSET` will help to retain the ordering of the array.

```
SELECT *
FROM UNNEST(['foo', 'bar', 'baz', 'qux', 'corge', 'garply', 'waldo', 'fred'])
  AS element
WITH OFFSET AS offset
ORDER BY offset;

+----------+--------+
| element  | offset |
+----------+--------+
| foo      | 0      |
| bar      | 1      |
| baz      | 2      |
| qux      | 3      |
| corge    | 4      |
| garply   | 5      |
| waldo    | 6      |
| fred     | 7      |
+----------+--------+

```

`GROUP BY 1,2` refers to the first and second items in the select list.

Analytics schema:

https://support.google.com/analytics/answer/3437719?hl=en

__Create the model__

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model`
OPTIONS
(
model_type='logistic_reg', # Since we want to classify as A/B
labels = ['will_buy_on_return_visit'] # Set the thing we want to predict
)
AS

#standardSQL
SELECT
  * EXCEPT(fullVisitorId) 
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)
;
```

After running, the query will create a new ML model in `project:dataset.model`

`EXCEPT` will return all of the rows in the left query not in the right query.

For example:

```sql
  WITH a AS (
SELECT * FROM UNNEST([1,2,3,4]) AS n

    ), b AS (
SELECT * FROM UNNEST([4,5,6,7]) AS n)

SELECT * FROM a

EXCEPT DISTINCT

SELECT * FROM b

-- | n |
-- | 1 |
-- | 2 |
-- | 3 |
```


__Evaluate the model__

One feature we can use to evaluate the model is the receiver operating characteristic (ROC).
Essentially this shows the quality of a binary classifier by mapping true positive rates against false positive rates.

We want to get the area under the curve as close as possible to 1.0

https://cdn.qwiklabs.com/GNW5Bw%2B8bviep9OK201QGPzaAEnKKyoIkDChUHeVdFw%3D

```sql
SELECT
  roc_auc,
  CASE
    WHEN roc_auc > .9 THEN 'good'
    WHEN roc_auc > .8 THEN 'fair'
    WHEN roc_auc > .7 THEN 'not great'
  ELSE 'poor' END AS model_quality
FROM
  ML.EVALUATE(MODEL ecommerce.classification_model,  (

SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20170501' AND '20170630') # eval on 2 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)

));
```

roc_auc is a queryable field

__Improving the model__

We can improve the model by adding more features:

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model_2`
OPTIONS
  (model_type='logistic_reg', labels = ['will_buy_on_return_visit']) AS

WITH all_visitor_stats AS (
SELECT
  fullvisitorid,
  IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid
)

# add in new features
SELECT * EXCEPT(unique_session_id) FROM (

  SELECT
      CONCAT(fullvisitorid, CAST(visitId AS STRING)) AS unique_session_id,

      # labels
      will_buy_on_return_visit,

      MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

      # behavior on the site
      IFNULL(totals.bounces, 0) AS bounces,
      IFNULL(totals.timeOnSite, 0) AS time_on_site,
      totals.pageviews,

      # where the visitor came from
      trafficSource.source,
      trafficSource.medium,
      channelGrouping,

      # mobile or desktop
      device.deviceCategory,

      # geographic
      IFNULL(geoNetwork.country, "") AS country

  FROM `data-to-insights.ecommerce.web_analytics`,
     UNNEST(hits) AS h

    JOIN all_visitor_stats USING(fullvisitorid)

  WHERE 1=1
    # only predict for new visits
    AND totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430' # train 9 months

  GROUP BY
  unique_session_id,
  will_buy_on_return_visit,
  bounces,
  time_on_site,
  totals.pageviews,
  trafficSource.source,
  trafficSource.medium,
  channelGrouping,
  device.deviceCategory,
  country
);
```

Point: Ensure you use the same training data. Otherwise differences could be due to differences in input, rather than model improvements.

Now we have a better model, we can make predictions.

__Make predictions__

```sql
SELECT
*
FROM
  ml.PREDICT(MODEL `ecommerce.classification_model_2`,
   (

WITH all_visitor_stats AS (
SELECT
  fullvisitorid,
  IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid
)

  SELECT
      CONCAT(fullvisitorid, '-',CAST(visitId AS STRING)) AS unique_session_id,

      # labels
      will_buy_on_return_visit,

      MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

      # behavior on the site
      IFNULL(totals.bounces, 0) AS bounces,
      IFNULL(totals.timeOnSite, 0) AS time_on_site,
      totals.pageviews,

      # where the visitor came from
      trafficSource.source,
      trafficSource.medium,
      channelGrouping,

      # mobile or desktop
      device.deviceCategory,

      # geographic
      IFNULL(geoNetwork.country, "") AS country

  FROM `data-to-insights.ecommerce.web_analytics`,
     UNNEST(hits) AS h

    JOIN all_visitor_stats USING(fullvisitorid)

  WHERE
    # only predict for new visits
    totals.newVisits = 1
    AND date BETWEEN '20170701' AND '20170801' # test 1 month

  GROUP BY
  unique_session_id,
  will_buy_on_return_visit,
  bounces,
  time_on_site,
  totals.pageviews,
  trafficSource.source,
  trafficSource.medium,
  channelGrouping,
  device.deviceCategory,
  country
)

)

ORDER BY
  predicted_will_buy_on_return_visit DESC;
```

### Reading List
* https://cloud.google.com/bigquery
* https://cloud.google.com/bigquery#section-5
* https://cloud.google.com/bigquery/docs/loading-data#choosing_a_data_ingestion_method
* https://cloud.google.com/blog/products/g-suite/connecting-bigquery-and-google-sheets-to-help-with-hefty-data-analysis
* https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-e2e-journey
* https://cloud.google.com/bigquery-ml/docs/tutorials

## Machine Learning Options on Google Cloud
### Options to build ML Models
Google Cloud offers four options for building machine learning models.
![image](https://user-images.githubusercontent.com/80007111/182684306-7af46b29-538b-489e-b144-afd703b2e762.png)
![image](https://user-images.githubusercontent.com/80007111/182684963-ca00dfdb-8ce0-46e5-b807-499f65745184.png)
* BigQuery ML is good for those who are familiar with SQL and have data in BigQuery
* Pre-built APIs are great is you have little ML expertise
* AutoML is used to build custom models with your own training data with minimal coding
* Custom training is used when you want full control of the ML workflow


### Pre-built APIs
ML models require lots of high-quality training data. If this is not available then pre-built APIs are a great place to start. Pre-built APIs are offered as services. The available pre-built APIs are:
* Speech-to-Text API - Converts audio to text for data processing
* Cloud Natural Language API - Recognises parts of speech called entities and sentiment
* Cloud Translation API - Converts text from one language to another
* Text-to-Speech API - Converts text into high-Quality voice audio
* Vision API - Works with and recognises content in static images
* Video Intelligence API - Recognises motion and action in video

### AUTO ML
For AutoML two technologies are vital, transfer learning and Neural Architect Search

Transfer learning

With transfer learning, you build a knowledge base in the field. Transfer learning alows people with smaller datasets or less computational power achieve state of the art results by taking advantage of pre-trained models that have been trained on similar larger datasets. Because it learns via transfer learning it doesnt have to learn from scratch.

Neural Architect Search

The goal of neural architect search is to find the optimal model for the relevant project. AutoML is powered by the latest machine learning research so although a model performs training, the AutoML platform actually trains and evaluates multiple models and compare them to each other. This neural architecture search produces and ensemble of ML models and chooses the best one.

AutoML allows a nocode solution that means it can train high quality custom machine learning models with minimal effort and requires little machine learning expertise. Therefore;
* Data scientists can focus their time on tasks like defining business problems or evaluation and imporving model results
* AutoMl can be used to quickly prototype models and explore new datasets before investing in development. This can involve identifying the best features in a dataset.

AutoML support four types of data,
* Image
* Tabular
* Text
* Video
for each data type AutoML solves different types of problems called objectives

For image data, 
* you can use a classification model to analyze image data and return a list of content categories that apply to the image.
* You can also use an object detection model to analyze your image data and return annotations that consists of a label, and bounding box location for each object found in an image.

For tabular data, 
* you can use a regression model to analyze tabular data and return a numeric value.
* You can use a classification model to analyze tabular data and return a list of categories.
* A forecasting model can use multiple rows of time dependent tabular data from the past to predict a series of numeric values in the future.

For text data,
* you can use a classification model to analyze text data and return a list of categories as applied to the texts found in the data.
* An entity extraction model can be used to inspect text data for node entities referenced in the data and label those entities in the text.
* A sentiment analysis model can be used to inspect text data and identify the prevailing emotional opinion within it, especially to determine a writer's comment as positive, negative, or neutral. 

Finally, for video data,
* You can use a classification model to analyze video data and return a list of categorized shots and segments.
* You can use an object tracking model to analyze video data and return a list of shots and segments where these objects were detected.
* An action recognition model can be used to analyze video data and return a list of categorized actions with the moments the actions happened.

In reality you may not be restricted to just one data type and one objective, but instead need to combine multiple data types in different objectives to solve a business problem. AutoMl is a powerful tool that can help across these different data types and objectives

### Custom Training
If you want to code your machine learning model, you can use this option by building a custom training solution with Vertex AI Workbench. 

 Workbench is a single development environment for the entire data science workflow from exploring, to training and then deploying a machine learning model with code.

Before any coding begins, you need to determine what environment you want, your ML training code to use
*  pre-built container
* a custom container

if your ML training needs a platform like Tensorflow, Pytorch, Scikit-learn or XGboost and Python code to work with the platform, a pre built container is probably your best solution.

A custom container alternatively is like an empty room with no cabinets, appliances or cookware. You define the exact tools, you need to complete the job.

### Vertex AI
Some traditional challenges of Machine learning are:
* Determining how to handle large quantities of data
* Determining the right machine learning model to train the data
* Harnessing the required amount of computing power

Challenges of getting ML data into produciton
* Scalability
* Monitoring
* Continuous integration and continuous delivery or deployment

Ease of use challenges
* Tools require advanced coding skills
* Take focus away from model configuration
* No unified workflow
* Difficulties finding tools

Vertex AI is googles solution to alot of these challenges. It is a unified platform that jbrings oll the components of machine learning ecosystem and  workflow together.
* In the context of Vertex AI, a unified platform means having one digital experience to create, deploy and manage models over time and at scale.

Vertex AI allows users to build machine learning models with either AutoML, a cloudless solution, or custom training, a code-based solution. custom training enables data scientist to have full control over the development environment and process. Being able to perform such a wide range of tasks in one unified platform has many benefits. This can be summarized with four S's. 
* Seamless
* Scaleable
* Sustainable
* Speedy

### AI Solutions

![image](https://user-images.githubusercontent.com/80007111/182706675-6a0d4f12-8cd4-4157-9db0-c1c6307d85eb.png)

* The bottom layer is the AI foundation and include the Google Cloud infrastructure and data. 
* The middle layer represents the AI development platform
* The top layer represents AI solutions for which there are two groups

Horezontal solutions

Horizontal solutions usually apply to any industry that would like to solve the same problem. 

Examples include
* Document AI and CCAI. Document AI uses computer vision and optical character recognition, along with natural language processing to create pre-trained models to extract information from documents. The goal is to increase the speed and accuracy of document processing to help organizations make better decisions faster while reducing costs.
* Contact Center AI or CCAI. The goal of CCAI is to improve customer service in contact centers through the use of artificial intelligence. They can help automate simple interactions, assist human agents, unlock caller insights and provide information to answer customer questions.

Industry solutions

The second group is vertical or industry solutions. This represents solutions that are relevant to specific industries. Examples include, 
* Retail Product Discovery, which gives retailers the ability to provide a Google quality search and recommendations on their own digital properties, helping to increase conversions and reduce search abandonment,
* Google Cloud Healthcare Data Engine, which generates healthcare insights and analytics with one end-to-end solution
* Lending DocAI, which aims to transform the home loan experience for borrowers and lenders by automating mortgage document processing.

### Reading List
* https://cloud.google.com/training/machinelearning-ai
* https://cloud.google.com/vertex-ai#section-1
* https://cloud.google.com/architecture/ml-on-gcp-best-practices
* https://www.fast.ai/2018/07/23/auto-ml-3/
* https://cloud.google.com/vertex-ai/docs/training/containers-overview?_ga=2.143882930.-601714452.1627921693
* https://cloud.google.com/deep-learning-containers/docs/overview#pre-installed_software


## The Machine Learning Workflow with Vertex AI

### Introduction

Machine Learning vs Traditional Programming

IN traditional programming a computer can only follow the algorithms that a human has set up. Machine learning you expect the machine to learn from the provided data and examples to solve the puzzle on its own

Key stages of machine learning process
* Data preparation
  * A model needs a large amount of data to learn from
  * This data can either be real time streaming or batch data and it can either be structured or unstructured data
* Model training
  * A model needs a huge amount of iterative training
* Model serving
  * A model needs to actually be used in order to predict results
  * This is when the model is deployed, monitored and managed

Vertex AI supports this workflow with 2 options
1. AutoML: codeless
2. Custom training: code-based

### Data Preparation

Data preparation involves uploading the data and then preparing the data through feature engineering.

Uploading data requires providing a meaningful name for the data set and selecting the data type and objective (image, tabular, text, video) Selecting the correct data type and objective you need to check data requirements and add labels to the data. A label is a training target. Labels can be added manually or can be added using googles paid label service via the Vertex console.

A feature refers to a factor that contributes to the prediction. It is an independent variable in statistics or a column in  a table. To help prepare features Vertex AI has feature store. This is a centralised repository to organise, store and serve machine learning features. It aggregates all the different features from different sources and updates them to make them available from a central repository. Engineers can then use the features available in the feature store dictionary to build a dataset. The benifits of Vertex AI feature store are:
* Features are sharable for training or serving tasks
* Features are reusable
* Features are scalable
* Features are easy to use

### Model training
This stage involves two steps:
* Model training
* Model evaluation

AI vs ML
Artificial intelligence is an umbrella term that includes anything related to computers mimicking human intelligence (e.g spell check) Machine learning is a subset of AI that mainly refers to supervised and unsupervised learning. Deep learning or DNNs are a subset of ML that adds layers in between input data and outpuyt results to make a machine larn at more depth. Supervised learning is task driven and identifies a goal whereas unsupervised learning is data driven and identifies a pattern.
![image](https://user-images.githubusercontent.com/80007111/183029654-58537676-e569-4c8b-95ad-ccf0a2a0b1d8.png)

![image](https://user-images.githubusercontent.com/80007111/183030192-03549f64-8a68-4cba-82a9-fd4ea0f4584a.png)

### Model Evaluation

Models need to be evaluated continously when being developed. Vertex AI provides extensive evaluation metrics to help determine performace. Amoung these metrics are two sets of measurements:
* The first is based on the confusion matrix
* The second is based on feature importance

Confusion matrix is a specific performance measurement for machine learning classification problems. It is a table with combinations of predicted and actual values
![image](https://user-images.githubusercontent.com/80007111/183043840-5636f3dc-07c9-4f77-a408-6354f82acbc3.png)

Recall refers to all the positive cases and looks at how many were predicted correctly. Recall = TP/(TP + FN)

Precision refers to all the cases predicted as positive and how many are actually positive Precision = TP/(TP+FP)

![image](https://user-images.githubusercontent.com/80007111/183044494-4a17d453-7ea6-40e6-861b-11ff440b33db.png)
![image](https://user-images.githubusercontent.com/80007111/183044551-97f7b22f-7113-4fe4-b967-bdbf9a1a36ba.png)

Precission and recall are often a trade off, depending on your use case. you may need to optimize for one or the other. In Vertex AI, the platform visualises the precision and the recall curve, so they can be adjusted based on the problem that needs solving

Feature Importance
In vertex AI feature importance is displayed through a bar chart to illustrate how each feature contributes to a prediction. the longer a bar the more important

Feature importance is just one example of Vertex AI's comprehensive machine learning functionality called Explainable AI. Explainable AI is a set of tools and frameworks to help understand and interpret predictions made by machine learning models.

### Model Deployment and Monitoring

 MLOps combines machine learning development with operations. And applies similar principles from DevOps to machine learning models. MLOps aims to solve production challenges related to machine learning. In this case this refers to building an integrated machine learning system and operating it in production.
 * These are considered to be some of the biggest pain points by the ML practitioners community. Because both data and code are constantly evolving A machine learning. 

Practicing MLOps means advocating for automation and monitoring at each step of the ML system construction. This means adopting a process to enable continuous integration, continuous training and continuous delivery. 

MLOps during Model Deployment
MLOps provides a set of best practices on the back end to automate this process. There are three options to deploy a machine learning model. 
1. The first is to deploy to an end point. This option is best when immediate results with low latency are needed. 
  * Such as making instant recommendations based on a user's browsing habits whenever they're online. 
  * A model must be deployed to an end point before that model can be used to serve real time predictions. 
2. The second option is to deploy using batch prediction. This option is best when no immediate response is required and accumulated data should be processed with a single request. 
  * For example sending out new ads every other week based on the user's recent purchasing behavior and what's currently popular on the market.
3. The final option is to deploy using offline prediction. This option is best when the model should be deployed in a specific environment off the cloud. 
![image](https://user-images.githubusercontent.com/80007111/183047034-d0422665-9a73-4968-83fe-ee3ae934fcb7.png)


Model Monitoring
The backbone of MLOps on vertex AI is a tool called vertex AI pipelines. It automates monitors and governs machine learning systems by orchestrating the workflow in a serve earless manner. 
* If something goes wrong, it automatically triggers warnings based on a predefined threshold. 

With vertex AI workbench, you can define your own pipeline. You can do this with pre built pipeline components. Which means that you primarily need to specify how the pipeline is put together using components as building blocks. 

### Vertex AI: Predicting Loan Risk with AutoML
#### Create a dataset
Navigation menu > Vertex AI > Datasets > create dataset
* for the data type and objective, click Tabular and then select Regression / classification then create

#### Upload data
For the data source, select **Select CSV files form Cloud Storage**
For import file path enter: spls/cbl455/loan_risk.csv
Click Continue

To see the descriptive statistics for each column of your dataset, click generate statistics then click each column name to show analytical charts

#### Train your model
Click **Train new Model**
Training Method
* For objective select Classification then click continue

Model Details
* Give the model a name and leave the target column as default then click continue

Training Options
* Specify which columns you want to include in the model,
  * Click the minus sign on the CLientID row to exclude it from training 
* Click advanced options to select different optimisation objectives e.g. log loss or AUC ROC

Compute and pricing
* For budget enter **1**
  * Training your AutoML model for 1 node hour is typically a good start for understanding whether there is a relationship between the features and label you've selected. From there, you can modify your features and train for more time to improve model performance
* Leave early stopping enabled then click start training 

#### Evaluate the model performance (demonstration only)
If you had a model trained, you could navigate to the Models tab in Vertex AI.
1. Navigate to the Models.
2. Click on the model you just trained.
3. Browse the Evaluate tab.
However in this lab, you can skip this step since you use a pre-trained model.

**The Precision / Recall curve**
![image](https://user-images.githubusercontent.com/80007111/183067181-a93c4aee-2f38-4892-adc6-318b52e02056.png)
The confidence threshold determines how a ML model counts the positive cases. A higher threshold increases the precision, but decreases recall.
* you can manually adjest the threshold to observe its impace on precision and recall and find the best tradeoff point between the two

**The Confusion matrix**
A confusion matrix tells you teh percentage of examples from each class in your test set that your model predicted correctly
![image](https://user-images.githubusercontent.com/80007111/183067594-8651749e-3d3d-449b-be6a-c296fc0bcf77.png)
Here the model is able to predict 100% of the repay examples and 87% of the default examples
* this could be improved by adding more data, engineering new features, and changing the training method

**The feature importance**
In Vertex AI, feature importance is displayed through a bar chart to illustrate how each feature contributes to a prediction.
![image](https://user-images.githubusercontent.com/80007111/183067921-458733f0-e470-428e-8f4e-3fcde312dc01.png)
From this you may decide to remove the least important features next time you train a model or to combine two of the more significant features into a feature cross to see if this improves model performance
Feature importance is an example from Vertex AIs machine learning functionality called Explainable AI. Which i sa set of tools and frameworks to help understand and interpret predicitons made by machine learning models

#### Deploy the model (demonstration only)
A model resource in Vertex can have multiple endpoints associated with it, and you can split traffic between endpoints

**Create and define an endpoint**
On the model page go to the **Deploy and test** tab and click **Deploy to endpoint**
Give the endpoint a name and click continue

**Model Setting and Monitoring**
leave the traffic splitting setting as is. As the machine type select **n1-standard-8, 8 vCPUs, 30 GiB memory.** then click continue
In Model monitoring click continue then in Model obkjectives > Training data source, select Vertex AI dataset and choose your own dataset
In Target column, enter default
Leave the remaining settings as is and click depliy

#### SML Bearer Token
To allow the pipeline to authenticate, and be authorised to call the endpoint to get the predictions, you will need to provide your Bearer Token
1. log in to https://gsp-auth-kjyo252taq-uc.a.run.app/
2. When logging in, use your student email address and password.
3. Copy the token

### Get Predictions
In this section we will work with the Shared Machine Learning servce (SML) to work with an existing trained model
![image](https://user-images.githubusercontent.com/80007111/183069670-03fb4ee3-5197-47d9-b71d-bc17480b0197.png)
To use the trained model you will need to create some environment variables
1. Open a Cloud Shell window.

2. Replace INSERT_SML_BEARER_TOKEN with the bearer token value from the previous section:
* AUTH_TOKEN="INSERT_SML_BEARER_TOKEN"

3. Download the lab assets:
* gsutil cp gs://spls/cbl455/cbl455.tar.gz .

4. Extract the lab assets:
* tar -xvf cbl455.tar.gz

5. Create an ENDPOINT environment variable:
* ENDPOINT="https://sml-api-vertex-kjyo252taq-uc.a.run.app/vertex/predict/tabular_classification"

6. Create a INPUT_DATA_FILE environment variable:
* INPUT_DATA_FILE="INPUT-JSON" 

The file INPUT-JSON is composed of the following values:

age	ClientID	income	        loan
40.77	997	        44964.01	3944.22

Test the SML Service by passing the parameters specified in the environment variables:
1. Perform a request to the SML service:
./smlproxy tabular \
  -a $AUTH_TOKEN \
  -e $ENDPOINT \
  -d $INPUT_DATA_FILE
  
![image](https://user-images.githubusercontent.com/80007111/183078758-069bb104-1815-43d9-8c63-ba9002065f7f.png)

 
This query should result in 


