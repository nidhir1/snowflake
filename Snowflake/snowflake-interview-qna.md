# Snowflake
> Venkata Bhattaram / TINITIATE

* REF
* https://www.sqlschool.com/Snowflake-Online-Training.html
## Snowflake Architecture, Basics
* Architecture
  * Database Storage
  * Query Processing
  * Cloud Services
* Warehouse, Role, Database, Schema and Table
  * Virtual Warehouse | Virtual data warehouse
    * They are compute clusters that power the modern data warehouse, 
    acting as an on-demand resource. It is is an independent compute resource that 
    can be leveraged at any time for SQL execution.
  * Role
    * An entity to which privileges can be granted. Roles are in turn assigned to users.
      Note that roles can also be assigned to other roles, creating a role hierarchy.

## Pandas, boto3
## Partitioning
## Snowpark
* Snowpark is a set of libraries and runtimes that lets you securely deploy and 
  process non-SQL code – like Python, Java, and Scala, directly within Snowflake.
* Process data in your preferred language with familiar languages like Python, Java, 
  or Scala for data manipulation, transformations, and analysis.
* **Pushdown processing**: Snowpark pushes down computations to the Snowflake engine,
  This means non-SQL code at the data, eliminating the need for data movement for 
  performance boosting.
* **Build diverse applications**: Develop data pipelines, machine learning models, 
  web applications, and more – all within the Snowflake platform.
* **Seamless integration**: Snowpark seamlessly integrates with existing Snowflake 
  features like user-defined functions (UDFs) and stored procedures.

## Replication
## Stage
* Stage is a virtual location or container where external data files are stored before being loaded into Snowflake tables.
* Stages can be located in cloud storage services like Amazon S3, Azure Blob Storage, Google Cloud Storage, or on an on-premises file system.
* Stages support different file formats, such as CSV, JSON, Parquet, Avro
  Snowflake can automatically recognize and load data from files in these formats.
* **Loading Data** into Snowflake tables from stages using the COPY INTO command.
* Stages act as an intermediate data storage locations. Data from here is ingested into Snowflake tables.
* **Unloading Data** from Snowflake tables to a stage using the COPY INTO command with a stage as the target.
```sql
-- Create an external stage pointing to an S3 bucket
CREATE OR REPLACE STAGE my_s3_stage
  URL = 's3://your-s3-bucket/'
  CREDENTIALS = (AWS_KEY_ID = 'your-access-key-id' AWS_SECRET_KEY = 'your-secret-key')
  FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY = '"');
```

## SnowPipe

## Time Travel
In Snowflake, Time Travel is a built-in feature that is available for all tables by default.

* Retention Period:
* The Time Travel feature has a retention period, which is the duration for which historical versions of your data are retained.
* By default, Snowflake retains historical data for 1 day. You can configure the retention period when creating a table or changing the retention period for an existing table.
```sql
-- Set the retention period to 7 days for a table
CREATE TABLE my_table (col1 INT, col2 STRING) 
HISTORY_TABLE = (CHANGE_RETENTION_TIME_IN_DAYS => 7);

-- Set the retention period to 30 days for a table
CREATE TABLE my_table (col1 INT, col2 STRING) 
WITH DATA_RETENTION_TIME_IN_DAYS = 30
```

* System Versioned Table:
* To take advantage of Time Travel, you need to create a table with system versioning enabled. This means the table will retain historical versions of rows.
```sql
-- Create a table with system versioning
CREATE TABLE my_table (col1 INT, col2 STRING) 
SYSTEM_VERSIONING = TRUE;
```

* Querying Historical Data:
* Once a table is set up with system versioning, you can query historical data using the Time Travel feature in your SQL queries.
```sql
-- Query data as of a specific timestamp
SELECT * FROM my_table AS OF SYSTEM TIME '2023-01-01 00:00:00';
```
```sql
-- Query data before a specific timestamp
SELECT * FROM my_table BEFORE SYSTEM TIME '2023-01-01 00:00:00';
```
* Managing Retention Policy:
* You can also alter the retention policy for an existing table if you need to change the retention period.
```sql
-- Change the retention period for an existing table
ALTER TABLE my_table SET CHANGE_RETENTION_TIME_IN_DAYS = 14;
```
* Retrive dropped table, works for a max of 90 days and default 1 day.
```sql
UNDROP TABLE my_table;
```

