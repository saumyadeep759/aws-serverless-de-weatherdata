import sys
import boto3

client = boto3.client('athena')

SOURCE_TABLE_NAME = 'crawledweather_data_bucket_batch_sam'
NEW_TABLE_NAME = 'weather_data_batch_parquet_tbl_sam'
NEW_TABLE_S3_BUCKET = 's3://open-meteo-weather-data-parquet-bucket-sam/'
MY_DATABASE = 'de_project_database_sam'
QUERY_RESULTS_S3_BUCKET = 's3://sam-athena-query-results-bucket-may-2024'

# Refresh the table
queryStart = client.start_query_execution(
    QueryString = f"""
    CREATE TABLE {NEW_TABLE_NAME} WITH
    (external_location='{NEW_TABLE_S3_BUCKET}',
    format='PARQUET',
    write_compression='SNAPPY',
    partitioned_by = ARRAY['time'])
    AS

    SELECT
        latitude
        ,longitude
        ,temp_c
        ,daylight_s
        ,precipitation_mm
        ,windspeed_kmh
        ,row_ts
        ,time
    FROM "{MY_DATABASE}"."{SOURCE_TABLE_NAME}"

    ;
    """,
    QueryExecutionContext = {
        'Database': f'{MY_DATABASE}'
    }, 
    ResultConfiguration = { 'OutputLocation': f'{QUERY_RESULTS_S3_BUCKET}'}
)

# list of responses
resp = ["FAILED", "SUCCEEDED", "CANCELLED"]

# get the response
response = client.get_query_execution(QueryExecutionId=queryStart["QueryExecutionId"])

# wait until query finishes
while response["QueryExecution"]["Status"]["State"] not in resp:
    response = client.get_query_execution(QueryExecutionId=queryStart["QueryExecutionId"])
    
# if it fails, exit and give the Athena error message in the logs
if response["QueryExecution"]["Status"]["State"] == 'FAILED':
    sys.exit(response["QueryExecution"]["Status"]["StateChangeReason"])
