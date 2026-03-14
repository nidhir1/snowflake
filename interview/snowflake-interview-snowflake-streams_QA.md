# ❄️ Snowflake Streams --- CDC, Auditing & Data Pipelines





# 🔹 1️⃣ WORKING WITH SNOWFLAKE STREAMS

## A. BASICS --- WHAT IS A STREAM?

### Ques: What is a Snowflake Stream?

**Ans:**\
A **Snowflake Stream** is a special object that tracks changes made to a
table.

It does **not store data itself**, but records **which rows were
inserted, updated, or deleted** since the last time the stream was
consumed.

Streams enable **Change Data Capture (CDC)** inside Snowflake.

Example pipeline:

Source Table → Stream → Task → Target Table

This allows incremental processing instead of full reloads.

------------------------------------------------------------------------

### Ques: Why do we need Streams?

**Ans:**

In data pipelines we often need to process **only the changes** since
the last run.

Without Streams:

-   ETL pipelines must reprocess entire tables
-   This wastes compute and time

Streams allow pipelines to read **only newly changed rows**.

Benefits:

-   incremental processing
-   faster ETL
-   lower compute cost
-   easier data synchronization

------------------------------------------------------------------------

### Ques: What problem do Streams solve in ETL pipelines?

**Ans:**

Traditional ETL pipelines often perform **full refreshes**.

Example:

    SELECT * FROM source_table

Even if only 10 rows changed in a 10 million row table.

Streams solve this by tracking:

-   inserts
-   updates
-   deletes

So pipelines process **only the changed rows**.

------------------------------------------------------------------------

### Ques: Are Streams physical data copies?

**Ans:**

No.

Streams do **not store copies of data**.

Instead they store **metadata pointers to changed rows**.

This makes Streams lightweight and efficient.

------------------------------------------------------------------------

### Ques: How do Streams track table changes?

**Ans:**

Snowflake tracks changes using **table version metadata**.

When rows change:

-   Snowflake records row metadata
-   Stream exposes those changes as rows
-   ETL processes them

Streams internally rely on **Time Travel metadata**.

------------------------------------------------------------------------

### Ques: What is CDC (Change Data Capture)?

**Ans:**

CDC means capturing **changes made to data over time**.

These changes include:

-   INSERT
-   UPDATE
-   DELETE

Streams are Snowflake's native CDC mechanism.

------------------------------------------------------------------------

### Ques: What operations can Streams capture?

**Ans:**

Streams capture:

-   INSERT
-   UPDATE
-   DELETE

Updates appear as:

-   DELETE of old row
-   INSERT of new row

This helps downstream systems reconstruct changes.

------------------------------------------------------------------------

### Ques: Do Streams modify underlying tables?

**Ans:**

No.

Streams are **read-only tracking objects**.

They do not modify the source table.

------------------------------------------------------------------------

### Ques: How often can a Stream be queried?

**Ans:**

Streams can be queried anytime.

However, when consumed inside transactions (such as ETL tasks), the
**offset advances** so that already processed rows are not returned
again.

------------------------------------------------------------------------

### Ques: What privileges are needed to create streams?

**Ans:**

To create a stream, a role typically needs:

-   USAGE on database
-   USAGE on schema
-   SELECT privilege on source table
-   CREATE STREAM privilege on schema

------------------------------------------------------------------------

# 🔹 B. STREAM OBJECT & DML AUDITING

### Ques: What metadata does a Stream store?

**Ans:**

Streams expose metadata columns describing changes.

These columns help identify:

-   operation type
-   update events
-   row identity

------------------------------------------------------------------------

### Ques: What are the metadata columns (METADATA\$)?

**Ans:**

Streams provide special columns:

METADATA$ACTION
METADATA$ISUPDATE\
METADATA\$ROW_ID

These columns describe the type of change captured.

------------------------------------------------------------------------

### Ques: What does METADATA\$ACTION represent?

**Ans:**

METADATA\$ACTION indicates the operation type.

Possible values:

INSERT\
DELETE

Updates appear as a combination of DELETE + INSERT.

------------------------------------------------------------------------

### Ques: What does METADATA\$ISUPDATE show?

**Ans:**

This column indicates whether a row change is part of an update.

TRUE → row is part of update event\
FALSE → pure insert or delete

------------------------------------------------------------------------

### Ques: What is METADATA\$ROW_ID?

**Ans:**

ROW_ID uniquely identifies a row change event.

It helps Snowflake maintain ordering and consistency of change records.

------------------------------------------------------------------------

### Ques: How do Streams help with DML auditing?

**Ans:**

Streams allow organizations to track:

-   which rows changed
-   what operations occurred

This can support:

-   ETL pipelines
-   audit trails
-   replication pipelines

------------------------------------------------------------------------

### Ques: Can Streams detect TRUNCATE operations?

**Ans:**

No.

