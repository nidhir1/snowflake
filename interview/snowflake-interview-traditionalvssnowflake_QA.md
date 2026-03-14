# TRADITIONAL RDBMS vs SNOWFLAKE --- Detailed Q&A

------------------------------------------------------------------------

## 🔹 1. BASIC --- FUNDAMENTAL QUESTIONS

### Ques: What is a traditional RDBMS?

**Ans:**\
A traditional RDBMS (Relational Database Management System) is a
database system that stores data in structured tables with rows and
columns and enforces relationships between those tables using keys and
constraints. It is designed primarily for transactional workloads where
data integrity, consistency, and quick updates are critical.

------------------------------------------------------------------------

### Ques: Examples of traditional RDBMS.

**Ans:**\
Common examples include:

-   Oracle Database\
-   Microsoft SQL Server\
-   MySQL\
-   PostgreSQL\
-   IBM DB2

These systems are commonly used in applications like banking systems,
ERP systems, and e‑commerce platforms.

------------------------------------------------------------------------

### Ques: What is Snowflake?

**Ans:**\
Snowflake is a cloud‑native data platform designed for large‑scale
analytics and data warehousing. It runs entirely in the cloud and
separates compute from storage, allowing organizations to scale
resources independently for analytics workloads.

------------------------------------------------------------------------

### Ques: Is Snowflake an RDBMS?

**Ans:**\
Yes, Snowflake is technically a relational database system because it
supports tables, SQL queries, joins, and relational data models.
However, it is optimized for **analytical workloads (OLAP)** rather than
transactional workloads (OLTP).

------------------------------------------------------------------------

### Ques: What is the basic purpose of an RDBMS?

**Ans:**\
Traditional RDBMS systems are primarily designed for **transaction
processing**, where systems must support frequent inserts, updates, and
deletes with strict consistency and reliability.

Examples include banking transactions, order processing systems, and
inventory management.

------------------------------------------------------------------------

### Ques: What is the basic purpose of Snowflake?

**Ans:**\
Snowflake is designed primarily for **analytics and data warehousing**,
where organizations analyze large datasets to generate insights,
reports, dashboards, and machine learning features.

------------------------------------------------------------------------

### Ques: What is OLTP?

**Ans:**\
OLTP stands for **Online Transaction Processing**. It refers to systems
designed to handle large numbers of small transactions such as inserts,
updates, and deletes.

Example: placing an order on an e‑commerce website.

------------------------------------------------------------------------

### Ques: What is OLAP?

**Ans:**\
OLAP stands for **Online Analytical Processing**. These systems process
large datasets for reporting, business intelligence, and analytics
rather than transactional operations.

Example: generating a sales report for the last five years.

------------------------------------------------------------------------

### Ques: Which systems are generally used for OLTP?

**Ans:**\
Traditional relational databases such as:

-   Oracle
-   SQL Server
-   MySQL
-   PostgreSQL

These systems handle high‑frequency transactional workloads.

------------------------------------------------------------------------

### Ques: Which systems are generally used for OLAP?

**Ans:**\
Analytical platforms such as:

-   Snowflake
-   Amazon Redshift
-   Google BigQuery
-   Azure Synapse

These systems are optimized for querying very large datasets.

------------------------------------------------------------------------

### Ques: Why do enterprises separate OLTP and OLAP?

**Ans:**\
Combining transactional and analytical workloads in the same system can
cause performance problems. Analytical queries scan large datasets,
which can slow down transactional systems. Separating OLTP and OLAP
allows each system to specialize and scale appropriately.

------------------------------------------------------------------------

### Ques: What is schema-on-write?

**Ans:**\
Schema‑on‑write means data must conform to a predefined schema before
being stored. This is typical in traditional relational databases where
tables have strict structures.

------------------------------------------------------------------------

### Ques: What is schema-on-read?

**Ans:**\
Schema‑on‑read means data is stored first and the structure is applied
later when it is queried. This approach is more flexible and commonly
used in modern analytics platforms.

------------------------------------------------------------------------

### Ques: Which approach is typical for RDBMS?

**Ans:**\
Traditional RDBMS systems generally use **schema‑on‑write**, requiring
data to match predefined schemas before insertion.

------------------------------------------------------------------------

### Ques: Which approach is common in Snowflake?

**Ans:**\
Snowflake supports both structured and semi‑structured data and often
uses a flexible approach closer to **schema‑on‑read**, especially when
working with JSON or VARIANT data types.

------------------------------------------------------------------------

### Ques: What are indexes in RDBMS?

**Ans:**\
Indexes are specialized data structures used to speed up query
performance by allowing the database to locate rows quickly instead of
scanning entire tables.

------------------------------------------------------------------------

### Ques: Does Snowflake require indexes?

**Ans:**\
No. Snowflake automatically manages data organization using
**micro‑partitions and metadata pruning**, eliminating the need for
manual indexing.

------------------------------------------------------------------------

### Ques: What is storage scaling in RDBMS?

**Ans:**\
In traditional databases, storage and compute resources are usually tied
together. Scaling storage often requires scaling the entire database
server.

------------------------------------------------------------------------

### Ques: What is compute scaling in Snowflake?

**Ans:**\
Snowflake separates compute from storage. This means storage can grow
independently while compute resources (virtual warehouses) can scale up
or down as needed.

------------------------------------------------------------------------

### Ques: Can Snowflake be used for reporting and analytics?

**Ans:**\
Yes. Snowflake is designed specifically for analytics, dashboards,
reporting systems, and large‑scale data analysis.

------------------------------------------------------------------------

## 🔹 2. INTERMEDIATE --- UNDERSTANDING KEY DIFFERENCES

### Ques: How is performance tuning done in RDBMS?

