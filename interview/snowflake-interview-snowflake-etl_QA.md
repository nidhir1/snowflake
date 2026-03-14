# ❄️ Snowflake --- Tasks, Partitions, Stages & Bulk Loading


# 🔹 1️⃣ SNOWFLAKE TASKS & AUTOMATION

## Ques: What is a Snowflake Task?

Ans: A **Snowflake Task** is a built‑in scheduling mechanism that allows
Snowflake to automatically execute SQL statements or stored procedures.

Think of it like a **cron job inside Snowflake**.

Instead of running a query manually every day, you can create a task
that runs the query automatically.

Common use cases:

-   Daily ETL pipelines
-   Hourly aggregations
-   Incremental CDC processing
-   Data cleanup jobs

Example:

``` sql
CREATE TASK daily_sales_aggregation
WAREHOUSE = compute_wh
SCHEDULE = 'USING CRON 0 2 * * * UTC'
AS
INSERT INTO sales_summary
SELECT * FROM sales_raw;
```

This task runs **every day at 2 AM**.

------------------------------------------------------------------------

## Ques: Why do we use tasks?

Ans: Tasks are used to **automate data pipelines inside Snowflake**.

Without tasks:

1.  Developers must run queries manually
2.  Or depend on external tools like Airflow

With tasks, Snowflake can run pipelines automatically.

Typical scenarios:

  Use Case         Example
  ---------------- -----------------------------
  ETL pipelines    Raw → Transform → Analytics
  CDC pipelines    Process incremental changes
  Data refresh     Refresh dashboards
  Quality checks   Validate datasets

Tasks allow building **fully automated data pipelines directly in
Snowflake**.

------------------------------------------------------------------------

## Ques: What kind of SQL statements can tasks run?

Ans: Tasks can run almost any SQL Snowflake supports, including:

-   Data transformation queries
-   Stored procedures
-   Multi‑statement scripts

Example:

``` sql
INSERT INTO target_table
SELECT * FROM staging_table;
```

Tasks often orchestrate **complex transformation pipelines**.

------------------------------------------------------------------------

## Ques: How do tasks help automate ETL?

Ans: ETL pipelines usually contain multiple stages.

Example pipeline:

Step 1 → Load raw data\
Step 2 → Transform data\
Step 3 → Aggregate metrics\
Step 4 → Update reporting tables

Tasks allow these steps to run automatically in sequence.

Pipeline using tasks:

Load Task\
↓\
Transform Task\
↓\
Aggregate Task\
↓\
Reporting Task

This forms a **task workflow (DAG)**.

------------------------------------------------------------------------

## Ques: Do tasks require a warehouse?

Ans: Yes.

Tasks run SQL queries and SQL execution requires compute resources.

In Snowflake, compute is provided by **Virtual Warehouses**.

Example:

``` sql
WAREHOUSE = compute_wh
```

Without a warehouse, tasks cannot run.

------------------------------------------------------------------------

## Ques: What happens if the warehouse is suspended?

Ans: If the warehouse is suspended and **auto‑resume is enabled**,
Snowflake will:

1.  Automatically start the warehouse
2.  Run the task
3.  Suspend it again later if idle

This helps reduce compute cost.

------------------------------------------------------------------------

## Ques: What is a task owner?

Ans: The **task owner** is the role that created the task.

The owner controls:

-   Task configuration
-   Scheduling
-   Permissions
-   Execution

------------------------------------------------------------------------

# 🔹 REAL‑TIME USES OF TASKS

## Ques: What is scheduled ETL using tasks?

Ans: Scheduled ETL means running data pipelines automatically at fixed
intervals.

Example schedules:

  Schedule   Example
  ---------- -------------------
  Hourly     Update dashboards
  Daily      Load batch files
  Weekly     Refresh analytics

Example:

``` sql
SCHEDULE='1 hour'
```

------------------------------------------------------------------------

## Ques: How do tasks integrate with streams?

Ans: Snowflake **Streams capture table changes** (CDC).

They track:

-   Inserts
-   Updates
-   Deletes

Architecture:

Source Table\
↓\
Stream captures changes\
↓\
Task processes changes\
↓\
Target table updated

Example:

``` sql
WHEN SYSTEM$STREAM_HAS_DATA('orders_stream')
```

This runs the task **only when new changes exist**.

------------------------------------------------------------------------

## Ques: Can tasks trigger incrementally?

Ans: Yes.

Tasks can execute only when new data arrives.

Using streams ensures:

-   Incremental processing
-   Lower compute cost
-   Real‑time pipelines

------------------------------------------------------------------------

## Ques: How do tasks ensure exactly‑once processing?

Ans: Streams maintain **change offsets**.

Workflow:

1.  Task reads changes from stream
2.  Offset moves forward
3.  Same changes are not processed again

