❄️ Snowflake — Tasks, Partitions, Stages & Bulk Loading
Complete Q&A — Medium-Detail, Teaching Style
🔹 1️⃣ SNOWFLAKE TASKS & PARTITIONS (TASK AUTOMATION)
A. TASKS — BASICS
✅ What is a Snowflake Task?

A task is a Snowflake object that automatically executes SQL on a schedule or based on dependency.
You can think of it as Snowflake’s built-in orchestration engine for pipelines.

Tasks are commonly used for:

incremental loads

CDC processing

cleanup and maintenance jobs

transformation pipelines

✅ Why do we use tasks?

Tasks remove manual execution and make data operations:

automated

repeatable

predictable

Instead of relying on cron jobs, scripts, or people, Snowflake runs pipelines for you.

✅ What kind of SQL statements can tasks run?

Tasks can execute:

single SQL statements

multi-statement scripts

stored procedures

MERGE / INSERT / UPDATE ETL logic

They primarily support DML + procedural logic (not administrative server operations).

✅ How do tasks help automate ETL?

Tasks allow ETL steps to run in order:

Load raw data

Transform data

Publish to reporting tables

All without external schedulers.

✅ Do tasks require a warehouse?

Yes — unless the task is configured as serverless.

If using a warehouse:

task resumes the warehouse

runs the query

warehouse suspends if auto-suspend is enabled

✅ What happens if the warehouse is suspended?

If the task has permission to resume the warehouse → it resumes automatically.
If not → the task fails and appears in task history.

✅ What is a task owner?

The role that created the task.

Tasks execute using the owner’s privileges — meaning RBAC must be designed carefully so tasks don’t run with excessive permissions.

B. REAL-TIME USES OF TASKS
✅ What is scheduled ETL using tasks?

Tasks run recurring pipelines such as:

nightly batch loads

hourly refresh jobs

continuous CDC processing

All inside Snowflake.

✅ How do tasks integrate with streams?

Classic CDC pattern:

Data lands in staging

Stream records incremental changes

Task processes only new changes

This ensures controlled, incremental data loads.

✅ Can tasks trigger incrementally?

Yes — when combined with streams.
They read ONLY new changes since the previous run.

✅ How do tasks ensure exactly-once processing?

The stream consumption pointer moves only after successful completion.

If task fails → the changes remain in the stream and can retry safely.

✅ What is task history?

Snowflake logs each run:

start time

status

duration

SQL executed

error messages

Task history is essential for troubleshooting and auditing.

✅ How do you identify failed tasks?

View in UI or query metadata.
Failed entries show error codes and failure reasons.

✅ What happens when task execution overlaps?

Snowflake prevents overlap automatically:

if previous execution is still running, the next one waits or skips

No accidental double-runs.

✅ Can tasks be paused?

Yes — tasks can be suspended when you don’t want pipelines running temporarily.

C. DAG (DIRECTED ACYCLIC GRAPH)
✅ What is a DAG?

A DAG is an execution graph where tasks run in defined order without loops.

Parent → Child → Next Step

✅ Why do tasks use DAG dependency?

To enforce execution order and ensure downstream jobs run only when upstream jobs complete.

✅ What is a parent task?

A task that triggers another task after it finishes.

✅ What is a child task?

A task that runs only when its parent completes successfully.

✅ What happens when the parent fails?

Child tasks do not run.
This prevents bad or partial data propagation.

✅ Why are circular dependencies not allowed?

Circular dependency = infinite execution loop.
Snowflake blocks this design.

✅ How do dependent tasks pass execution?

Children do not need schedules — they trigger automatically after their parent finishes.

✅ How do you visualize task DAGs?

Using:

Snowflake UI task graph

metadata views

Both show relationships and execution flow.

D. TASK SCHEDULES & RESUME
✅ What scheduling options exist?

Tasks support:

CRON scheduling

fixed intervals

dependency-based execution

✅ What is CRON scheduling?

CRON allows flexible timing such as:

“Run every weekday at 3 AM.”

✅ What is time-based scheduling?

Simple intervals such as:

every 5 minutes

every hour

once per day

✅ Can tasks run every minute?

Yes — one minute is the minimum frequency.

✅ What is automatic resume?

Warehouse wakes up when task runs and suspends after processing (if configured).

✅ What happens if the system is down?

Runs may skip or be delayed depending on configuration — Snowflake does not rerun indefinitely.

✅ Who can enable/disable tasks?

Only roles that own the task or have admin privileges.

🔹 2️⃣ PARTITIONING — MICRO-PARTITIONS & CLUSTERING
A. MICRO-PARTITIONS — BASICS
✅ What are micro-partitions?

Snowflake automatically stores table data in small immutable blocks called micro-partitions.

✅ Typical micro-partition size?

Usually 50–500 MB compressed.

