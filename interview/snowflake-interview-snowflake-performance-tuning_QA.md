# ❄️ Snowflake --- Performance Tuning & Optimization




# 🔹 1️⃣ INDEXES & PERFORMANCE TUNING

## Ques: Does Snowflake support traditional indexes?

Ans: No. Snowflake does **not support traditional B‑Tree or OLTP style
indexes** like Oracle, SQL Server, or MySQL.

In traditional databases, indexes are used to quickly locate rows.
However, maintaining indexes adds overhead during writes and increases
complexity.

Snowflake instead relies on:

-   Micro-partitions
-   Partition pruning
-   Metadata statistics
-   Clustering
-   Search Optimization Service

These techniques allow Snowflake to avoid the maintenance cost of
indexes while still enabling fast queries.

------------------------------------------------------------------------

## Ques: Why doesn't Snowflake use OLTP-style indexes?

Ans: Snowflake is designed as an **OLAP data warehouse**, not an OLTP
system.

OLTP databases: - Handle many small transactions - Use indexes for row
lookups

Snowflake workloads: - Scan large datasets - Perform analytics and
aggregations

Indexes would:

-   Increase storage overhead
-   Slow down bulk ingestion
-   Require constant maintenance

Instead Snowflake optimizes **large scan queries using columnar storage
and metadata pruning**.

------------------------------------------------------------------------

## Ques: What replaces indexes in Snowflake?

Ans: Snowflake uses several techniques instead of indexes:

1.  **Micro-partitions**
2.  **Partition pruning**
3.  **Clustering keys**
4.  **Search Optimization Service**
5.  **Caching layers**

These allow Snowflake to locate data efficiently without maintaining
separate index structures.

------------------------------------------------------------------------

## Ques: What are micro-partitions?

Ans: Micro-partitions are the **fundamental storage units in Snowflake
tables**.

Key characteristics:

-   Columnar storage
-   Immutable
-   Automatically created
-   Compressed

Typical size:

50 MB -- 500 MB uncompressed

Snowflake automatically manages micro-partitions during data loading.

Users never manually create partitions.

------------------------------------------------------------------------

## Ques: What is partition pruning and how does it work?

Ans: Partition pruning means **skipping micro-partitions that do not
match a query filter**.

Example query:

SELECT \* FROM orders WHERE order_date = '2024-01-01'

Snowflake checks metadata for each partition:

-   Min order_date
-   Max order_date

If the partition does not contain the requested date, Snowflake skips it
entirely.

Benefits:

-   Less data scanned
-   Faster queries
-   Lower compute cost

------------------------------------------------------------------------

## Ques: Does Snowflake support secondary indexes?

Ans: No. Snowflake does not support secondary indexes.

Instead, it relies on:

-   Automatic partition pruning
-   Clustering
-   Search optimization

These approaches work better for analytical workloads.

------------------------------------------------------------------------

## Ques: What happens when workloads require faster lookups?

Ans: If workloads require **point lookups or selective filtering**,
Snowflake provides:

**Search Optimization Service**.

This feature builds specialized access paths to accelerate selective
queries.

Example use cases:

-   Customer lookup by ID
-   JSON field search
-   Semi‑structured data queries

------------------------------------------------------------------------

## Ques: When is Search Optimization Service appropriate?

Ans: Search Optimization is useful when:

-   Queries filter on highly selective columns
-   Lookup style queries exist
-   JSON path filtering is used
-   Tables are extremely large

Example:

SELECT \* FROM users WHERE user_id = 12345

Without search optimization, Snowflake may scan multiple partitions.

With it, Snowflake finds rows much faster.

------------------------------------------------------------------------

# 🔹 GENERAL PERFORMANCE TUNING

## Ques: Why is query design more important than warehouse size?

Ans: Bad queries will remain slow even with larger warehouses.

Example of poor query:

SELECT \* FROM large_table WHERE CAST(order_date AS STRING) =
'2024-01-01'

Problems:

-   SELECT \*
-   unnecessary casting
-   prevents pruning

Even a large warehouse must scan massive data.

Good query design reduces scanned data before scaling compute.

------------------------------------------------------------------------

## Ques: When should you increase warehouse size?

Ans: Increase warehouse size when:

-   Queries are CPU intensive
-   Large joins are required
-   Heavy aggregations exist
-   Parallel processing can help

Scaling up increases:

-   CPU
-   memory
-   parallelism

------------------------------------------------------------------------

## Ques: When should scaling out be used instead of scaling up?

Ans: Scaling out means **multi-cluster warehouses**.

Use this when:

-   Many users run queries simultaneously
-   Workloads are highly concurrent
-   Dashboards generate multiple queries

Scaling out helps concurrency rather than single query speed.

------------------------------------------------------------------------

## Ques: What is the cost vs performance balance in Snowflake?

Ans: Snowflake allows scaling compute easily, but more compute means
higher cost.

