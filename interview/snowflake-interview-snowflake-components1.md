# Snowflake — Components, Objects, Warehouses, Stages and Platform Features
Complete Questions and Answers Guide

---

## 1. BASIC — CORE COMPONENTS OVERVIEW

### What are the main components of Snowflake?
Snowflake is organized into three logical layers:

1) Storage layer – Snowflake stores data in compressed, encrypted, columnar micro-partitions. There is no manual index, partition, or storage tuning required.  
2) Compute layer – Virtual Warehouses execute SQL, transformations, loads, and analytics. Each warehouse is isolated from others.  
3) Services layer – Coordinates authentication, optimization, metadata, transactions, security, monitoring, and governance.

Separating these layers allows independent scaling, simpler management, and high concurrency.

---

### What is a Snowflake account?
A Snowflake account is an isolated environment assigned to your organization. It contains:

- users and roles  
- databases and schemas  
- warehouses  
- billing and limits  
- security configurations  

Everything you create, query, or secure lives inside this account boundary.

---

### What is a region in Snowflake?
A region is the physical cloud location where Snowflake stores your data. Region choice affects latency, data residency laws, disaster recovery design, and data transfer pricing. Data remains in its region unless replicated intentionally.

---

### What is a database in Snowflake?
A database is a logical container grouping schemas and objects that belong to a business domain. Common examples include SALES, FINANCE, or ANALYTICS. It provides a top-level boundary for permissions and lifecycle.

---

### What is a schema?
A schema is a sub-container inside a database. It organizes related objects such as tables, views, stages, sequences, and functions. Schemas help structure environments clearly and control access at a granular level.

---

### What is a table?
A table stores business data. Although we see rows and columns, Snowflake internally stores tables in compressed columnar micro-partitions, which improves performance and storage efficiency.

---

### What are views?
A view is a query saved as an object. It does not store data itself but retrieves it from underlying tables. Views allow abstraction, security control, and standardized business logic.

---

### What are temporary tables?
Temporary tables exist only for the session or task that created them. They are used for transient calculations and automatically disappear, preventing clutter and cleanup work.

---

### What are transient tables?
Transient tables are persistent tables without Fail-safe recovery. They cost slightly less and are suitable for intermediate ETL data, but are not recommended for critical systems.

---

### What is a virtual warehouse?
A virtual warehouse is the compute engine that processes SQL. It runs queries, loads data, refreshes materialized views, and executes tasks. Warehouses can scale, pause, and resume independently.

---

### What is a stage?
A stage is a location used to load files into Snowflake or unload data out. It may be internal (inside Snowflake) or external (S3, Azure Blob, GCS). Stages provide structure, metadata tracking, and security around file operations.

---

### What is a file format?
A file format object defines how files are interpreted. It stores reusable parsing rules such as delimiters, encoding, compression type, and whether headers exist. This prevents repetition and errors.

---

### What is a role?
A role defines permissions. Privileges are assigned to roles, and users switch roles depending on what they need to do. This supports clean, auditable security management.

---

### What is a user?
A user is a login identity. Users are assigned roles, warehouses, MFA, and default session parameters such as timezone and language.

---

### What is session context?
Session context defines the environment in which SQL runs. It includes the active role, the current database and schema, and the warehouse. It controls what objects queries access.

---

### What is the services layer?
The services layer manages everything that coordinates the platform. It parses queries, applies optimization rules, enforces security, manages metadata, and handles transactions and logging automatically.

---

### What is the compute layer?
The compute layer consists of virtual warehouses. They process workloads but do not store data, allowing them to scale elastically.

---

### What is the storage layer?
The storage layer holds compressed, encrypted, column-organized data. Snowflake fully manages it, so administrators do not tune disks, partitions, or indexes.

---

### What are system views and information schema?
These are built-in metadata views that show details about objects, usage, security, and history. They are essential for auditing, troubleshooting, lineage analysis, and reporting.

---

### What tools connect to Snowflake?
Snowflake integrates with BI tools, ETL platforms, programming languages, and notebooks. Examples include Power BI, Tableau, Looker, dbt, Fivetran, Informatica, Python, JDBC/ODBC clients, Snowsight, and SnowSQL.

---