✅ How are rows assigned?

Snowflake handles it automatically — you do not define partitions manually.

✅ Does Snowflake require manual partitioning?

No — partitioning is fully automatic.

✅ What metadata is stored?

Each micro-partition contains:

min/max column values

distinct value counts

null counts

partition lineage

stats for pruning

✅ What is pruning?

Pruning skips reading partitions that are not relevant to the query.

Example:

WHERE order_date = '2024-01-01'


Partitions whose dates don’t match get skipped.

✅ Why does pruning improve performance?

Because:

fewer partitions scanned

less compute time

faster query execution

✅ How does CDC affect partitions?

Updates/deletes create new partitions, because partitions are immutable.
Old partitions remain for Time Travel.

B. CLUSTERING
✅ What is a cluster key?

A logical key that organizes rows so similar values sit closer together — improving pruning.

✅ Cluster key vs. partitioning?

Partitioning = automatic storage layout
Clustering = optional performance tuning

✅ When do we need clustering?

When tables are:

very large

frequently filtered on the same columns

suffering from excessive scanning

✅ What is reclustering?

Rearranging micro-partitions to improve clustering quality.

✅ What is automatic clustering?

Snowflake automatically reclusters table storage without manual scripts.

✅ Does clustering cost credits?

Yes — reclustering consumes compute.

✅ When should we NOT use clustering?

Avoid clustering on:

small tables

random insert/update patterns

non-selective columns

It will increase cost without improving speed.

✅ How does clustering help heavy scan workloads?

It dramatically increases pruning, meaning fewer partitions are scanned.

✅ What indicates clustering problems?

Queries scan large percentages of table even when filters exist.

✅ How to monitor clustering?

Use Snowflake system views to check clustering depth and efficiency.

C. INTERNAL PARTITION TYPES & USAGE
✅ What internal partition structures exist?

Micro-partitions are organized in internal trees so Snowflake can navigate quickly when scanning.

✅ How does Snowflake manage partitions as tables grow?

Snowflake continuously:

merges

splits

reorganizes

to keep storage balanced.

✅ How does Time Travel interact?

Older partition versions are kept temporarily and removed when retention expires.

✅ How do deletes/updates create overhead?

Because old partitions remain and new ones are created — temporarily increasing storage.

✅ Why do poorly designed keys increase cost?

Bad clustering prevents pruning → queries scan much more data → cost rises.

🔹 3️⃣ STAGES & BULK LOADING
A. STAGES — BASICS
✅ What is a stage?

A staging area where files are placed before loading into tables.

✅ Why use stages instead of loading directly?

Stages separate:

file ingestion

table loading

This makes loads reliable, resumable, and scalable.

✅ Types of stages

internal stages

external stages

✅ What is a user stage?

Private stage for one Snowflake user.

✅ What is a table stage?

Stage dedicated to a single table.

✅ What is a named stage?

Reusable, explicitly created stage — often shared across ETL processes.

✅ Which stage is used when uploading via GUI?

Usually the user stage.

B. FILE FORMATS & COPY COMMAND
✅ What is a file format object?

Reusable definition describing how Snowflake interprets files (delimiters, compression, JSON rules, etc.).

✅ Supported formats?

CSV, JSON, Parquet, Avro, ORC and more.

✅ Why define reusable file formats?

So COPY commands reuse configuration instead of redefining parsing each time.

✅ What is COPY INTO table?

Command that loads staged files into tables — automatically parallelized and fault-tolerant.

✅ What does ON_ERROR do?

Controls behavior for bad rows:

skip

fail

continue logging errors

✅ What is VALIDATION MODE?

Simulates the load without writing data — useful for safety testing.

✅ How do we avoid duplicate loads?

Track metadata columns and persist file tracking records.

✅ What are metadata columns?

Snowflake-generated values including:

$FILE

$ROW

$LOAD_TIME

✅ How do we load semi-structured data?

Load into VARIANT column using JSON or Parquet COPY options.

C. BULK LOAD WORKFLOWS
✅ What is bulk loading?

Efficiently ingesting large datasets in batches.

✅ Why do small files hurt performance?

Too many small files increase:

metadata overhead

parallel job coordination

Leading to slower loads.

✅ Why compressed files?

They transfer faster and reduce storage cost.

✅ What is parallel loading?

COPY automatically splits loading across multiple threads and warehouse resources.

✅ How does COPY scale?

Larger warehouses process more files in parallel — horizontally scaling throughput.

✅ How do we monitor loads?

Use load history views and query logs for performance, failures, and file diagnostics.

✅ What is unloading?

Exporting Snowflake tables back into staged files.

🔹 4️⃣ SCENARIOS — REAL THINKING

⭐ Scenario 1 — Need to automate CDC ingestion

Problem

The business wants fresh data continuously.
Full reload every day is slow and expensive.