**Ans:**\
Performance tuning in traditional databases often requires manual work
such as creating indexes, optimizing queries, partitioning tables, and
adjusting hardware resources.

------------------------------------------------------------------------

### Ques: How is performance tuning handled in Snowflake?

**Ans:**\
Snowflake handles much of the optimization automatically through
micro‑partition pruning, automatic clustering, and scalable compute
warehouses.

------------------------------------------------------------------------

### Ques: What is manual partitioning?

**Ans:**\
Manual partitioning means dividing large tables into smaller pieces
based on keys such as date ranges. This must usually be configured and
maintained manually in traditional databases.

------------------------------------------------------------------------

### Ques: Does Snowflake require partition management?

**Ans:**\
No. Snowflake automatically partitions data into micro‑partitions and
manages them internally.

------------------------------------------------------------------------

### Ques: How are backups handled in RDBMS?

**Ans:**\
Traditional databases rely on scheduled backups, replication, and
disaster recovery processes managed by administrators.

------------------------------------------------------------------------

### Ques: How are backups and recovery handled in Snowflake?

**Ans:**\
Snowflake uses built‑in features such as **Time Travel, Fail‑safe, and
replication** to manage recovery automatically without manual backups.

------------------------------------------------------------------------

### Ques: What is clustering in traditional databases?

**Ans:**\
Clustering usually refers to organizing table data based on indexed
columns to improve query performance.

------------------------------------------------------------------------

### Ques: What is clustering in Snowflake?

**Ans:**\
Snowflake clustering refers to defining **cluster keys** that help
Snowflake organize micro‑partitions for better pruning during queries.

------------------------------------------------------------------------

### Ques: What is sharding?

**Ans:**\
Sharding is a technique used to distribute data across multiple database
servers to handle very large datasets.

------------------------------------------------------------------------

### Ques: Does Snowflake require manual sharding?

**Ans:**\
No. Snowflake automatically distributes data across storage and compute
layers without requiring manual sharding.

------------------------------------------------------------------------

### Ques: How do RDBMS systems scale?

**Ans:**\
Most traditional systems scale **vertically**, meaning more CPU, RAM, or
storage must be added to a single server.

------------------------------------------------------------------------

### Ques: How does Snowflake scale?

**Ans:**\
Snowflake scales **horizontally** using independent compute clusters
called virtual warehouses.

------------------------------------------------------------------------

## 🔹 3. ADVANCED --- ARCHITECTURE & DESIGN THINKING

### Ques: What is shared‑disk architecture?

**Ans:**\
In shared‑disk architecture, multiple compute nodes share access to the
same storage system.

------------------------------------------------------------------------

### Ques: What is shared‑nothing architecture?

**Ans:**\
Shared‑nothing systems distribute both storage and compute across nodes,
and each node operates independently.

------------------------------------------------------------------------

### Ques: What is multi‑cluster shared data architecture?

**Ans:**\
This architecture separates compute clusters from shared storage,
allowing multiple compute clusters to access the same data
simultaneously.

------------------------------------------------------------------------

### Ques: Which category does Snowflake belong to?

**Ans:**\
Snowflake uses a **multi‑cluster shared data architecture**, which
separates compute and storage layers.

------------------------------------------------------------------------

### Ques: How does Snowflake separate compute and storage?

**Ans:**\
Data is stored in cloud object storage, while compute is handled by
independent virtual warehouses that can be scaled or paused
independently.

------------------------------------------------------------------------

### Ques: Why is separation of compute important?

**Ans:**\
It allows multiple teams to run queries without interfering with each
other and enables elastic scaling of compute resources.

------------------------------------------------------------------------

### Ques: What happens if multiple teams query the same data in RDBMS?

**Ans:**\
They compete for the same compute resources, which can lead to
performance degradation.

------------------------------------------------------------------------

### Ques: How does Snowflake avoid resource contention?

**Ans:**\
Different teams can use separate warehouses, each with its own compute
resources.

------------------------------------------------------------------------

### Ques: What is workload isolation?

**Ans:**\
Workload isolation means separating different workloads so that one
team's queries do not affect another team's performance.

------------------------------------------------------------------------

### Ques: Why is workload isolation critical in analytics?

**Ans:**\
Analytics queries can be large and resource‑intensive. Isolation ensures
stable performance for all teams.

------------------------------------------------------------------------

### Ques: How is semi‑structured data handled in RDBMS?

**Ans:**\
Traditional databases usually require complex parsing or special
extensions to work with JSON or XML data.

------------------------------------------------------------------------

### Ques: How is semi‑structured data handled in Snowflake?

**Ans:**\
Snowflake supports a **VARIANT data type**, which allows JSON, XML, and
other semi‑structured formats to be stored and queried directly.

------------------------------------------------------------------------

### Ques: How does Snowflake securely share data without copying?

**Ans:**\
Snowflake supports **secure data sharing**, allowing data to be shared
across accounts without physically duplicating the dataset.

------------------------------------------------------------------------

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS

### Ques: Does Snowflake replace OLTP systems?

**Ans:**\
No. Snowflake is designed for analytics and should not replace
transactional databases.

------------------------------------------------------------------------

### Ques: Is Snowflake just a faster database?

**Ans:**\
No. Snowflake is a **cloud data platform with a different
architecture**, not simply a faster version of traditional databases.

------------------------------------------------------------------------

### Ques: Do we create indexes in Snowflake for tuning?

**Ans:**\
No. Snowflake uses automatic micro‑partition pruning instead of manual
indexing.

------------------------------------------------------------------------

### Ques: Does Snowflake eliminate the need for good data modeling?

**Ans:**\
No. Good schema design and data modeling remain critical for performance
and maintainability.