This ensures **exactly‑once CDC processing**.

------------------------------------------------------------------------

## Ques: What is task history?

Ans: Task history records execution details such as:

-   Execution time
-   Status (success/failure)
-   Query executed
-   Duration

Example:

``` sql
SELECT * FROM TABLE(INFORMATION_SCHEMA.TASK_HISTORY());
```

This helps monitor pipelines.

------------------------------------------------------------------------

## Ques: How do you identify failed tasks?

Ans: You can check:

1.  `TASK_HISTORY()`
2.  Snowflake UI monitoring

Look for status:

FAILED

Error messages also show the root cause.

------------------------------------------------------------------------

## Ques: What happens when task execution overlaps?

Ans: Snowflake prevents overlapping execution.

Example:

If a task runs every **5 minutes** but takes **10 minutes**, the next
run is skipped.

This prevents duplicate processing.

------------------------------------------------------------------------

## Ques: Can tasks be paused?

Ans: Yes.

Pause:

``` sql
ALTER TASK my_task SUSPEND;
```

Resume:

``` sql
ALTER TASK my_task RESUME;
```

------------------------------------------------------------------------

# 🔹 DAG (TASK WORKFLOWS)

## Ques: What is a DAG?

Ans: DAG stands for **Directed Acyclic Graph**.

It represents task dependencies where execution follows a specific order
and no circular dependencies exist.

Example:

Task A → Task B → Task C

Pipeline example:

Load Data\
↓\
Transform Data\
↓\
Aggregate Data

------------------------------------------------------------------------

## Ques: Why do tasks use DAG dependency?

Ans: Data pipelines require correct ordering.

Example:

You cannot transform data before loading it.

DAG ensures pipeline stages execute in the correct sequence.

------------------------------------------------------------------------

## Ques: What is a parent task?

Ans: A **parent task** runs before dependent tasks.

Example:

Load Data Task

------------------------------------------------------------------------

## Ques: What is a child task?

Ans: A **child task** runs after the parent task completes.

Example:

Transform Data Task

------------------------------------------------------------------------

## Ques: What happens when a parent task fails?

Ans: Child tasks will not run.

This prevents pipeline corruption and ensures data consistency.

------------------------------------------------------------------------

# 🔹 2️⃣ MICRO‑PARTITIONS

## Ques: What are micro‑partitions?

Ans: Snowflake stores data internally in **micro‑partitions**.

They are:

-   Columnar
-   Compressed
-   Immutable storage units

Typical size:

50 MB -- 500 MB uncompressed

Users do not manage partitions manually --- Snowflake handles it
automatically.

------------------------------------------------------------------------

## Ques: What metadata is stored for micro‑partitions?

Ans: Snowflake stores metadata such as:

-   Minimum value per column
-   Maximum value per column
-   Row count
-   Column statistics

This metadata helps optimize queries.

------------------------------------------------------------------------

## Ques: What is pruning?

Ans: Partition pruning means skipping partitions that do not match query
filters.

Example:

``` sql
SELECT * FROM orders
WHERE order_date = '2024-01-01';
```

Snowflake scans only partitions containing that date.

Benefits:

-   Less data scanned
-   Faster queries
-   Lower cost

------------------------------------------------------------------------

# 🔹 3️⃣ STAGES

## Ques: What is a stage?

Ans: A **stage** is a location used to store data files before loading
them into Snowflake tables.

Workflow:

S3 Bucket\
↓\
Snowflake Stage\
↓\
COPY INTO table

Stages act as **data landing zones**.

------------------------------------------------------------------------

## Ques: What are internal stages?

Ans: Internal stages store files inside Snowflake.

Examples:

-   User stage
-   Table stage
-   Named stage

------------------------------------------------------------------------

## Ques: What are external stages?

Ans: External stages reference external cloud storage.

Examples:

-   AWS S3
-   Azure Blob Storage
-   Google Cloud Storage

Example:

``` sql
CREATE STAGE sales_stage
URL='s3://sales-data/';
```

------------------------------------------------------------------------

# 🔹 4️⃣ BULK LOADING

## Ques: What is bulk loading?

Ans: Bulk loading means loading large datasets efficiently in batches.

Snowflake uses:

COPY INTO command

Example:

``` sql
COPY INTO sales_table
FROM @sales_stage
FILE_FORMAT = (TYPE = CSV);
```

Snowflake automatically:

-   Loads files in parallel
-   Distributes workload across compute nodes
-   Optimizes ingestion performance

------------------------------------------------------------------------

## Ques: Why do small files hurt performance?

Ans: Snowflake loads files in parallel.

If files are too small:

-   Too many files
-   High overhead
-   Inefficient compute usage

Recommended size:

100 MB -- 250 MB compressed