* This will return details about the most recent query that included the specified table name in its text, including its query ID
```sql
SELECT *
FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
WHERE QUERY_TEXT LIKE '%<table_name>%'
ORDER BY QUERY_START_TIME DESC
LIMIT 1;
```
* session history, For the most recent query: Use `SELECT $SQLID;` or `SELECT LAST_QUERY_ID();`
For specific queries within the current session: 
```sql
SELECT LAST_QUERY_ID(n);
-- where n is the query position (1 for first, 2 for second, etc.).
```

## Snowflake TSQL
### Python UDF (user-defined functions)
* Create Python UDT
```python
def snore(n):   # return a series of n snores
    result = []
    for a in range(n):
        result.append("Zzz")
    return result
```
*  SnowSQL (CLI client) and use the PUT command to copy the file from the local file system to the default user stage, named @~. (The PUT command cannot be executed through the Snowflake GUI.)
```bash 
put
file:///Users/Me/sleepy.py
@~/
auto_compress = false
overwrite = true
;
```
* Import WHL files with put
```bash
snowsql -q "PUT file://C:\hydr8-0.0.1-py3-none-any.whl @MYDEMODB.MYSCHEMA.OTHER_PYTHON_PACKAGES AUTO_COMPRESS=FALSE OVERWRITE=TRUE"
```


## TSQL
### Triggers
### procedures
### Functions
### Mat Views
### Built-In Functions
### Built-In procs

## SNOWFLAKE FEATURES
## Indexes
## Performance Tuning
```sql
EXPLAIN USING TABULAR 
SELECT Z1.ID, Z2.ID 
FROM   Z1, Z2
WHERE  Z2.ID = Z1.ID;
```
```sql
EXPLAIN USING TEXT
SELECT Z1.ID, Z2.ID 
FROM   Z1, Z2
WHERE  Z2.ID = Z1.ID;
```

## Clustering and Partitioning
* Clusters and partitions are two different ways to organize data in Snowflake. Clusters organize data within micro-partitions, while partitions divide a table into smaller units.
* Micro-partitions provide the foundational data organization mechanism, ensuring efficient storage and parallel processing.
* Data clustering adds an extra layer of optimization by physically ordering data based on your chosen columns, further enhancing query performance for specific access patterns.
* Time Travel is a form of clustering.

### Hash Partitioning:
* Hash partitioning involves distributing rows across partitions based on a hash function applied to one or more columns.
* This method ensures an even distribution of data among the partitions.
* Hash partitioning is suitable for scenarios where you want to distribute the data evenly across partitions, and you may not have a clear range of values for partitioning.
```sql
CREATE TABLE my_table (
  col1 INT,
  col2 STRING
)
```

### Range Partitioning
* Range partitioning involves dividing the data into partitions based on a specified range of values for a chosen column.
* Each partition represents a specific range of values.
* Range partitioning is useful when there are clear ranges for the values in a column, and you want to optimize queries that involve filtering based on those ranges.
```sql
CREATE TABLE my_table (
  col1 INT,
  col2 STRING
)
PARTITION BY RANGE(col1) (
  PARTITION p1 VALUES LESS THAN (100),
  PARTITION p2 VALUES LESS THAN (200),
  PARTITION p3 VALUES LESS THAN (300)
);
```

### Time Travel and Clustering:
* It's important to note that Snowflake's Time Travel feature, which allows accessing historical versions of data, is compatible with partitioning.
* Additionally, Snowflake supports automatic clustering, which organizes data within partitions to improve query performance.
```sql
-- Enable automatic clustering on a table
ALTER TABLE my_table CLUSTER BY (col1);
```
* This command organizes the data within partitions based on the specified clustering key (col1 in this case), making it more efficient for certain types of queries.
* Partitioning in Snowflake can significantly enhance query performance, reduce storage costs, and improve overall data management. * The choice between hash and range partitioning depends on the nature of the data and the types of queries you expect to run.
* It's often a good practice to analyze query patterns and workload characteristics before deciding on the appropriate partitioning strategy.