## 2. INTERMEDIATE — DATABASES, SCHEMAS, OBJECTS

### Difference between database and schema
A database is the top-level container. A schema is a folder within the database. Databases separate domains; schemas organize content within each domain.

---

### Why separate DEV, UAT, PROD?
Environment separation reduces production risk. It supports testing change safely, validating transformations, enforcing approvals, and maintaining clear audit trails.

---

### What is a fully qualified object name?
A fully qualified object name includes database, schema, and object:

DATABASE.SCHEMA.OBJECT

This prevents confusion when objects with the same name exist in multiple places.

---

### Why is naming convention important?
Consistent naming simplifies navigation, supports automation tools, reduces human mistakes, and makes environments far easier to maintain as systems grow.

---

### What is a permanent table?
A permanent table includes full data protection features. It supports Time Travel and Fail-safe and is suitable for critical business information.

---

### When use transient tables?
Use transient tables for data that is moderately important but recoverability risk is acceptable. Common in ETL staging or intermediate layers.

---

### When use temporary tables?
Use temporary tables for work that lives only during the transformation. They automatically clean themselves up after the session.

---

### What is a view and why use it?
A view sits between users and tables. It hides complexity, enforces rules, and protects sensitive fields. It allows consistent reporting logic without copying data.

---

### What is a secure view?
A secure view ensures that underlying data cannot be exposed through query history or metadata inspection. It is used for regulated data such as PII.

---

### What are materialized views?
Materialized views store the results of expensive queries. They refresh automatically when underlying data changes and significantly improve performance for repeated aggregations.

---

### What are sequences?
Sequences generate unique incremental numeric values. They are usually used for surrogate keys when natural identifiers are not reliable.

---

### What are stages inside databases versus schemas?
Schema stages belong to teams or projects and are easier to manage locally. Database stages are centralized and are usually used by shared ingestion frameworks.

---

### What is the purpose of INFORMATION_SCHEMA?
INFORMATION_SCHEMA exposes metadata queries, helping teams see which objects exist, how big they are, who uses them, and how they relate.

---

### How does Snowflake manage metadata?
Snowflake maintains metadata centrally in the services layer. It is transactional, strongly consistent, and automatically updated without manual registry maintenance.

---

### What happens when you drop a table?
The table is logically removed but not immediately destroyed. During the Time Travel window, it can be restored if needed.

---

### How does Time Travel affect dropped objects?
Time Travel allows tables and data to be restored to a previous point in time, enabling rollback from accidental changes or deletions.

---

### What is UNDROP?
UNDROP restores a dropped object while it is still within the Time Travel retention period.

---

### What is a clustering key?
A clustering key instructs Snowflake to group related rows physically. It is useful for very large historical datasets where queries repeatedly filter on specific columns.

---

### Why define clustering?
Clustering reduces the number of partitions scanned for repeated query patterns. This lowers compute cost and speeds up execution.

---

### What objects support Time Travel?
Permanent and transient tables generally support Time Travel. Temporary tables do not, and some objects have restrictions.

---

## 3. VIRTUAL WAREHOUSES (COMPUTE)

### What is a virtual warehouse used for?
A virtual warehouse executes all compute actions. It runs SQL queries, loads files, refreshes materialized views, performs transformations, and drives tasks.

---

### Difference between warehouse and storage
Warehouse performs computation; storage holds data. Shutting down a warehouse never deletes stored data.

---

### How are warehouse sizes defined?
Warehouse sizes are predefined from X-Small up to multi-X-Large. Each size has more CPU and memory and consumes more credits, but may process workloads faster.

---

### What is auto-suspend?
Auto-suspend pauses warehouses automatically after inactivity. This prevents idle compute from consuming cost.

---

### What is auto-resume?
Auto-resume wakes the warehouse automatically when the next query arrives. Users rarely notice the pause.

---

### What is warehouse scaling policy?
Scaling policy controls how Snowflake reacts to workload bursts. Conservative settings prefer queueing over spinning extra clusters, whereas aggressive options scale more readily.

---

### When scale up versus scale out?
Scale up when individual queries need more power. Scale out when many users run queries at the same time and you want to avoid queueing.

---

### What is a multi-cluster warehouse?
A warehouse that can automatically add and remove clusters during high concurrency. This eliminates waiting without having to resize constantly.

