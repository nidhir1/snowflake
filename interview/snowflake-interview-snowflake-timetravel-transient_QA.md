# Snowflake --- Time Travel, Fail-safe and Transient Tables


------------------------------------------------------------------------

## 1. TIME TRAVEL FEATURE IN SNOWFLAKE

### A. BASICS --- WHAT TIME TRAVEL IS

### Ques: What is Time Travel in Snowflake?

**Ans:**\
Time Travel is a Snowflake feature that allows users to access
historical versions of data. It lets you query, restore, or recreate
tables as they existed at a specific point in the past. Instead of
restoring from traditional backups, Snowflake maintains historical
versions of objects internally using metadata and storage snapshots.
This makes recovery extremely fast and allows teams to fix data mistakes
without downtime.

### Ques: Why do we need Time Travel?

**Ans:**\
In real-world environments, data errors happen frequently---such as
accidental deletes, incorrect updates, dropped tables, or faulty ETL
jobs. Time Travel provides a safety net that allows teams to quickly
recover data to a previous state. Without it, organizations would need
full database restores, which are slow and disruptive.

### Ques: What kinds of operations does Time Travel help recover from?

**Ans:**\
Time Travel can recover from destructive operations such as: - DELETE
statements that remove rows - UPDATE statements that overwrite values -
TRUNCATE operations that remove all rows - Dropped tables or schemas -
ETL scripts that accidentally overwrite data

As long as the operation happened within the retention window, the data
can be restored.

### Ques: What objects support Time Travel?

**Ans:**\
Time Travel supports several Snowflake objects including: - Permanent
tables - Transient tables (with limited protection) - Databases -
Schemas - Materialized views

Temporary tables generally do not support long-term Time Travel because
they are designed for short-lived sessions.

### Ques: What is retention period in Time Travel?

**Ans:**\
The retention period defines how long Snowflake keeps historical data
versions available. During this time window, users can query past states
or restore objects. Once the retention period expires, the historical
versions are permanently removed.

### Ques: What is the default retention for permanent tables?

**Ans:**\
The default retention period for permanent tables is **1 day (24
hours)**. However, depending on the Snowflake edition, administrators
can increase the retention period---sometimes up to 90 days in
enterprise environments.

### Ques: Can the retention period be changed?

**Ans:**\
Yes. Retention can be configured at the **database, schema, or table
level** using Snowflake parameters. Increasing retention improves
recovery capability but also increases storage costs because Snowflake
must store more historical versions.

### Ques: Do schemas and databases support Time Travel?

**Ans:**\
Yes. Time Travel can restore entire schemas or databases that were
dropped accidentally. As long as the drop occurred within the retention
period, the objects can be recovered using the UNDROP command.

### Ques: Does Time Travel store duplicate copies of data?

**Ans:**\
No. Snowflake does not store full duplicate copies of tables. Instead,
it stores metadata and incremental changes (deltas). When users query
historical data, Snowflake reconstructs the table state dynamically
using these deltas.

### Ques: Where does Time Travel metadata live?

**Ans:**\
All Time Travel metadata is stored internally in Snowflake's storage
layer and services layer. Users do not manage this data directly; they
simply query historical states using SQL commands.

------------------------------------------------------------------------

## B. DML OPERATIONS & SILENT AUDITS

### Ques: What are DML operations in Snowflake?

**Ans:**\
DML stands for **Data Manipulation Language** and refers to SQL
statements that modify data. Common DML operations include INSERT,
UPDATE, DELETE, MERGE, and TRUNCATE.

### Ques: Which DML operations are tracked for Time Travel?

**Ans:**\
Snowflake tracks most data modifications including INSERT, UPDATE,
DELETE, MERGE, and TRUNCATE. These operations generate historical
versions that can be accessed during the retention window.

### Ques: What does "silent audit" mean?

**Ans:**\
Silent audit means Snowflake automatically records historical changes
without requiring explicit audit tables, triggers, or logs. This makes
historical recovery and debugging possible without additional
configuration.

### Ques: How does Snowflake internally track history?

**Ans:**\
Snowflake records metadata snapshots and change logs. When users query a
past state, Snowflake reconstructs the table version using these
metadata snapshots and deltas.

### Ques: Is Time Travel manual or automatic?

**Ans:**\
Time Travel is automatic. As soon as objects are created, Snowflake
begins tracking historical versions as long as retention is enabled.

### Ques: Does Time Travel work for INSERT, UPDATE, DELETE?

**Ans:**\
Yes. Time Travel captures changes from all these operations, allowing
users to revert or inspect previous versions.

### Ques: Does TRUNCATE support Time Travel recovery?

**Ans:**\
Yes. Even though TRUNCATE removes all rows instantly, Snowflake
preserves the previous version of the table during the retention window.

### Ques: Does DROP TABLE support Time Travel?

**Ans:**\
Yes. Dropped tables can be restored using the UNDROP command if they are
still within the retention period.

### Ques: What happens when table structure changes?

**Ans:**\
Snowflake stores schema metadata as well. When a table is restored to a
previous point, both its structure and data revert to that historical
state.

### Ques: Can we query historical versions without restoring?

**Ans:**\
Yes. Users can query historical snapshots using Time Travel clauses
without restoring the table.

