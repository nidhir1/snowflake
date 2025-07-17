# **Snowpipe: Streaming Data Ingestion in Snowflake**  

## **What is Snowpipe?**  
Snowpipe is **Snowflake's continuous data ingestion service**, enabling **real-time or near-real-time loading** of data into Snowflake from cloud storage (AWS S3, Azure Blob, Google Cloud Storage). It is a **serverless, automated, and scalable** solution that loads data as soon as it arrives, eliminating the need for batch processing.  

## **How Snowpipe Works**  
1. **Data lands in Cloud Storage** (e.g., S3, Azure Blob).  
2. **Snowpipe detects new files** using event-based notifications (AWS SNS/SQS, Azure Event Grid, GCS Pub/Sub) or manual polling.  
3. **Data is loaded into Snowflake tables** using an **auto-ingestion process** based on a defined `COPY INTO` command.  
4. **Metadata is maintained** to prevent duplicate processing.  
5. **Files are processed in micro-batches**, ensuring lower latency than traditional batch processing.  

## **Use Cases for Snowpipe**  
### **1. Real-Time or Near-Real-Time Data Ingestion**  
- Streaming logs, sensor data, or application events can be loaded continuously into Snowflake.  
- Example: **IoT device telemetry** processing for real-time analytics.  

### **2. ETL/ELT Pipelines for Cloud Data Warehousing**  
- Snowpipe enables **incremental data ingestion**, reducing the need for large batch loads.  
- Example: **Financial transactions processing**, where updates occur frequently.  

### **3. Loading JSON, Avro, Parquet, or CSV Data**  
- Supports **semi-structured** and structured formats directly from storage.  
- Example: **Web event tracking** (clickstream data) stored in JSON format.  

### **4. Optimized Data Lake Integration**  
- Works seamlessly with **cloud storage-based data lakes**.  
- Example: **Data ingestion from third-party SaaS platforms** (e.g., CRM, ERP).  

---

## **Building ETL/ELT Pipelines with Snowpipe**  
### **ETL (Extract, Transform, Load) with Snowpipe**  
1. **Extract:** Data is generated in source systems and pushed to cloud storage.  
2. **Transform:** Data transformation is applied **before loading** (e.g., preprocessed Parquet files).  
3. **Load:** Snowpipe ingests transformed data into Snowflake.  

### **ELT (Extract, Load, Transform) with Snowpipe**  
1. **Extract:** Data is generated and pushed to cloud storage in raw format.  
2. **Load:** Snowpipe **automatically ingests raw data** into Snowflake.  
3. **Transform:** Transformations (e.g., cleansing, deduplication, aggregations) are done **inside Snowflake** using SQL or Snowpark.  

**💡 ELT is preferred with Snowflake** since it leverages Snowflake’s **scalability and performance** for transformations.  

---

## **Pros and Cons of Snowpipe**  

| Feature      | Pros 🚀 | Cons ⚠️ |
|-------------|--------|--------|
| **Performance** | Near-real-time ingestion | Slight latency compared to direct streaming (Kafka, Kinesis) |
| **Cost Efficiency** | Pay-per-use model (billed per micro-batch) | Costs may add up with frequent small loads |
| **Scalability** | Auto-scales with workloads | No manual tuning of ingestion speed |
| **Ease of Use** | Simple `COPY INTO` SQL-based setup | Initial setup (event notifications) requires cloud IAM permissions |
| **Data Formats** | Supports structured & semi-structured data | No direct support for unstructured data (e.g., images) |
| **Error Handling** | Automatic retry & metadata tracking | Debugging errors can be tricky |

---

## **Final Thoughts**  
- **Snowpipe is ideal for near-real-time ingestion** where batch processing is too slow.  
- **Best suited for ELT workflows**, allowing Snowflake to handle transformations efficiently.  
- **Integrates well with cloud storage and event-based architectures** for continuous data loading.  
- **Cost-effective** if optimized properly (e.g., batching small files instead of frequent tiny loads).  


## Here’s a Snowpipe implementation example for loading data from S3 into Snowflake, covering:
### Full Load – Load a large table into a staging table.
### Incremental (Delta) Load – Continuously load new or changed data into the staging table.

