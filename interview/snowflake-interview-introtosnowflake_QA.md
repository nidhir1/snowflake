# INTRODUCTION TO SNOWFLAKE — IMPROVED DETAILED ANSWERS





### ✅ What is auto-suspend?
Auto-suspend automatically pauses a virtual warehouse when there is no activity
for a configured period.

It prevents paying for compute while nothing is running — which is important
because most cost wastage in Snowflake happens when warehouses are left running
unnecessarily.

---

### ✅ What is auto-resume?
Auto-resume automatically starts a warehouse when a new query arrives.

Developers and analysts don’t need to think about starting compute — Snowflake
wakes it up in seconds — making cost optimization practical without hurting
productivity.

---

### ✅ Why are warehouses billed separately from storage?
Snowflake treats compute and storage as completely independent services:

- Storage = cost of keeping data available at all times
- Compute = cost only when queries or loads are running

This model allows storing massive amounts of data cheaply while paying for
compute only when actually needed.

---

### ✅ What happens if you stop a warehouse?
Query execution stops, but **your data is unaffected**.

Storage remains active, available, and billed separately. You can restart the
warehouse anytime and continue working immediately — nothing is lost.

---

### ✅ What is result caching?
Snowflake stores the final output of previously executed queries.

If the same user runs the same query on unchanged data, Snowflake returns the
answer instantly from cache — with zero re-computation.

This improves speed and reduces cost for repeated analytics.

---

### ✅ What is data caching?
Data caching keeps recently accessed micro-partitions in local compute memory.

When another query needs the same blocks, Snowflake reads from cache instead of
pulling from cloud storage — significantly reducing latency and compute work.

---

### ✅ What is metadata caching?
Metadata includes partition ranges, statistics, column details, and query
history.

Snowflake caches this globally so query planning becomes extremely fast. It
already “knows” where relevant data lives instead of scanning everything again.

---

### ✅ What are stages in Snowflake?
A stage is a **landing area** where raw files exist before being loaded into
tables.

Stages control how files are secured, tracked, validated, and ingested — acting
as a controlled bridge between external storage and Snowflake tables.

---

### ✅ Internal vs External stage
**Internal stage**
Snowflake stores files internally and manages security automatically.

**External stage**
Snowflake references files stored in your cloud storage (S3, Azure Blob, GCS).
You manage permissions, lifecycle, and governance.

Choice depends on cost, security rules, and ownership policies.

---

### ✅ What is COPY INTO?
`COPY INTO` is Snowflake’s high-performance bulk-loading command.

It:

- reads files from stages
- validates records
- loads in parallel
- tracks what has already been processed
- handles rejected records

This command forms the backbone of most ingestion pipelines.

---

### ✅ What is Fail-safe (expanded)?
Fail-safe is Snowflake’s final recovery window beyond Time Travel.

It exists only for rare disaster scenarios such as infrastructure failure or
corruption. Customers cannot trigger Fail-safe themselves — it requires
Snowflake Support, is time-limited, and is not meant for day-to-day restores.

Think of it as: **“break glass only.”**

---

### ✅ What is Zero-Copy Cloning?
Zero-Copy Cloning creates instant logical copies of databases, schemas, or
tables without duplicating storage.

Snowflake references the same underlying data until changes occur, making it:

- fast
- safe
- storage-efficient

Perfect for dev/test environments, experimentation, and backups before risky
changes.

---

### ✅ What is Time Travel?
Time Travel allows querying or restoring data as it existed at a specific time
in the past.

It helps recover:

- accidental deletes
- corrupted loads
- overwrites
- wrong updates

It is essential for debugging, auditing, and safety.

---

### ✅ What is workload isolation?
Workload isolation ensures heavy processes never slow down user reporting.

By assigning separate warehouses to ETL, BI, and Data Science, each team scales
independently while sharing the same data — no blocking or fighting for
resources.

---

### ✅ What is Snowpipe?
Snowpipe is Snowflake’s continuous ingestion service.

It automatically loads data as soon as new files arrive in cloud storage,
allowing near-real-time data pipelines without manual scheduling.

---

### ✅ What are streams?
Streams track row-level changes — inserts, updates, deletes — since the last
checkpoint.

They are essential for:

- CDC pipelines
- incremental loading
- change auditing
- replaying operations safely

---

### ✅ What are tasks?
Tasks let Snowflake run scheduled SQL operations internally.

Instead of external cron jobs, Snowflake can automate:

- transformations
- rollups
- cleansing jobs
- CDC processing

Governed, audit-friendly, and centralized.

---

### ✅ What is Snowpark?
Snowpark lets developers run Python, Java, or Scala directly inside Snowflake’s
compute engine — instead of exporting data to external clusters.

This reduces data movement, improves governance, and simplifies advanced data
engineering and ML workflows.