## Information Schema



"""
Strong in python including libraries for pandas, boto3, snowpark- is must
Experience in Snow SQL, Snow Pipes, Streams, Views, Procedures, performance tuning, CDC and Dynamic Tables.
Exposure to Time travel, data movement between Snowflake to other RDBMS and vice versa
Experienced in SQL performance tuning and troubleshooting
Should have knowledge of Real time or Near real time setups using Kafka with Snowpipe
Knowledge on CI/CD using Jenkins/Bamboo
Experience in Agile/Scrum based project executions
Good to have python experience with hands-on.
Very good SQL expertise in Joins, Analytical functions.
Good to have Cloud knowledge on AWS services like S3, Lambda, Athena or Blob


##############
Ch 1: Introduction to Cloud & Snowflake
Database Introduction & Database Types
OLTP, OLAP and Warehouse Databases
Need for Datawarehouses; Advantages
Popular Database Technologies
Popular Datawarehouse Technologies
Need for Cloud & Remote DWH Store
Cloud Implementation Types & Usage
IaaS: Infrastructure As A Service
PaaS: Platform As A Service
SaaS: Software As A Service
Snowflake Cloud: Introduction
Snowflake: Popular SaaS Platform
Cloud Datawarehouse Concepts
Advantages, Dependencies in Snowflake
#############
Ch 2: Snowflake Account & Editions
Creating Snowflake Account (Cloud)
Snowflake Trail Account: Limitations, Uses
Snowflake Account Components
Cloud Platform: Azure, AWS, Google Cloud
Regions and Availability with Snowflake
Snowflake Editions, Credits; UI Usage
Default Accounts; Web UI & Snow Sight
Snowflake Storage: Ondemand, Capacity
Snowflake Editions and Comparisons
Standard Edition: Features, Advantages
Enterprise Edition: Features, Advantages
Business Critical Edition: Features, Usage
Virtual Private Edition (VPS) & Pricing
Snowflake Pricing: Ondemand Vs Capacity
########
Ch 3: Architecture, Warehouse (DWH)
Snowflake Architecture & Compute
Shared Disk, Shared Nothing Architecture
Cluster Nodes and Snowflake Clusters
CPU & Memory Resources in Clusters
Disk Storage and Network Communication
Storage Layer and Cloud Service Layer
Database Query Layer & Data Cycle
Snowflake Datawarehouse Architecture
MPP: Massively Parallel Processing
Compute and Storage Components
Column Store and Virtual Warehouse
Datawarehouse Creation in SnowSight
Classic UI with Snowflake; Navigations
Data Load and Billing; Auto Suspend
####
Ch 4: Snowflake Databases & Tables
Snowflake Databases and Data Storage
Need for Snowflake Warehouse, Compute
Database Objects and Hierarchy
Snowflake Worksheet Parameters
Database Creation with Snowflake UI
Retention Time, DB List & Connections
Permanent and Transient Databases
Snowflake Tables and their Usage
Permanent, Transient & Temp Tables
Creating Tables with SnowSight
Describe Options, Data Inserts
CREATE TABLE AS SELECT
Cloning Tables, Case Sensitivity Options
Collation, ALTER, DROP & UNDROP
#######
Ch 5: Time Travel & Transient Tables
Time Travel Feature in Snowflake
DML Operations and Silent Audits
Continuous Data Protection Life Cycle
Invoking Time Travel Feature in Snowflake
Timestamp, Offset & Query ID Options
Time Travel using Offset Feature
Time Travel using Query ID Feature
Data Recovery using TIMESTAMP
Using OFFSET options in Real-world
Fail Safe and UNDROP Operations
Transient Tables and Real-time Usage
Permanent Table and Real-time Usage
Restrictions with Permanent Tables
Identical Names and Naming Conflicts
######
## Schemas and Session Context
* Schemas: Creation, Real-time Usage
* Permanant and Transient Schemas
* Managed Schemas in Real-time Usage
* Verifying and Cloning Snowflake Schemas
* Invoking Schemas & Cloning Operations
* ALTER SCHEMA.. IF EXISTS Options
* Creating, Working with Managed Schema
* Snowflake Sessions (Workspaces)
* Session Context: Role and Warehouse
* Session Context: Database & Schema
* Working with Fully Qualified Names
* CTAS: Create Table As Select
* Data Loading with GUI, SQL Scripts
* Using Query and History Tab in GUI
######
#Mod 2: Snowflake Concepts, Admin
######
## Constraints and Data Types
* Snowflake Constraints, Data Validations
* NULL and NOT NULL Properties
* Unique, Primary and Foreign Keys
* Working with Named Constraints
* Single Column, Multi Column Constraints
* Inline and Out Of Line Constraints
* Table Constraints Types, Real-time Use
* Constraint Properties: ENFORCED
* DEFERRED, IMMEDIATE Options
* Numeric, String and Binary Data Types
* Boolean Data Types and Usage
* Date and Time Data Types
* Semi Structured Data Types
* Geospatial & Variant Data Types
######
## Snowflake Cloning (Zero Copy)
* Cloning Operations with Snowflake
* Zero Copy and Schema Level Cloning
* Real-time Uses: Cloning in Snowflake
* Snapshot Concept, Metadata Options
* Possible Objects for Snowflake Clone
* Permissions for Snowflake Cloning
* Accessing, Controlling Cloned Objects
* Real-time Considerations @ Cloning
* Storage Layer and Metadata Layer
* Cloning with Foreign Key Constraints
* Cloning Snowflake Databases
* Cloning Snowflake Schemas, Tables
* Security and MANAGE GRANTS
* Cloning and COPY GRANTS Options
######
## Snowflake Procedures & Views
* Snowflake Procedures and Functions
* Creating and Using Stored Procedures
* Using CALL Command in Snowflake
* SQL and JavaScript Options with SPs
* Snowflake Deferred Name Resolution
* Overloading with Snowflake SPs
* Transactions & Injection
* Variables & CALL Command
* Using execute() with Procedures
* sqlText:command, createStatement
* Cursoring Operations
* Dynamic DML Operations with SPs
* Loops & next.scan()
* RETURN and RETURNS Statements
* Views & Query Storage
* DML and SELECT Operations on Views
* Regular Views, System Predefined Views
* Recursive Views, Parameterized Views
#####
## Security Management
* Security Management Concepts
* Security Entities with Snowflake
* Securable Objects, Users & Roles
* Prevligies and Privilege Groups
* Snowflake Security Hierarchy
* Organization, Account, Users, Roles
* Schema, Tables, Other DB Objects
* Creating and Using Roles, Users
* System Defined Roles and Usage
* Role Hierarchy and Dependency
* Creating and Working with Users
* Auditing Users and Password Policy
* RBAC: Role Based Access Control
* DAC: Discretionary Access Control
######
## Snowflake Transactions
* Working with Transactions (ACID)
* Atomicity, Consistency, Isolation
* Durability and Data Storage Options
* Transaction Types with Snowflake
* Implicit, Explicit and Auto Commit
* BEGIN TRANSACTION & COMMIT
* DDL Statements and Transactions
* BEGIN WORK Versus START
* current_transaction() and Usage
* to_timestamp_ltz and Usage
* Failed Transactions with SPs
* Batches Versus Transactions
* Transactions with Stored Procedures
* Scoped and INNER Transactions
#####
## Snowflake Streams & Audits
* Working with Snowflake Streams
* Stream Object and DML Auditing
* Snapshot Creation, Offset Options
* METADATA$ACTION Parameter
* METADATA$ISUPDATE Parameter
* METADATA$ROW_ID Parameter
* Stream Types: Standard Stream
* Append Only Stream & Usage
* Insert Only Stream & Usage
* Data Flow with Snowflake Streams
* Auditing INSERT, UPDATE, DELETE
* show streams; desc streams
* Streams on Transient Tables
* Time Travel; Using Stream Tables
######
# ETL, Stages & Pipes
########
## Snowflake Tasks, Partitions
* Snowflake Tasks and Real-time Use
* Snowflake Serverless Compute Model
* User Managed & Snowflake Managed
* Tasks Tree: Root and Link Concepts
* Directed Acyclic Graph (DAG)
* Tasks Schedules and RESUME Options
* CREATE, ALTER, DESCRIBE, SHOW
* CREATE TASK … AFTER Statement
* ALTER TASK … ADD AFTER Statement
* CRON Syntax with Tasks, Procedures
* Virtual Warehouse Concepts (DWH)
* Multi Cluster Warehouse Config
* Client Utilities, Drivers & Connectors
* Auto Scale Options, Billing (Pricing)
###############
## SnowSQL and Variables
* SnowSQL: Concepts, Client Installation
* SnowSQL Tool: Configuration Options
* SnowSQL: Account Authorization
* DDL, DML and SELECT Operations
* Snowflake SQL Query Syntax Format
* SnowSQL Command Line Options
* Working with DB, Schema and Tables
* Snowflake Variables, Batch Processing
* Snowflake SQL Data Types, Usage
* DECLARE, LET, BEGIN and END
* EXECUTE IMMEDIATE, FOR, END FOR
* Creating Warehouse, Database, Tables
* Granting Permissions, Query Execution
* Writing Output to Files (Win, MAC OS)
#######
## Snowflake Partitions, Stages
* Snowflake Partitions, Real-time use
* Service and Storage Layer Concepts
* Micro Partition with DML, CDC
* Cluster Key: Usage, ReClusters
* Depth and Overlap Properties
* Internal Partition Types & Usage
* List, Range and Hash Partitions
* SYSTEM$CLUSTERING_INFORMATION
* Snowflake Stages and Usage
* Internal and External Stages
* User Stages : Creation, Usage
* Table Stages: Creation, Usage
* Internal Named Stages, Usage
* COPY Command, Bulk Data Loads
#####
## Azure & External Storage
* Working with Azure Storage
* Azure Subscription, Resources
* Create, Use Azure Storage Account
* Storage Containers, BLOB Data
* Using SnowSQL with Azure BLOB
* SAS [Shared Access Signature]
* Using SAS Key and FILE PATH
* External Stages in Snowflake
* File Formats: Creation, Usage
* Creating & Using External Stages
* Azure Storage with BLOB
* COPY INTO Command Usage
* Listing Stages with Snowflake
* Snowflake Patterns & RegEx
#######
## SnowPipes & Incremental Loads
* Snowflake SnowPipes: Incremental Loads & SnowPipe
* Creating Azure Account For BLOB
* Creating Container, Generate SAS Key
* Creating Azure Queue and Event Grid
* Notification Integration in Snowflake
* Integrations: Show, Describe, Use
* Azure Active Directory & IAM
* Linking Azure AD with Snowflake
* Enterprise Application, Authentication
* File Formats with Regular Expr
* External Data Stages
* Data Unloading Concepts
* PUT and GET with SnowSQL
* Bulk Unloads; Data Preparation & Stages
* Data Unloading to User Stages
* Data Unloading to Table Stages
## SQL
* DWH & Snowflake Introduction
* Snowflake Architecture
* Snowflake Service Form
* Snowflake Objects
* Using SnowSQL CLI, Snowflake Data
* Sequence in Snowflake
* Snowflake Stream & Tasks
* SnowSQL Operators, SnowSQL Concepts
* Data Loads & Validations
* Staging Operations with Snowflake
* Unloading Data from Snowflake
* Virtual Warehouse in Snowflake
* Replication and Business Continuity
* Security Management with Snowflake
* Snowflake with Azure Data Factory
* Managing Governance in Snowflake
* Snowflake: DWH and ETL
* Snowflake: Cloud Configurations
* Snowflake: DWH Architecture
* Snowflake: Database, Schemas
* Snowflake: Constraints, SP, Tasks
* Snowflake: Partitions & Tuning
* Snowflake: Security Management
* Snowflake: Automated Backups
* Snowflake: SnowSQL Concepts
* Snowflake: Variables & Tasks
* Snowflake: File Formats, Stages
* Snowflake: External Stages
* Snowflake: Azure Integration
* Snowflake: Snow Pipes & Upserts
* Snowflake: Power BI Analytics
