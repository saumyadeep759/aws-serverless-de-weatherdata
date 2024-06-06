# Project Overview
This project showcases the development of a serverless data pipeline on AWS. It collects weather data from an open-source weather API, stores it in Amazon S3 using Kinesis Firehose, and performs data processing using AWS Glue. AWS Glue handles tasks like organizing data schemas and partitions, converting data into efficient formats (Parquet), and ensuring data quality. Users can query the data with AWS Athena and visualize it using Grafana.

# Architecture
The architecture of this project consists of the following key components:
1. AWS Lambda: Fetches weather data from the Meteo API.
2. Amazon Kinesis Firehose: Delivers streaming data to Amazon S3.
3. Amazon S3: Stores raw and processed data.
4. AWS Glue: Used for creating crawlers, ETL jobs, and workflows, including schema creation, partitioning, data compression, and quality checks.
5. Amazon Athena: Queries the data stored in S3.
6. Grafana: Visualizes the queried data.     

![AWS-Serverless-Data-Engineering-Pipeline](https://github.com/saumyadeep759/aws-serverless-de-weatherdata/assets/26822492/e1377fa8-35d8-4c63-ad3a-bb9862a62b6f)


# Features
1. Serverless Architecture: Fully managed components with no server maintenance.
2. Scalable Data Ingestion: Utilizes Kinesis Firehose for real-time data streaming.
3. Automated ETL: AWS Glue automates the extraction, transformation, and loading of data, with integrated data quality checks.
4. Efficient Storage: Data is stored in Parquet format for efficient querying.
5. Data Visualization: Integrates with Grafana for real-time data visualization.
6. Query Capability: Data can be queried using Amazon Athena for further analysis      

# Prerequisites
1. AWS Account: The steps to set up the same can be found [here](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html)
2. IAM roles with necessary permissions
3. Grafana setup and configured to read from Athena

# Data Source

### Overview
The Open-Meteo weather API has been used which is a free, open-source weather API that provides real-time and forecast weather data for any location worldwide. It offers access to various meteorological data, including temperature, precipitation, and wind, without requiring authentication.
Open-Meteo weather API: https://open-meteo.com/en/docs

### Fields used
The API has been used to retrieve historical weather data for Berlin (Germany) for the last 3 months i.e. from 01-Mar-2024 to 31-May-2024.

The following fields have been retrieved:  
-`latitude`: latitude of the place  
-`latitude`: longitude of the place  
-`time`: date time  
-`temperature_2m_max`:  maximum air temperature at 2 meters above the ground  
-`daylight_duration`: total amount of time (in seconds) in a day during which natural sunlight was present, from sunrise to sunset  
-`precipitation_sum`: total amount of precipitation (including rain, snow, sleet, etc.) accumulated in millimeters  
-`wind_speed_10m_max`: maximum wind speed(in km per hour) measured at 10 meters above the ground    



# Project Steps

## Part 1: Data Ingestion
Goal: The Goal is to read data from the weather API using a Lambda function and then dumping the data into an AWS s3 bucket in an organised fashion using Kinesis Firehose.

### 1.1 Create s3 buckets to store data and query results
Initially 2 s3 buckets are created in AWS s3- one for storing data (ingested from the API) and another for storing the Athena query results.

### 1.2 Configure Kinesis Firehose to stream data from Lambda to s3
Navigate to Kinesis in AWS and create and configure a Firehose stream with a buffer size and interval of 5mb and 60s respectively.


### 1.3 Create Lambda function to ingest data from the external API
A Lambda function (in Python) is created to ingest the historical data from the weather API and then a python dictionary is used in the code to store the entire batch of data in it. The entire dictionary data is then connected to the AWS Kinesis firehose which dumps it to the s3 bucket (intended for storing data).
Once the function is deployed, tested and invoked, the data appears in the s3 bucket.  


## Part 2: Data Transformation
Goal: The goal is to process and manipulate data to make it suitable for various purposes such as analytics, reporting, and machine learning

### 2.1 Use AWS Glue crawler to crawl data and create table schema and partitions
The 1st step is to navigate to AWS Glue and configure a Crawler to read data from the s3 bucket and create a partioned table and schema. Once the crawler is configured, the newly created table can be accessed in Athena Query editor.

### 2.2 Set up ETL job to create parquet table
Initially, a new S3 bucket to store the Parquet output files from the Glue jobs needs to be created.
Under AWS Glue, navigate to ETL jobs and create a job in python to create a parquet version of the table created in step 2.1. 

### 2.3 Configure other ETL jobs and orchestrate a workflow
Create other ETL jobs to delete table , incorporate data quality checks and eventually publish a prod version of the table to query in Athena. All these jobs need to be configured and arranged in a AWS Glue workflow as shown below:
<img width="798" alt="image" src="https://github.com/saumyadeep759/aws-serverless-de-weatherdata/assets/26822492/e5d82c9a-297a-446b-a962-fac9ec8bb4f7">

<img width="553" alt="image" src="https://github.com/saumyadeep759/aws-serverless-de-weatherdata/assets/26822492/224e0526-30c1-4f1f-b28a-c16d38939f39">

Once the workflow is ready, the same can be executed .

### 2.4 Accessing logs in CloudWatch

Logs are especially useful for debugging jobs that have failed, as the error messages indicating the reasons the job failed will be inside the logs. The logs can be accessed in 'Job run monitoring' section.  



## Part 3: Data Visualization

### 3.1 Configure Grafana platform
In AWS IAM, create a new Grafana user and then navigate to Grafana website to login using the creds  and then create a data connection.    

### 3.2 Create Visualizations
The final step is to create visualizaions in Grafana to highlight KPIs as shown below
URL: https://saumyadeep759.grafana.net/d/ednwvlzx0vkzkd/precipitation?orgId=1&from=1709870404822&to=1717642804822

<img width="1292" alt="image" src="https://github.com/saumyadeep759/aws-serverless-de-weatherdata/assets/26822492/c868107b-e79c-4fc2-91f9-54f4dd948d3e">



## Acknowledgements
- [Build Your First Serverless Data Engineering Project](https://maven.com/david-freitag/first-serverless-de-project)
- [Open Meteo weather API](https://api.open-meteo.com//)
- [Grafana Labs](https://grafana.com/)