---

### How does concurrency affect warehouses?
Too many concurrent sessions cause queueing. Solutions include enabling multi-cluster, splitting workloads, or using additional warehouses.

---

### Can multiple warehouses query the same data?
Yes. Since storage is shared, any number of warehouses can read the same tables at the same time without conflict.

---

### Why separate ETL and BI warehouses?
ETL is compute-heavy and can disrupt dashboards. Separating compute ensures reporting remains responsive while ingestion continues independently.

---

### What happens when a warehouse is paused?
All compute billing stops. Queries either wait or trigger auto-resume depending on configuration.

---

### How to monitor warehouse usage?
Use built-in usage views and history functions to review load, queueing, and spend trends. These show where tuning is required.

---

### What are resource monitors?
Resource monitors track credit usage across warehouses or accounts. They can send alerts and suspend workloads automatically when budgets reach thresholds.

---

### Can warehouses be shared between accounts?
Warehouses themselves are not shared. Data can be shared, but each account controls its own compute resources.

---

## 4. STAGES AND FILE HANDLING

### What is a stage?
A stage is a holding area for files that are being loaded or unloaded. It acts as a managed gateway between file systems and Snowflake tables.

---

### What are internal stages?
Internal stages live inside Snowflake. They are secure, simple to manage, and ideal for controlled ingestion.

---

### What are external stages?
External stages reference S3, Azure Blob, or Google Cloud storage. They are used when working with data lakes or shared storage systems.

---

### What is a user stage?
Each user automatically receives a private stage. It is useful for personal testing and small imports.

---

### What is a table stage?
A stage dedicated to an individual table. Any file placed here is usually intended only for that table.

---

### What is a named internal stage?
A reusable stage object that teams or pipelines share. It simplifies multi-table ingestion workflows.

---

### Why use stages instead of loading directly?
Stages allow validation, error handling, control of permissions, repeatable ingestion pipelines, and easier troubleshooting. They formalize movement of data.

---

### What is COPY INTO?
COPY INTO loads files into Snowflake tables or unloads query results back to files. It is the primary data movement command.

---

### What file formats does Snowflake support?
Snowflake supports structured and semi-structured formats including CSV, JSON, Parquet, Avro, ORC, and XML.

---

### What is a file format object?
A file format object saves parsing instructions in one place. This ensures all future loads interpret files consistently.

---

### What is VALIDATION MODE?
Validation mode runs COPY INTO without writing to tables. It identifies bad rows and errors in advance.

---

### What is ON_ERROR?
ON_ERROR controls handling of problematic records. Possible actions include skipping bad rows, aborting the load, or continuing with warnings.

---

### How to handle duplicate file loads?
Snowflake records which files have been previously loaded and can ignore repeats using metadata such as load history and file tracking.

---

### What is metadata column FILE_NAME?
FILE_NAME exposes the originating file name inside queries. It assists in debugging, tracing, and deduplication strategies.

---

### How to unload data to files?
Use COPY INTO targeting a stage location. Query results become output files in internal or external storage.

---

## 5. SERVICES AND PLATFORM FEATURES

### What is Query History?
Query History shows which queries executed, who ran them, how long they took, and how much they cost. It is essential for operational monitoring.

---

### What is Query Profile?
Query Profile visualizes execution plans, showing where time and resources were spent. It helps diagnose slow or inefficient SQL.

---

### What are tasks?
Tasks schedule SQL to run automatically. They are used for pipelines, refresh jobs, incremental loads, and recurring operations.

---

### What are streams?
Streams track table changes over time. They enable incremental processing instead of re-loading full datasets.

---

### What is Snowpipe?
Snowpipe enables automated continuous ingestion. As soon as new files land in a stage, Snowflake loads them without manual triggers.

---

### What is data sharing?
Data sharing allows sharing data across accounts without copying it. Consumers read from the same physical data, reducing duplication and cost.

---

### What is a reader account?
A reader account gives external partners controlled access without requiring them to purchase their own Snowflake subscription.

---

### What is replication?
Replication copies databases, schemas, or accounts across regions or clouds for disaster recovery and global access.

---

### What is failover and failback?
Failover moves workloads to a replicated site during outages. Failback restores activity to the primary site after recovery.