Best practice:

1.  Optimize query
2.  Reduce scanned data
3.  Use clustering
4.  Scale warehouse only if necessary

Goal:

**Maximum performance with minimal compute credits.**

------------------------------------------------------------------------

## Ques: How does Snowflake parallelize queries?

Ans: Snowflake splits queries across multiple nodes.

Parallelization occurs across:

-   micro-partitions
-   CPU cores
-   worker nodes

Large tables are processed simultaneously across compute nodes.

------------------------------------------------------------------------

## Ques: How does result reuse improve performance?

Ans: Snowflake stores results of executed queries in the **Result
Cache**.

If the exact same query is run again and data has not changed:

Snowflake returns results instantly without running the query again.

This improves performance and eliminates compute cost.

------------------------------------------------------------------------

## Ques: Why should SELECT \* be avoided?

Ans: SELECT \* forces Snowflake to read **every column** in a table.

Problems:

-   larger scans
-   unnecessary IO
-   slower queries

Better practice:

SELECT only required columns.

------------------------------------------------------------------------

# 🔹 2️⃣ CLUSTERING & PARTITIONING

## Ques: What is the difference between partitions and cluster keys?

Ans:

Partitions: - Automatically created - Managed internally by Snowflake

Cluster keys: - Optional - Organize data to improve pruning

Partitions exist always, clustering is used only when needed.

------------------------------------------------------------------------

## Ques: What problem do cluster keys solve?

Ans: Cluster keys help when queries filter on specific columns
frequently.

Example:

WHERE order_date BETWEEN '2024-01-01' AND '2024-01-31'

Clustering organizes data by that column so partitions contain similar
values.

This improves pruning efficiency.

------------------------------------------------------------------------

## Ques: When should cluster keys be avoided?

Ans: Avoid clustering when:

-   tables are small
-   queries scan entire table
-   workloads change frequently
-   clustering cost exceeds benefits

Clustering consumes compute credits.

------------------------------------------------------------------------

# 🔹 3️⃣ QUERY OPTIMIZATION

## Ques: What is the Query Profiler?

Ans: Query Profiler is a visual tool in Snowflake UI that shows **how a
query was executed**.

It displays:

-   execution stages
-   scan size
-   join operations
-   time spent per step

This helps identify performance bottlenecks.

------------------------------------------------------------------------

## Ques: What is remote spilling?

Ans: Remote spilling occurs when intermediate query data exceeds
available memory.

Snowflake temporarily writes data to remote storage.

This slows queries because disk IO is slower than memory processing.

Solution:

-   Increase warehouse size
-   Optimize joins
-   Reduce intermediate results

------------------------------------------------------------------------

# 🔹 4️⃣ CACHING LAYERS

## Ques: What is result caching?

Ans: Result caching stores the **final results of executed queries**.

If the same query runs again and underlying data has not changed,
Snowflake returns results instantly.

Result cache duration:

24 hours.

------------------------------------------------------------------------

## Ques: What is metadata caching?

Ans: Metadata caching stores information about:

-   table structure
-   partition statistics
-   file locations

This helps Snowflake plan queries faster.

------------------------------------------------------------------------

## Ques: What is warehouse data cache?

Ans: Warehouse cache stores **recently accessed table data in memory or
SSD** on the warehouse nodes.

If the same data is queried again while the warehouse remains active,
queries run faster.

------------------------------------------------------------------------

# 🔹 5️⃣ SCENARIO‑BASED THINKING

## Ques: Query slow only the first time --- why?

Ans: The first execution reads data from storage.

Later executions benefit from:

-   Result cache
-   Data cache
-   Metadata cache

Therefore queries become faster.

------------------------------------------------------------------------

## Ques: Warehouse scaled up but performance didn't improve --- why?

Ans: Possible reasons:

-   Query scanning too much data
-   Poor filtering
-   Bad join conditions
-   Partition pruning not happening

Scaling compute cannot fix inefficient queries.

------------------------------------------------------------------------

## Ques: Analysts constantly using SELECT \* --- impact?

Ans: Major impact on performance:

-   larger scans
-   unnecessary IO
-   slower dashboards

Best practice:

Select only needed columns.

------------------------------------------------------------------------

# 🔹 6️⃣ TRICK / MISCONCEPTIONS

## Ques: Does Snowflake automatically optimize everything?

Ans: No.

Snowflake optimizes many operations automatically, but:

-   poor query design
-   inefficient joins
-   unnecessary scans

can still cause slow performance.

------------------------------------------------------------------------

## Ques: Do bigger warehouses always run faster?

Ans: Not always.

If queries scan huge datasets unnecessarily, larger warehouses will only
increase cost without solving the root problem.

------------------------------------------------------------------------

## Ques: Does clustering always improve speed?

Ans: No.

Clustering helps only when queries filter on clustered columns.

Otherwise it adds unnecessary cost.

------------------------------------------------------------------------
