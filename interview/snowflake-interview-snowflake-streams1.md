❄️ Snowflake Streams — Deep, Real-World Explanation (CDC + Auditing)
🔹 1️⃣ WORKING WITH SNOWFLAKE STREAMS
A. BASICS — WHAT IS A STREAM?
❓ What is a Snowflake Stream?

A Stream is a change-tracking object attached to a table (or view) that records what changed since the last time someone read it.

It tracks:

Type of change	Example
Insert	New order added
Update	Customer email corrected
Delete	Old record removed

Important:

Streams do not store copies of rows.
They store references to changes recorded by Time Travel.

You should imagine it like:

📌 “Sticky notes attached to the table telling you what was changed.”

❓ Why do we need Streams?

Without streams, every incremental pipeline would require:

❌ Full table reloads
❌ Expensive join comparisons
❌ Complex “delta detection” logic
❌ Increased compute cost
❌ Higher latency

Streams let you ask:

“Give me ONLY the rows that changed since last run.”

This makes pipelines:

✔ efficient
✔ predictable
✔ cheaper
✔ reliable

❓ What problem do Streams solve in ETL pipelines?

Typical ETL question:

“Yesterday I loaded data — what changed since then?”

Streams answer that automatically.

They are perfect for:

near real-time ingestion pipelines

replication to downstream systems

auditing and reconciliation

CDC workloads (Change Data Capture)

❓ Are Streams physical data copies?

No — and this is critical.

A Stream does not:

✖ duplicate data
✖ copy tables
✖ store entire records

Instead, it keeps pointers to modified micro-partitions.

This means:

✔ low storage usage
✔ always consistent with source
✔ minimal maintenance

❓ How do Streams track table changes?

When data changes, Snowflake stores new versions using:

✅ Time Travel
✅ metadata versioning

A Stream reads those versions and exposes only differences.

Think of it as:

📷 snapshot (before)
📷 snapshot (after)
📝 stream shows differences

❓ What is CDC?

CDC = Change Data Capture

Goal:

Detect data changes and deliver them downstream safely.

Snowflake Streams = built-in CDC engine.

❓ What operations can Streams capture?

Depends on stream type:

Stream Type	Inserts	Updates	Deletes
Standard	✔	✔	✔
Append-Only	✔	✖	✖
Insert-Only	✔	✖	✖
❓ Do Streams modify underlying tables?

Never.

Streams are passive observers.
They record — they don’t change.

❓ How often can a Stream be queried?

Unlimited — but:

Every read advances consumption state.

Meaning: old changes will not appear again.

❓ What privileges are needed?

User must have:

USAGE on database + schema

OWNERSHIP or SELECT on table

CREATE STREAM privilege on schema

B. STREAM OBJECT & DML AUDITING
❓ What metadata does a Stream store?

It stores:

what changed

table version at change time

consumption pointer (bookmark)

It does NOT store:

✖ permanent history
✖ full copies
✖ user info

❓ What are METADATA$ columns?

Streams automatically return extra fields.

Column	Meaning
METADATA$ACTION	INSERT or DELETE
METADATA$ISUPDATE	True if part of an Update
METADATA$ROW_ID	Internal identifier for row linkage
METADATA$VERSION	Table version
METADATA$DELETE	Marks logical delete

These allow you to reconstruct history.

❓ METADATA$ACTION

Updates are NOT stored as one operation.

Update transforms into:

1️⃣ delete old version
2️⃣ insert new version

So you can see change over time.

❓ METADATA$ISUPDATE

True when action belongs to an UPDATE event.

Allows differentiation between:

actual delete

“delete caused by update”

❓ METADATA$ROW_ID

Unique row fingerprint.

Crucial for matching update before/after values.

❓ Can Streams detect TRUNCATE?

No — TRUNCATE bypasses CDC.

Production pattern:

👉 Avoid TRUNCATE on tables with streams
👉 Use DELETE with WHERE when auditing needed

❓ Do Streams track changes forever?

No.

Streams rely on Time Travel.

Once retention expires:

❌ Stream becomes STALE
❌ Changes cannot be recovered

❓ What happens after a stream is consumed?

bookmark moves

Snowflake remembers latest processed point

next query returns only new changes

Consumption does NOT delete data — it only advances marker.

C. STREAM TYPES (DETAILED)
⭐ Standard Stream

Captures:

✔ Full CDC
✔ Before/after states

Use when:

syncing to downstream systems

slowly changing dimensions

operational CDC replication

back-auditing business transactions

⭐ Append-Only Stream

Tracks inserts only.

Perfect for:

log ingestion

append-only datasets

telemetry

clickstream

event pipelines

Lighter → lower compute.

⭐ Insert-Only Stream

Even lighter.

Used when:

every row is brand new

no updates/deletes ever occur

downstream only appends

❓ Why choose lighter stream types?

Because unnecessary metadata costs:

compute

memory

storage

Rule:

Choose smallest stream that meets business need.

D. DATA FLOW WITH STREAMS
❓ What is source table?

Table receiving actual data.

❓ What is target table?

Transformed table where cleaned/processed records go.

❓ Streams + Tasks = Pipeline

Typical pattern:

1️⃣ Data lands in table
2️⃣ Stream records changes
3️⃣ Task runs every X minutes
4️⃣ Reads stream
5️⃣ Applies logic to target table

This gives:

✔ near real-time ETL
✔ consistent incremental processing

❓ What happens when ETL reads stream?

changes delivered

only new rows appear

stream position moves forward

If ETL crashes before commit:

👉 pointer does not move
👉 safe retry

❓ Offset checkpoint

Internal pointer telling Snowflake:

“Up to here, the pipeline has already processed changes.”