Thinking

We only want new or changed rows.

CDC must be:

incremental

reliable

idempotent (no duplicates)

Solution

👉 Use Streams + Tasks + MERGE

Flow

1️⃣ Data lands in staging
2️⃣ Stream tracks changes
3️⃣ Task wakes up every X minutes
4️⃣ Task processes stream changes using MERGE

Why this is correct

✔ exactly-once processing
✔ safe retry if job fails
✔ minimal compute cost

⭐ Scenario 2 — ETL should run only after upstream load finishes

Problem

Two pipelines depend on each other.
ETL should NOT start early.

Thinking

Time-based jobs risk running before data arrives.

Solution

👉 Build a task DAG

Parent task → Child task

If parent fails, child does not run.

Benefit

✔ reliable sequencing
✔ no corrupt or partial data

⭐ Scenario 3 — Table scan takes too long

Problem

Query scans 90% of table even with filters.

Thinking checklist

1️⃣ Does filter match actual stored column?
2️⃣ Are values selective enough?
3️⃣ Are partitions aligned?
4️⃣ Does clustering help?

Fix options

optimize filters

verify correct data types

add clustering only if table is very large and repeatedly filtered on same key

Key takeaway

Fix query logic first — clustering later.

⭐ Scenario 4 — Lots of deletes and updates slowed queries

Problem

Performance degraded after heavy deletes.

Reason

Snowflake data is immutable.

Updates/deletes create:

new partitions for new data

old partitions kept for Time Travel

This causes:

✔ storage bloat
✔ more partitions scanned

Fix

avoid frequent delete-rewrite cycles

use soft delete flags if appropriate

recluster large tables when needed

⭐ Scenario 5 — Need fast loading from S3

Correct setup

✔ Create external stage
✔ Store compressed files
✔ Use COPY with parallelization

Why

Snowflake loads directly from cloud storage

Compression reduces transfer cost

COPY distributes work across threads automatically

⭐ Scenario 6 — Data duplicated after loads

Thinking

Likely loaded same file twice.

Fix

Track files using metadata columns:

$FILE

$ROW

$LOAD_TIME

Store processed file list in control table and skip repeat loads.

⭐ Scenario 7 — Task ran but target table did not update

Checklist

1️⃣ Check task history
2️⃣ Verify warehouse had permissions
3️⃣ Confirm SQL didn’t fail silently
4️⃣ Test logic manually

Most common cause:

Task executes, but query conditions filtered everything.

⭐ Scenario 8 — Queries reading unnecessary partitions

Cause

Filters don’t align with natural data distribution.

Fix

use correct date columns

avoid functions on filter columns (e.g., TO_DATE(col))

cluster only when justified

⭐ Scenario 9 — Bulk load jobs failing due to file size

Rule

Too many tiny files = slow
One giant file = slow

Fix

Aim for 100–250 MB compressed chunks.

⭐ Scenario 10 — Daily automated load: task or manual scripts?

Use tasks, not external cron jobs.

Reason:

✔ managed inside Snowflake
✔ retry logic available
✔ integrated monitoring

🔹 5️⃣ TRICK / MISCONCEPTION QUESTIONS — FULL EXPLANATIONS
❌ “Tasks replace Airflow / Informatica / ADF”

Reality

Tasks automate workflows inside Snowflake only.

External orchestration tools still needed for:

cross-system pipelines

complex dependencies

notifications, retries, governance

Snowflake tasks = lightweight internal scheduler.

❌ “Tasks run without warehouses”

Reality

Most tasks require warehouses.

Only serverless tasks don’t — but Snowflake still charges compute.

❌ “Micro-partitions work like indexes”

Reality

Indexes require maintenance.

Micro-partitions are:

✔ automatic
✔ metadata-driven
✔ immutable

Snowflake skips partitions using metadata — not pointers like indexes.

❌ “Clustering always improves performance”

Reality

Clustering helps only when:

table is very large

queries repeatedly filter the same key

Otherwise clustering wastes credits.

❌ “Time Travel duplicates entire tables”

Reality

Snowflake doesn’t copy data.

It stores historical pointers to older partitions.

❌ “Stages are permanent storage”

Reality

Stages are temporary landing zones.

They are not meant to replace data lake or archival storage.

❌ “COPY deletes source files automatically”

Reality

COPY reads only.
Deleting files is your pipeline’s responsibility.

❌ “Smaller files always load faster”

Reality

Thousands of tiny files overload Snowflake metadata.

Balanced file sizes load fastest.

❌ “Clustering fixes poorly written SQL”

Reality

Bad SQL stays bad.

First fix:

right filters

correct joins

avoid functions on filter columns

Only then consider clustering.

❌ “Every table should be clustered”

Reality

Most tables do not need clustering.

Clustering is a tuning tool — not default design.