### Step 1: Create a Stage in Snowflake
* Before using Snowpipe, define an external stage that points to your Amazon S3 bucket.
```sql
-- Create an external stage pointing to S3
CREATE OR REPLACE STAGE my_s3_stage
URL = 's3://my-bucket/data/'
STORAGE_INTEGRATION = my_s3_integration
FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY='"' SKIP_HEADER=1);
This stage allows Snowflake to access the S3 bucket securely.
```
#### Step 2: Full Load - Load a Large Table into a Staging Table
* For a full load, manually load all data from S3 into the staging table using COPY INTO.
```sql
-- Create a staging table
CREATE OR REPLACE TABLE stage_table (
    id INT,
    name STRING,
    value FLOAT,
    updated_at TIMESTAMP
);

-- Load full data from S3 (one-time full load)
COPY INTO stage_table
FROM @my_s3_stage
FILE_FORMAT = (TYPE = 'CSV');
-- This command loads all files from S3 into the stage_table. After this, new data will be loaded incrementally.
```
#### Step 3: Snowpipe for Incremental (Delta) Loads
* Create Snowpipe to Continuously Load Deltas
* Instead of running COPY INTO manually, we create a Snowpipe to automatically load new files as they arrive.
```sql
CREATE OR REPLACE PIPE my_snowpipe
AUTO_INGEST = TRUE
AS 
COPY INTO stage_table
FROM @my_s3_stage
FILE_FORMAT = (TYPE = 'CSV');
```
* Set Up S3 Event Notifications, For real-time ingestion, configure S3 event notifications to trigger Snowpipe whenever a new file arrives.
* This can be done using AWS SNS/SQS. Snowpipe will then automatically load new data without manual intervention.
#### Step 4: Merge Delta Data into Target Table
* Once the new data lands in the staging table, you can merge it into the final table.
```sql
-- Create the target table
CREATE OR REPLACE TABLE final_table (
    id INT PRIMARY KEY,
    name STRING,
    value FLOAT,
    updated_at TIMESTAMP
);

-- Merge incremental changes from the staging table
MERGE INTO final_table AS target
USING stage_table AS source
ON target.id = source.id
WHEN MATCHED AND source.updated_at > target.updated_at 
    THEN UPDATE SET target.name = source.name, target.value = source.value, target.updated_at = source.updated_at
WHEN NOT MATCHED 
    THEN INSERT (id, name, value, updated_at) VALUES (source.id, source.name, source.value, source.updated_at);
```
#### Final Workflow:
* Initial Full Load → Load historical data using COPY INTO.
* Ongoing Delta Load → Snowpipe automatically loads new data as it arrives in S3.
* Data Merge → Use MERGE INTO to update or insert new records into the final table.
* Key Benefits of Using Snowpipe for Delta Loads:
* Near real-time ingestion – No need for manual batch jobs.
* Cost-efficient – Pay only for the compute used in micro-batches.
* Seamless S3 integration – Automatically detects and loads new files.
* Scalable – Handles high-volume data ingestion without performance issues.

## Streams
* Steps to Schedule Snowpipe & Merge with Snowflake Tasks
* Create a Stream on the staging table to track new records.
* Create a Snowflake Task to merge delta records into the final table.
* Schedule the Task to run at regular intervals (e.g., every 5 minutes).
### Step 1: Create a Stream on the Staging Table
* A stream tracks INSERT, UPDATE, DELETE changes in the staging table.
```sql
CREATE OR REPLACE STREAM stage_table_stream
ON TABLE stage_table;
```
* This will keep track of all newly ingested files using Snowpipe.
### Step 2: Create a Snowflake Task to Merge Delta Data
* A Snowflake Task runs on a schedule to process new data.
```sql
CREATE OR REPLACE TASK merge_stage_to_final
WAREHOUSE = my_warehouse
SCHEDULE = '5 MINUTE'  -- Runs every 5 minutes
WHEN SYSTEM$STREAM_HAS_DATA('stage_table_stream')  -- Runs only if new data exists
AS
MERGE INTO final_table AS target
USING stage_table_stream AS source
ON target.id = source.id
WHEN MATCHED AND source.updated_at > target.updated_at 
    THEN UPDATE SET target.name = source.name, target.value = source.value, target.updated_at = source.updated_at
WHEN NOT MATCHED 
    THEN INSERT (id, name, value, updated_at) VALUES (source.id, source.name, source.value, source.updated_at);
```
### Step 3: Enable the Task
* After creating the task, enable it:
```sql
ALTER TASK merge_stage_to_final RESUME;
```
* Snowpipe automatically loads new files into stage_table.
* The stream (stage_table_stream) tracks new records.
* The task (merge_stage_to_final) runs every 5 minutes, only if new data exists.
* The MERGE statement updates or inserts records into final_table.

### Alternative Scheduling Options
* Run every X minutes (SCHEDULE = '10 MINUTE')
* Run every hour (SCHEDULE = 'USING CRON 0 * * * * UTC')
* Event-driven Execution (WHEN SYSTEM$STREAM_HAS_DATA(...))