❓ Can one stream feed multiple consumers?

No.

Each consumer requires its own stream.

Otherwise:

❌ one consumer steals changes from others.

❓ Can one table have multiple streams?

Yes — and common.

Example:

stream for ETL

stream for ML feature refresh

stream for audit

❓ Reset / recreate stream?

You must drop + recreate.

You may lose older data depending on retention.

🔹 2️⃣ TIME TRAVEL WITH STREAMS
❓ How do Streams rely on Time Travel?

Streams read history stored by Time Travel.

If history disappears…

Stream breaks.

❓ What is stale stream error?

Means:

Stream needs history that no longer exists.

To fix:

recreate stream

restore clone older than retention

extend retention next time

❓ What if table is dropped?

If recovered through:

✔ UNDROP
✔ CLONE

Then stream can sometimes be restored — depending on snapshot.

❓ Do Streams survive Fail-Safe?

Only indirectly.

Fail-Safe protects data, not streams.

If base table is restored, stream may still need reconstruction.

Best Practices

✔ consume streams frequently
✔ avoid long idle periods
✔ avoid truncate on CDC tables
✔ increase retention if pipelines are slow
✔ don’t treat streams as permanent audit systems

🔹 3️⃣ AUDITING INSERT / UPDATE / DELETE (IN DEPTH)
Inserts

Rows appear flagged INSERT.

Updates

Appear as:

DELETE (old row)
INSERT (new row)

Deletes

Appear as DELETE only.

❓ Do Streams track WHO changed data?

No.

To answer WHO, use:

QUERY_HISTORY

ACCESS_HISTORY

Join them together.

❓ What if a stream is read twice accidentally?

You cannot rewind.

Options:

restore clone and replay

rebuild ETL checkpoint logic

🔹 4️⃣ SCENARIOS — HOW A REAL ENGINEER THINKS

Instead of short answers, we explain:

👉 situation
👉 thought process
👉 decision
👉 why this is correct

⭐ Scenario 1 — “We need incremental pipeline. What should we use?”

Problem

Data lands continuously in a staging table.
You only want new/changed rows — NOT full reloads.

Thinking

Full reload = expensive & slow

Manual comparison logic = error-prone

We need built-in change capture

Decision

👉 Use Standard Stream + Task

Explanation

Stream tracks inserts, updates, deletes

Task runs on schedule and processes only deltas

Retry safe — if task fails, changes remain in stream

Result

✔ cheaper
✔ reliable
✔ supports near-real-time ingestion

⭐ Scenario 2 — “Pipeline missed a day. How do we catch up?”

Problem

Task stopped for 24 hours.
Stream now has too many changes OR stale risk.

Thinking

Streams rely on Time Travel.
If retention is still valid — changes still exist.

Decision

👉 Use Time Travel to backfill missing period
👉 Then resume stream processing

Why

Time Travel lets you recreate table at older timestamp and process missing data safely.

Lesson

Streams = “current window”
Time Travel = “historical safety net”

⭐ Scenario 3 — “Someone truncated table. Will stream recover data?”

Problem

TRUNCATE removes all rows.

Thinking

Streams only track CDC events.
TRUNCATE bypasses change tracking.

Decision

👉 Restore data using Time Travel or clone
👉 Rebuild stream if needed

Key takeaway

Streams = track changes
They are NOT backups.

⭐ Scenario 4 — “We only append logs. Which stream?”

Problem

Event logs never update or delete.

Thinking

Standard Stream is unnecessary overhead.

Decision

👉 Use Append-Only Stream

Why

Cheaper metadata tracking

Smaller footprint

Simplest to manage

⭐ Scenario 5 — “We want very light CDC — only inserts ever.”

Decision

👉 Insert-Only Stream

Why

Snowflake doesn’t spend time tracking deletes/updates you never use.

Rule

Choose the lightest stream type that still meets functional needs.

⭐ Scenario 6 — “Stream became STALE. What happened?”

Problem

Error:

STREAM has become stale

Thinking

Stream needed Time Travel history that expired.

Decision

1️⃣ recreate the stream
2️⃣ if needed, clone table from older snapshot
3️⃣ increase retention so it doesn’t happen again

Lesson

Streams are NOT permanent.
They depend on Time Travel windows.

🔹 5️⃣ MISCONCEPTIONS — FULL EXPLANATIONS
❌ Misconception 1

“Streams store full copies of changed rows.”

Reality

Streams store metadata + pointers, not data.

They reference Time Travel snapshots instead of duplicating storage.

✔ cheaper
✔ faster
✔ always consistent

❌ Misconception 2

“Streams keep history forever.”

Reality

History exists only as long as Time Travel retention exists.

Once retention expires:

stream may become stale

missing history cannot be replayed

you must recreate or backfill

❌ Misconception 3

“Streams can rewind backwards.”

Reality

Streams only move forward.

Reading advances consumption marker permanently.

If you accidentally process twice:

👉 recover using clone or backfill logic — not by rewinding stream.

❌ Misconception 4

“Streams replace Time Travel.”

Reality

Streams depend on Time Travel.

Think of roles:

Feature	Purpose
Streams	incremental data changes
Time Travel	historical snapshots & recovery

They complement each other — not substitutes.

❌ Misconception 5

“Streams show who changed the data.”

Reality

Streams show what changed, not who.

To track WHO, combine:

QUERY_HISTORY

ACCESS_HISTORY

Streams are technical CDC — not audit logs.

❌ Misconception 6

“Streams are good for full reload jobs.”

Reality

Streams are designed ONLY for incremental processing.

If you reload full table regularly:

👉 disable stream during reload
👉 or process from base table instead

Reload + stream together = confusion + data duplication.