------------------------------------------------------------------------

## C. CONTINUOUS DATA PROTECTION (CDP)

### Ques: What is Continuous Data Protection?

**Ans:**\
Continuous Data Protection (CDP) means Snowflake continuously records
data changes so that past versions are always available within the
retention window.

### Ques: How does CDP relate to Time Travel?

**Ans:**\
Time Travel is the mechanism that implements CDP in Snowflake by
allowing users to query historical versions of data.

### Ques: How does Time Travel differ from Fail-safe?

**Ans:**\
Time Travel is user-accessible and fast. Fail-safe is a last-resort
recovery mechanism managed by Snowflake support after Time Travel
expires.

### Ques: Is CDP the same as backup?

**Ans:**\
No. CDP allows querying historical states instantly. Backups typically
require restoring entire systems.

### Ques: Why is CDP better than manual backups?

**Ans:**\
CDP provides faster recovery, continuous history tracking, and
eliminates the need for manual backup schedules.

### Ques: What are retention tradeoffs?

**Ans:**\
Longer retention increases safety but also increases storage cost
because Snowflake stores more historical versions.

### Ques: How long can enterprise plans retain Time Travel data?

**Ans:**\
Enterprise editions can support retention up to **90 days**.

### Ques: How does retention affect storage billing?

**Ans:**\
Longer retention increases storage usage because Snowflake stores
additional historical metadata and data changes.

### Ques: What happens when retention expires?

**Ans:**\
Once retention expires, historical versions are permanently removed and
cannot be accessed through Time Travel.

### Ques: What risks exist if retention is too short?

**Ans:**\
Mistakes discovered after the retention window cannot be recovered using
Time Travel.

------------------------------------------------------------------------

## 2. USING TIME TRAVEL (PRACTICAL)

### Ques: What is offset-based Time Travel?

**Ans:**\
Offset-based recovery allows querying a table as it existed a certain
amount of time ago (for example, 5 minutes ago).

### Ques: What units are allowed in offset?

**Ans:**\
Offsets can be specified in seconds, minutes, hours, or days.

### Ques: When would you use offset instead of timestamp?

**Ans:**\
Offset is useful when you know approximately when the error happened but
not the exact timestamp.

### Ques: What is Query ID--based recovery?

**Ans:**\
This method restores data to the state just before a specific SQL query
executed.

### Ques: Where can Query IDs be found?

**Ans:**\
Query IDs appear in Snowflake query history, UI history tab, and
ACCOUNT_USAGE views.

------------------------------------------------------------------------

## C. FAIL-SAFE & UNDROP

### Ques: What is Fail-safe in Snowflake?

**Ans:**\
Fail-safe is a 7-day emergency recovery window after Time Travel
expires. It is designed for disaster recovery scenarios.

### Ques: Who controls Fail-safe?

**Ans:**\
Fail-safe is controlled entirely by Snowflake support engineers.

### Ques: Can users query Fail-safe data?

**Ans:**\
No. Fail-safe cannot be queried directly by users.

### Ques: What is UNDROP used for?

**Ans:**\
UNDROP restores dropped objects such as tables, schemas, or databases
within the Time Travel retention window.

------------------------------------------------------------------------

## 3. TRANSIENT TABLES

### Ques: What is a transient table?

**Ans:**\
A transient table is a persistent Snowflake table designed for temporary
or intermediate workloads but without Fail-safe protection.

### Ques: How is a transient table different from a permanent table?

**Ans:**\
Permanent tables include Fail-safe and longer recovery protections.
Transient tables remove Fail-safe to reduce storage cost.

### Ques: Do transient tables support Time Travel?

**Ans:**\
Yes, but typically with shorter retention windows.

### Ques: Do transient tables support Fail-safe?

**Ans:**\
No. Once Time Travel expires, transient table data cannot be recovered.

### Ques: Why are transient tables cheaper?

**Ans:**\
Because they skip Fail-safe storage and long-term durability layers.

### Ques: When should transient tables be used?

**Ans:**\
They are ideal for staging tables, ETL intermediate steps, scratch
datasets, and temporary transformations.

------------------------------------------------------------------------

## 4. SCENARIO-BASED QUESTIONS

### Ques: Developer accidentally deleted rows --- what do you do?

**Ans:**\
Use Time Travel to query the table state before the deletion and restore
the missing rows.

### Ques: A TRUNCATE wiped a table --- what is the recovery path?

**Ans:**\
Clone the table from a timestamp before the truncate event and restore
the data.

### Ques: You dropped a schema by mistake --- what options exist?

**Ans:**\
Use UNDROP if still within Time Travel retention. Otherwise, contact
Snowflake support for Fail-safe recovery.

------------------------------------------------------------------------

## 5. TRICK / MISCONCEPTION QUESTIONS

### Ques: Is Time Travel a replacement for backups?

**Ans:**\
No. Time Travel protects against short-term mistakes but does not
replace long-term backup strategies.

### Ques: Does Time Travel slow down queries?

**Ans:**\
No. Normal queries run on current data. Historical reconstruction
happens only when explicitly requested.

### Ques: Is Time Travel available on all Snowflake editions?

**Ans:**\
Yes, but retention limits vary depending on the Snowflake edition.
