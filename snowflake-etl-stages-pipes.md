# Snowflake ETL, Stages & Pipes

## 1. Snowflake Tasks and Partitions

### Snowflake Tasks and Real-time Use
**Snowflake Tasks** automate SQL execution based on time or event triggers, enabling real-time ETL and orchestration.  
They can run single SQL statements or call stored procedures for complex logic.

**Example:**
```sql
CREATE OR REPLACE TASK load_task
  WAREHOUSE = my_wh
  SCHEDULE = 'USING CRON 0 * * * * UTC'
AS
  CALL load_new_data();
```

- Tasks can **chain together** to form dependency-driven workflows.
- Each task executes **in isolation**, maintaining transactional integrity.
- Tasks are ideal for **CDC ingestion**, **data refresh**, and **audit updates**.

### Directed Acyclic Graph (DAG)
Tasks can be organized in a **DAG structure**, where child tasks depend on parent completion.

**Example:**
```sql
CREATE OR REPLACE TASK task_a ...;
CREATE OR REPLACE TASK task_b AFTER task_a ...;
CREATE OR REPLACE TASK task_c AFTER task_b ...;
```

Execution flows in one direction without loops, ensuring predictable scheduling.  
Snowflake automatically manages dependencies and retries in case of failures.

### Tasks Schedules and RESUME Options
- Use **CRON syntax** or **AFTER** dependencies for scheduling.  
- A task must be **resumed** after creation to activate it.

```sql
ALTER TASK load_task RESUME;
SHOW TASKS;
```

- Tasks can be **paused/resumed** anytime for maintenance.  
- You can set parameters like **ALLOW_OVERLAP_EXECUTION = FALSE** to prevent concurrent runs.

---

## 2. Snowflake Partitions

### Micro Partition with DML, CDC
Snowflake stores data in **micro-partitions** (50–500 MB each), automatically optimized for performance.  
Each micro-partition contains metadata (min/max values, compression stats, etc.) that supports **pruning** and **incremental processing**.

DML operations (INSERT/UPDATE/DELETE) affect only modified partitions, ensuring **high concurrency** and **minimal overhead**.

### Cluster Key: Usage and ReClusters
A **Cluster Key** improves query performance by organizing micro-partitions based on specified columns.

**Example:**
```sql
ALTER TABLE transactions CLUSTER BY (region_id, txn_date);
```

- Helps optimize large analytic queries.  
- **Re-clustering** automatically reorganizes partitions as data grows.  
- Use the `SYSTEM$CLUSTERING_INFORMATION()` function to evaluate clustering efficiency.

### Internal Partition Types & Usage
| Partition Type | Description |
|-----------------|--------------|
| **Automatic** | Default behavior, managed entirely by Snowflake |
| **Manual (Cluster Key)** | Explicit user-defined column ordering |
| **Reclustered** | Automatically optimized after DML operations |

Snowflake ensures optimal partitioning for storage and query performance without user intervention.

---

## 3. Snowflake Stages

### Internal and External Stages
Stages are **intermediate storage locations** for loading/unloading data.  
They can be either internal (within Snowflake) or external (AWS S3, Azure Blob, or GCS).

| Stage Type | Description |
|-------------|--------------|
| **User Stage** | Each user has a private internal stage (`@~`) |
| **Table Stage** | Each table has its own stage (`@%table_name`) |
| **Named Stage** | Custom stage with defined URL and credentials |

### User Stages and Table Stages
**User Stage:**
```sql
PUT file://data/customers.csv @~;
LIST @~;
COPY INTO customers FROM @~ FILE_FORMAT = (TYPE = CSV);
```

**Table Stage:**
```sql
PUT file://data/orders.csv @%orders;
COPY INTO orders FROM @%orders;
```

### COPY Command and Bulk Data Loads
The **COPY INTO** command enables efficient parallel loading of large datasets from stages into Snowflake tables.

**Example:**
```sql
COPY INTO sales
FROM @my_s3_stage/sales_data/
FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY='"')
ON_ERROR = 'CONTINUE';
```

- Supports both **structured and semi-structured data (JSON, Parquet)**.  
- Automatically scales parallelism for large file ingestion.  
- Can be combined with **Tasks and Streams** for continuous CDC ingestion.

---

## Summary

| Concept | Description |
|----------|--------------|
| **Tasks** | Automate SQL or procedural ETL workflows |
| **DAG** | Defines task dependencies for orchestrated flow |
| **Micro-partition** | Optimized storage layer enabling high-performance queries |
| **Cluster Key** | Logical ordering key for query optimization |
| **Stages** | Intermediate storage for data loading/unloading |
| **COPY Command** | High-speed bulk data ingestion into Snowflake |

---

**References:**
- [Snowflake Tasks Documentation](https://docs.snowflake.com/en/user-guide/tasks-intro)
- [Snowflake Micro-partitions](https://docs.snowflake.com/en/user-guide/tables-clustering-micro-partitions)
- [Snowflake Stages and Data Loading](https://docs.snowflake.com/en/user-guide/data-load-overview)