TRUNCATE removes all rows instantly without generating individual delete
events.

Streams cannot capture TRUNCATE operations.

------------------------------------------------------------------------

### Ques: Do Streams track historical changes forever?

**Ans:**

No.

Streams depend on **Time Travel retention**.

Once retention expires, older changes may no longer be accessible.

------------------------------------------------------------------------

### Ques: What happens when data is consumed from a Stream?

**Ans:**

When a stream is read inside a transaction:

-   the stream offset advances
-   those changes are marked as consumed

Future queries will only show new changes.

------------------------------------------------------------------------

### Ques: Why is consumption marker important?

**Ans:**

The consumption marker ensures:

-   changes are processed once
-   pipelines remain idempotent
-   duplicate processing is avoided

------------------------------------------------------------------------

# 🔹 C. STREAM TYPES

## ⭐ Standard Stream

### Ques: What is a Standard Stream?

**Ans:**

Standard streams track **INSERT, UPDATE, and DELETE operations**.

They provide full change tracking for general-purpose ETL pipelines.

------------------------------------------------------------------------

### Ques: When should we use Standard Streams?

**Ans:**

Use standard streams when pipelines must process:

-   inserts
-   updates
-   deletes

Example:

Synchronizing transactional tables.

------------------------------------------------------------------------

## ⭐ Append-Only Stream

### Ques: What is an Append-Only Stream?

**Ans:**

Append-only streams track **only inserted rows**.

They ignore updates and deletes.

------------------------------------------------------------------------

### Ques: When is Append-Only useful?

**Ans:**

Append-only streams work well when data is **never modified**.

Example:

log tables\
event tracking\
clickstream data

------------------------------------------------------------------------

## ⭐ Insert-Only Stream

### Ques: What is Insert-Only Stream?

**Ans:**

Insert-only streams track **only insert operations** and are optimized
for external tables.

They are commonly used in ingestion pipelines.

------------------------------------------------------------------------

### Ques: Why are lighter-weight Streams sometimes better?

**Ans:**

Insert-only or append-only streams:

-   reduce metadata overhead
-   improve performance
-   simplify pipelines

Use them when deletes and updates are irrelevant.

------------------------------------------------------------------------

# 🔹 D. DATA FLOW WITH STREAMS

### Ques: What is source table?

**Ans:**

The source table is the table being monitored by the stream.

Example:

    CREATE STREAM orders_stream ON TABLE orders;

------------------------------------------------------------------------

### Ques: What is target table?

**Ans:**

The target table stores processed or transformed data produced by ETL
pipelines.

Example:

orders_stream → transform → orders_analytics

------------------------------------------------------------------------

### Ques: How do Streams interact with tasks?

**Ans:**

Tasks schedule automated queries that read streams.

Example pipeline:

Table → Stream → Task → Target Table

This enables automated incremental processing.

------------------------------------------------------------------------

### Ques: What is offset checkpoint?

**Ans:**

Offset checkpoint indicates the **last processed change**.

Streams only return rows **after the checkpoint**.

------------------------------------------------------------------------

### Ques: Can one table have multiple streams?

**Ans:**

Yes.

Multiple streams can track the same table independently for different
pipelines.

------------------------------------------------------------------------

### Ques: Can one stream feed multiple consumers?

**Ans:**

Usually not recommended.

Once a stream is consumed, offsets move forward.

Separate streams should be created for independent consumers.

------------------------------------------------------------------------

# 🔹 2️⃣ TIME TRAVEL WITH STREAMS

### Ques: What role does Time Travel play with Streams?

**Ans:**

Streams rely on Time Travel metadata to detect changes between table
versions.

Without Time Travel retention, change tracking cannot function properly.

------------------------------------------------------------------------

### Ques: What is stale stream error?

**Ans:**

A stale stream occurs when:

-   retention period expires
-   stream offset falls outside available table history

The stream must be recreated.

------------------------------------------------------------------------

# 🔹 3️⃣ SCENARIO QUESTIONS

### Ques: Need incremental load from source to target --- which feature helps?

**Ans:**

Use **Snowflake Streams** combined with **Tasks**.

This enables incremental CDC pipelines.

------------------------------------------------------------------------

### Ques: Need real-time CDC --- task + stream or full refresh?

**Ans:**

Use **Streams + Tasks** for incremental processing.

Full refresh is inefficient for large tables.

------------------------------------------------------------------------

# 🔹 4️⃣ TRICK QUESTIONS

### Ques: Do Streams store real copies of changed rows?

**Ans:**

No.

Streams store metadata references, not physical data copies.

------------------------------------------------------------------------

### Ques: Do Streams keep data forever?

**Ans:**

No.

They depend on Time Travel retention.

------------------------------------------------------------------------

### Ques: Can Streams be queried without warehouse?

**Ans:**

No.

Querying streams requires compute provided by a warehouse.

