# Project Overview
This project demonstrates the creation of a serverless data pipeline on AWS, which ingests weather data from an open source weather API, processes and stores the data in Amazon S3 using Kinesis Firehose, and leverages AWS Glue for ETL processes. AWS Glue is used to orchestrate workflows for creating schemas and partitions, converting data into compressed table formats (Parquet), and implementing data quality checks. The data can be queried using AWS Athena and is ultimately visualized using Grafana.

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
Open-Meteo weather API: https://open-meteo.com/en/docs


# Project Steps





