# Advanced Snowflake Features

## 1. Snowflake on Azure & External Storage

### Working with Azure Storage
Snowflake seamlessly integrates with **Azure Blob Storage** for loading and unloading data.  
To access Azure Blob containers, you can create **external stages** using **SAS tokens** or **Azure AD credentials**.

**Example:**
```sql
CREATE STAGE azure_stage
URL = 'azure://myaccount.blob.core.windows.net/mycontainer'
CREDENTIALS = (AZURE_SAS_TOKEN='?sv=2024-01-01&ss=b&srt=sco&sp=rl')
FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY='"');
```

- Supports all major Azure regions and encryption modes.  
- Useful for hybrid data architectures where data is shared across cloud environments.

### Creating and Using External Stages
An **external stage** is a named Snowflake object that points to cloud storage locations such as Azure Blob, AWS S3, or Google Cloud Storage.

**Example Usage:**
```sql
COPY INTO my_table
FROM @azure_stage/path/to/files/
FILE_FORMAT = (TYPE = 'PARQUET')
ON_ERROR = 'CONTINUE';
```

External stages simplify secure, scalable access to large datasets stored outside Snowflake.

---

## 2. SnowPipes & Incremental Loads

### Snowflake SnowPipes: Incremental Loads
**Snowpipe** provides **continuous data ingestion** from cloud storage into Snowflake tables.  
It automatically loads data as soon as new files appear in the specified external stage.

**Example:**
```sql
CREATE PIPE my_snowpipe
AUTO_INGEST = TRUE
AS
COPY INTO raw_table
FROM @azure_stage
FILE_FORMAT = (TYPE = 'CSV')
ON_ERROR = 'SKIP_FILE';
```

- Snowpipes integrate with **event notifications** (e.g., Azure Event Grid or AWS S3 Events).  
- Enables **near real-time CDC pipelines** without manual intervention.

### Data Unloading Concepts
You can export (unload) Snowflake data to external cloud storage for archiving or sharing:

```sql
COPY INTO @azure_stage/export_data/
FROM analytics_table
FILE_FORMAT = (TYPE = 'PARQUET');
```

Snowflake automatically handles compression, parallelization, and file partitioning.

### External Data Stages
External stages are critical for both **data ingestion (COPY INTO)** and **data unloading (COPY INTO @stage)** processes.  
They act as secure connectors between Snowflake and cloud storage.

---

## 3. Snowflake Integration

### Snowflake with Azure Data Factory
**Azure Data Factory (ADF)** can orchestrate Snowflake pipelines by using:  
- **Linked Services** for Snowflake connections  
- **Copy Activities** for data movement between Azure Blob/SQL and Snowflake  
- **Data Flows** for transformation logic

ADF triggers can also invoke **Snowflake Tasks or Stored Procedures** via JDBC/ODBC.

**Example:** Use ADF to copy data from Blob Storage → Snowflake Stage → Target Table.

### Managing Governance in Snowflake
Governance and security are managed via:  
- **Role-based Access Control (RBAC)**  
- **Tag-based access policies**  
- **Masking policies** and **row access policies** for data privacy  
- Integration with Azure Active Directory for **federated authentication**

**Example:**
```sql
CREATE MASKING POLICY ssn_mask AS (val STRING) RETURNS STRING ->
  CASE WHEN CURRENT_ROLE() IN ('DATA_ADMIN') THEN val ELSE 'XXX-XX-XXXX' END;
```

---

## 4. Snowflake Data Sharing

### Secure Data Sharing between Snowflake Accounts
**Secure Data Sharing** allows instant, live data sharing between Snowflake accounts without copying or moving data.

**Example:**
```sql
CREATE SHARE trade_share;
GRANT USAGE ON DATABASE trade_db TO SHARE trade_share;
ADD ACCOUNT = 'ORG123.ACC456' TO SHARE trade_share;
```

The consumer can access the shared data instantly using a **reader account** or a **Snowflake account in another region**.

### Managing Shared Data and Access
- Data remains **in place**, ensuring security and zero duplication.  
- Sharing can be **revoked or restricted** anytime.  
- Supports both **direct sharing** and **data exchange via Marketplace**.

---

## 5. Snowflake Data Marketplace

### Exploring and Using Data from the Marketplace
The **Snowflake Data Marketplace** provides third-party datasets such as financial, demographic, or weather data directly within Snowflake.  
You can explore datasets via **Snowsight** or **SQL commands**.

**Example:**
```sql
SHOW DATA SHARES;
DESC SHARE "WEATHER_ANALYTICS"."DAILY_FORECAST";
```

### Publishing Data to the Marketplace
Data providers can **publish datasets** securely for others to access via sharing or subscription models.

Steps to publish:
1. Register your organization as a **data provider**.  
2. Create a **data share** with relevant objects.  
3. Submit it for listing on the marketplace.  

This allows external consumers to access your data **without data movement**.

---

## Summary

| Feature | Description |
|----------|--------------|
| **Azure Integration** | Direct access to Azure Blob using stages |
| **SnowPipes** | Continuous auto-ingest data pipelines |
| **ADF Integration** | Orchestrate and schedule Snowflake ETL workflows |
| **Governance** | RBAC, masking, and policy-driven controls |
| **Data Sharing** | Secure sharing between accounts or regions |
| **Marketplace** | Access or publish third-party datasets |

---

**References:**
- [Snowflake External Stages](https://docs.snowflake.com/en/user-guide/data-load-azure-create-stage)
- [Snowpipe Documentation](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro)
- [Azure Data Factory Integration](https://learn.microsoft.com/en-us/azure/data-factory/connector-snowflake)
- [Snowflake Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro)
- [Snowflake Marketplace](https://docs.snowflake.com/en/user-guide/marketplace-overview)
