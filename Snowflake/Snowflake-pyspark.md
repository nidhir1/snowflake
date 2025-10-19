# Using PySpark with Snowflake

PySpark is used with Snowflake mainly to **process, transform, and move large-scale data** between Spark environments (Databricks, EMR, on-prem Spark clusters) and Snowflake’s cloud data warehouse.  
The integration is typically done using the **Snowflake Connector for Spark**.

---

## How PySpark Fits in a Snowflake Workflow

| Step | Purpose | Example in Real Use |
|------|---------|---------------------|
| **1. Extract & Load** | Read data from multiple sources into PySpark (CSV, JSON, Kafka, databases). | Pull clickstream logs into Spark from AWS S3. |
| **2. Transform in PySpark** | Perform distributed processing, cleansing, aggregations, ML, or streaming operations. | Remove duplicates, apply NLP sentiment analysis, join with product catalog. |
| **3. Write to Snowflake** | Use Snowflake Connector for Spark to write processed DataFrames to Snowflake tables. | Store cleaned, enriched data in `analytics.sales_data`. |
| **4. Further ELT in Snowflake** | Run analytical queries, BI dashboards, or additional transformations in Snowflake SQL. | Build Tableau dashboard from Snowflake tables. |

---

## Why Use PySpark with Snowflake?

- **Complex transformations before loading** – Spark can handle computationally heavy ETL logic that might be costly or slower in Snowflake.
- **Machine Learning / AI workflows** – Spark integrates with MLlib, TensorFlow, etc., then outputs results to Snowflake for reporting.
- **Streaming ingestion** – PySpark Structured Streaming can ingest near real-time data and push to Snowflake.
- **Multi-source joins** – Spark can combine large datasets from various sources before loading into Snowflake.

---

## Integration Example: Writing PySpark DataFrame to Snowflake

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PySpark to Snowflake") \
    .getOrCreate()

sfOptions = {
  "sfURL": "myaccount.snowflakecomputing.com",
  "sfDatabase": "MYDB",
  "sfSchema": "PUBLIC",
  "sfWarehouse": "COMPUTE_WH",
  "sfRole": "SYSADMIN",
  "sfUser": "my_user",
  "sfPassword": "my_password"
}

# Sample DataFrame
df = spark.createDataFrame(
    [(1, "Alice"), (2, "Bob")],
    ["ID", "NAME"]
)

# Write to Snowflake
df.write \
    .format("net.snowflake.spark.snowflake") \
    .options(**sfOptions) \
    .option("dbtable", "MY_TABLE") \
    .mode("overwrite") \
    .save()
