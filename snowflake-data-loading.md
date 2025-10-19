# Snowflake Data Loading Tutorial

## Table of Contents
1. [Introduction](#introduction)  
2. [Data Loading Methods](#data-loading-methods)  
    - [Loading Data via GUI](#loading-data-via-gui)  
    - [Loading Data via SQL Scripts](#loading-data-via-sql-scripts)  
3. [Using Query and History Tabs in GUI](#using-query-and-history-tabs-in-gui)  
4. [Best Practices](#best-practices)  

---

## Introduction

Loading data into Snowflake is a critical step in analytics and ETL pipelines. Snowflake supports both GUI-based and SQL-based methods for loading data from local files, cloud storage (S3, Azure, GCS), and other sources.

---

## Data Loading Methods

### Loading Data via GUI

Snowflake Web Interface provides an easy way to load data:

**Steps:**
1. Navigate to **Databases** -> Select your schema.
2. Click on **Tables** -> Select a table -> **Load Table**.
3. Choose **Upload from Local File** or **Stage** (S3, Azure, GCS).
4. Map file columns to table columns.
5. Review and click **Load**.

**Advantages:**
- Quick for small datasets and one-off uploads.
- Intuitive interface with column mapping.

**Example:**
- Upload a CSV file containing `customer_data` into the `analytics` schema table `customer_data`.

### Loading Data via SQL Scripts

For automation and large datasets, SQL scripts are preferred.

**Using Internal Stage:**
```sql
-- Create a table
CREATE TABLE customer_data (
    customer_id INT,
    name STRING,
    email STRING
);

-- Upload CSV to internal stage
PUT file:///local/path/customer_data.csv @%customer_data;

-- Load data into table
COPY INTO customer_data
FROM @%customer_data
FILE_FORMAT = (TYPE = CSV FIELD_OPTIONALLY_ENCLOSED_BY='"');
```

**Using External Stage (S3 example):**
```sql
-- Create external stage
CREATE STAGE s3_stage
URL='s3://mybucket/data/'
CREDENTIALS=(AWS_KEY_ID='...' AWS_SECRET_KEY='...');

-- Copy into table
COPY INTO customer_data
FROM @s3_stage/customer_data.csv
FILE_FORMAT = (TYPE=CSV FIELD_OPTIONALLY_ENCLOSED_BY='"');
```

**Advantages:**
- Automates ETL workflows.
- Handles large volumes efficiently.
- Supports retries, parallel loading, and error handling.

---

## Using Query and History Tabs in GUI

### Query Tab
- Execute SQL statements directly in the Snowflake Web UI.
- Useful for validating loaded data, testing queries, and development.

**Example:**
```sql
SELECT * FROM analytics.customer_data LIMIT 10;
```

### History Tab
- Provides a record of executed queries in your session and past sessions.
- Useful for auditing, debugging, and monitoring load performance.
- You can:
  - Filter queries by user, warehouse, time.
  - See query execution details and status.
  - View errors and execution time.

**Use Case:**
- Verify that `COPY INTO` command succeeded.
- Identify failed loads and retry as necessary.

---

## Best Practices

1. **Use Staging Area**: Always use internal or external stages for large datasets.
2. **Validate Data**: Check data quality post-load using `SELECT` queries.
3. **Monitor Loads**: Use History tab for auditing and troubleshooting.
4. **File Format Consistency**: Define consistent file formats for CSV, JSON, PARQUET.
5. **Automation**: Use SQL scripts or Snowpipe for recurring data loads.

---

## References

- [Snowflake Data Loading Documentation](https://docs.snowflake.com/en/user-guide/data-load-overview.html)  
- [COPY INTO Reference](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html)

