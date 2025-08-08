## Basic Interview Questions:

#### QUES 1: What is Snowflake? What are the essential features of Snowflake?
##### Answer: Snowflake is a cloud-based data warehousing platform that separates compute from storage, allowing users to scale their processing resources and data storage independently. This process is more cost-effective and produces high performance.


Features of Snowflake:
* Automatic Scaling/Elasticity: Snowflake’s architecture allows for dynamic scaling of both compute and storage resources. It can automatically scale up or down based on workload requirements, ensuring optimal performance and cost efficiency.
* Separation of Compute and Storage: Snowflake separates the storage and compute layers, enabling independent scaling of both components. This flexibility allows businesses to scale compute resources for high-demand workloads without affecting storage and vice versa.
* Native Support for Semi-structured Data: Snowflake natively supports semi-structured data formats like JSON, Avro, and Parquet, which eliminates the need for pre-processing before ingestion.
* Zero Management: Snowflake is a fully managed service, meaning that it takes care of database management tasks like indexing, tuning, and partitioning, reducing administrative overhead.
* Concurrency: Snowflake can handle multiple concurrent users and workloads without impacting performance, thanks to its multi-cluster architecture.
* Data Sharing: Snowflake allows businesses to securely share data in real-time across different organizations without the need to replicate or move the data, enhancing collaboration.
* Security and Compliance: Snowflake includes robust security measures such as encryption (at rest and in transit), role-based access control (RBAC), multi-factor authentication, and compliance with standards like HIPAA and PCI DSS.
* Time Travel: Allows you to access historical data (up to 90 days) using AT or BEFORE clause.
* Zero-copy cloning: Creates a copy of database/table without duplicating the data — fast and storage-efficient.

#### Ques2:	What are Virtual Warehouses in Snowflake?
##### Virtual Warehouses are the compute resources used to perform all DML (Data Manipulation Language) operations, such as:
*	Querying data
*	Loading data
*	Unloading data
*	Cloning
*	Creating tables/views
* They do not store data — storage is separate.Instead, warehouses handle computation and processing.

##### Key Points:
* Dedicated compute clusters that can be started, stopped, and resized independently.

* Each warehouse consists of a cluster of servers running in the cloud.

* Warehouses are independent of storage, allowing scalable compute without impacting data storage.

* They enable concurrent query execution by isolating workloads across multiple warehouses.

* You pay for the compute resources (credits) only when warehouses are running.

* Warehouses can be configured for auto-suspend and auto-resume to save costs.

| Feature     | Description                             |
| ----------- | --------------------------------------- |
| Purpose     | Perform all compute operations          |
| Scalability | Independently sized and scaled          |
| Isolation   | Separates workloads to avoid contention |
| Cost        | Pay per use while warehouse is running  |

##### Interview Tip:
* Think of virtual warehouses as Snowflake’s “compute engines” that power your data workloads with flexibility and elasticity.

#### 2.	What is a Snowflake Schema?
##### A Snowflake Schema is a type of data warehouse schema design that is a normalized version of a star schema. In a snowflake schema:
*	Dimension tables are split into multiple related tables (normalized).
*	This creates a "snowflake" shape in the ER diagram — hence the name.
##### 🧱 Structure of a Snowflake Schema
*	Fact Table (central): Contains measurable, quantitative data (e.g., sales amount, revenue).
*	Dimension Tables (surrounding): Describe the context (e.g., customer, product, time).
*	Each dimension may have sub-dimensions, which leads to a multi-level structure.

#### Ques 3.	Difference between Database, Schema, and Table in Snowflake?
##### These are hierarchical structures used to organize and manage data in Snowflake.

| **Component** | **Description**                                                            | **Example**                     |
| ------------- | -------------------------------------------------------------------------- | ------------------------------- |
| **Database**  | Top-level container in Snowflake. It groups schemas together.              | `SALES_DB`                      |
| **Schema**    | A logical grouping of database objects like tables, views, functions, etc. | `PUBLIC`, `RAW_DATA`, `STAGING` |
| **Table**     | Stores actual data in rows and columns. Defined inside a schema.           | `CUSTOMERS`, `ORDERS`           |


📘 Hierarchy
 Database  →  Schema  →  Table

	
#### Ques 4.	What is Fail-safe in Snowflake?
##### Fail-safe in Snowflake is a 7-day period after the Time Travel retention period during which Snowflake can recover historical data, but only in emergencies — like system failures or accidental data loss.
________________________________________
#### 🕒 Fail-safe Lifecycle
1.	Active Data: Data is available and queryable.
2.	Time Travel Period (up to 90 days):
o	You can manually query or restore dropped/changed data using AT or BEFORE clauses.
3.	Fail-safe Period (7 days after Time Travel ends):
*	You cannot access the data directly.
*	Only Snowflake Support can recover data for disaster recovery.


#### Ques 5. What is Time Travel in Snowflake?
##### Time Travel in Snowflake is a feature that allows you to query, clone, and restore data from a previous point in time.
It’s like having a rewind button for your tables, schemas, and databases — useful for recovering from mistakes, auditing, or recreating past states of your data.
________________________________________
Key Points
1.	Purpose
o	Recover data that was deleted or modified.
o	Inspect historical versions for audits and debugging.
o	Create clones of tables/schemas/databases from a historical point.
2.	Default Retention Period
o	Standard Edition: 1 day (24 hours).
o	Enterprise Edition & above: Up to 90 days.
o	Configurable at table, schema, or database level.
3.	How it Works
o	Snowflake stores immutable micro-partitions.
o	When data changes, Snowflake doesn’t overwrite; it creates new micro-partitions and keeps old ones until the Time Travel retention expires.
o	Queries with AT or BEFORE retrieve those historical micro-partitions.
________________________________________
Example Usage
```sql
-- Query a table as it was 2 hours ago
SELECT * 
FROM SALES_DB.PUBLIC.CUSTOMERS
AT(OFFSET => -2*60*60);

-- Query table as it was before a specific timestamp
SELECT * 
FROM SALES_DB.PUBLIC.CUSTOMERS
BEFORE(TIMESTAMP => '2025-08-08 08:00:00');

-- Clone a table from 3 days ago
CREATE TABLE CUSTOMERS_CLONE 
CLONE SALES_DB.PUBLIC.CUSTOMERS
AT(TIMESTAMP => '2025-08-05 10:00:00');

-- Undrop a table
UNDROP TABLE SALES_DB.PUBLIC.CUSTOMERS;
```
________________________________________
Benefits
*	Accidental deletion recovery (no panic needed when someone runs DELETE without a WHERE).
*	Zero-copy cloning for testing older states   *without using extra storage.
*	Audit & compliance — see exactly what data looked like in the past.