---

### What is a masking policy?
A masking policy hides sensitive values such as phone numbers, emails, or credit cards depending on user role.

---

### What is a row access policy?
A row access policy restricts which rows a user may see based on rules, often related to country, department, or customer assignment.

---

### What are tags?
Tags attach metadata labels to objects for governance, classification, and cost allocation across departments.

---

### What is governance in Snowflake?
Governance means knowing who owns data, who can see it, how it is classified, and how it moves. Snowflake provides built-in tools to enforce these controls.

---

### What is Snowpark?
Snowpark allows developers to write Python, Java, or Scala code and execute it directly inside Snowflake, close to the data, reducing data movement.

---

### What are warehouse versus services responsibilities?
Warehouses perform compute. The services layer controls optimization, metadata, and governance. Understanding this split helps troubleshoot correctly.

---

## 6. SCENARIO BASED QUESTIONS

### Developer needs a sandbox copy of data
Use zero-copy cloning. It creates an instant logical copy without doubling storage.

---

### BI team complaints that ETL slows dashboards
Separate ETL and BI warehouses or enable multi-cluster to eliminate contention.

---

### Files arrive daily in S3 and must load automatically
Configure Snowpipe with an external stage and notifications to automate continuous ingestion.

---

### Accidental table drop
Use UNDROP or Time Travel to restore the table to a previous point.

---

### Need temporary data for transformations
Use temporary tables. They automatically disappear after session or task completion.

---

### Need persistent but cheaper storage
Use transient tables. They save cost but remove Fail-safe protection.

---

### Duplicate file loaded twice
Use metadata tracking so Snowflake ignores already-loaded files based on history and file name.

---

### Data scientists need JSON logs
Store them in VARIANT columns. Snowflake can query structured and semi-structured data together.

---

### Multiple users cause query queueing
Enable multi-cluster, scale the warehouse, or split workloads by team.

---

### External partner needs read-only access
Use secure data sharing or a reader account to provide controlled access without copies.

---

### Historical table queries are slow
Define an appropriate clustering key to improve pruning on large datasets.

---

### Need DEV, UAT, and PROD quickly
Use cloning to create fast environment copies without duplicating data.

---

### Warehouse runs idle all day
Configure auto-suspend, auto-resume, and resource monitors to control waste.

---

### Need audit history of queries
Use Account Usage and Query History views to review activities.

---

### Need to mask PII fields
Apply masking policies combined with role-based access control.

---

## 7. TRICK AND MISCONCEPTION QUESTIONS

### Does stopping a warehouse delete data?
No. Pausing only stops compute charges. Data stays stored independently.

---

### Are indexes required?
No. Snowflake uses micro-partitions and metadata pruning instead of traditional indexing.

---

### Do materialized views always improve performance?
Only when queries repeatedly use the same heavy aggregation. They add cost and maintenance overhead.

---

### Is Time Travel the same for all table types?
No. Temporary tables do not support it, and retention differs between table types.

---

### Do transient tables support Fail-safe?
No. They intentionally remove Fail-safe to reduce cost.

---

### Does Snowflake always block duplicate file loads?
No. It can detect duplicates if configured, but the pattern must be designed properly.

---

### Does Snowflake charge for paused warehouses?
Compute charges stop, but storage and other services still incur costs.

---

### Are stages only for loading data?
No. Stages are also used for unloading and managing file pipelines.

---

### Does cloning copy physical data?
Initially no. It is logical. As changes occur, new storage gradually accumulates.

---

### Can we change warehouses while a query runs?
No. A query uses the warehouse that started it.

---

### Are views faster than tables?
Views themselves are not faster. Performance depends on the underlying query logic.

---

### Does every object support UNDROP?
No. Some objects and temporary structures cannot be restored.

---

### Are tasks the same as cron jobs?
They schedule execution, but are integrated with Snowflake permissions, lineage, and recovery.

---

### Does every file require COPY INTO?
No. Some pipelines such as Snowpipe handle ingestion automatically.

---

### Can Snowflake replace ETL tools entirely?
Snowflake handles transformation extremely well, but orchestration, data quality, lineage, and scheduling often still require dedicated tools.

---

# End of File
