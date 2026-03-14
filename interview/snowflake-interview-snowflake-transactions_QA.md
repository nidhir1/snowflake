❄️ Snowflake — Transactions (ACID)
Complete Teacher-Style Notes — Deep Explanations + Scenarios
🔹 1️⃣ WORKING WITH TRANSACTIONS (ACID)
A. BASICS — WHAT IS A TRANSACTION?
❓ What is a transaction?

A transaction is a logical group of SQL operations treated as one single unit of work.

Either:

✔ everything succeeds
❌ or nothing is saved

It prevents partial changes in the database.

❓ Why are transactions important?

They protect against:

partial updates

crashes during execution

coding mistakes

inconsistent states

Without transactions, data becomes unreliable.

❓ What does ACID stand for?

ACID guarantees reliability:

Property	Meaning
Atomicity	All or nothing
Consistency	Data remains valid
Isolation	Transactions do not affect each other
Durability	Committed data is permanent
❓ Why does Snowflake support ACID?

Snowflake runs mission-critical analytics.

Banks, healthcare, compliance teams rely on data accuracy.
ACID makes Snowflake trustworthy — even at massive scale.

❓ How are transactions different in analytics vs OLTP?

OLTP systems:

millions of tiny transactions

small updates

strict low latency

Snowflake analytics:

bulk loads / transforms

fewer transactions

larger operations

But ACID consistency still applies.

B. ACID PROPERTIES IN SNOWFLAKE
❓ What is Atomicity in Snowflake?

If one statement fails:

➡️ Snowflake rolls everything back.

No partial effects remain.

❓ What happens if one statement fails?

Everything after BEGIN is undone.
Uncommitted data is not visible to any other session.

❓ What is Consistency?

Data transitions from valid state → valid state.

Snowflake ensures this through:

schemas

constraints (informational)

transaction engine

metadata integrity

❓ What is Isolation?

Users should never see each other’s uncommitted work.

❓ What isolation model does Snowflake use?

Snowflake uses:

Snapshot Isolation via MVCC (Multi-Version Concurrency Control)

Every query reads a consistent snapshot.

❓ What is MVCC?

Instead of locking data, Snowflake stores multiple versions.

Readers read older copies.
Writers create new versions.

No blocking reads.

❓ How does MVCC avoid locks?

Because:

Writes create new partitions

Reads reference historical partitions

So transactions rarely block each other.

❓ What is Durability?

Once committed:

✔ stored redundantly
✔ survives crashes
✔ recoverable via fail-safe

Committed data never “disappears”.

C. TRANSACTION TYPES IN SNOWFLAKE
❓ What are implicit transactions?

Single statements auto-commit:

INSERT
UPDATE
DELETE
COPY INTO


Each one is a standalone transaction.

❓ What are explicit transactions?

You control commit:

BEGIN;
UPDATE ...
DELETE ...
COMMIT;


Useful when multiple steps must complete together.

❓ What is auto-commit?

Feature that commits automatically after each statement.

Default: ON

❓ Which statements auto-commit?

Most DML and DDL statements.

❓ What is the difference between auto-commit and manual commit?
Mode	Behavior
Auto-commit ON	Each statement commits automatically
Manual	Developer chooses when to commit
❓ When should auto-commit be disabled?

When operations must succeed as one block:

migrations

ETL transformations

correction scripts

financial adjustments

❓ When do long transactions cause problems?

They:

hold old data versions longer

increase storage

slow cleanup

complicate rollbacks

Always keep them short.

D. DDL STATEMENTS & TRANSACTIONS
❓ Are DDL transactional?

Yes — but they usually commit instantly.

❓ What if CREATE TABLE fails mid-way?

Snowflake rolls everything back.
You never see half-created objects.

❓ Are schema changes auto committed?

Yes.

❓ Can DDL be rolled back?

No — but you may:

✔ UNDROP
✔ use Time Travel
✔ restore clone

❓ Do DDLs block others?

Usually no — MVCC avoids locking.

❓ Why behave differently from RDBMS?

Snowflake prioritizes:

concurrency

analytics scale

non-blocking reads

Traditional locking would slow analytical workloads.

🔹 2️⃣ USING TRANSACTIONS — PRACTICAL
A. MANAGING TRANSACTIONS
❓ What does BEGIN do?

Starts explicit transaction.

❓ What does COMMIT do?

Saves all changes permanently.

❓ What does ROLLBACK do?

Undo all statements since BEGIN.

❓ What if session closes before COMMIT?

Snowflake rolls back automatically.

❓ Can we nest transactions?

No.
Nested transactions not supported.

❓ What is a savepoint?

Checkpoint inside transaction.

❓ Are savepoints supported?

Not natively — you simulate with logic, not SQL savepoints.

B. CURRENT_TRANSACTION() & MONITORING
❓ What is current_transaction()?

Returns transaction ID and context.

Used when debugging.

❓ How to detect open/long transactions?

Use:

QUERY_HISTORY

ACCOUNT_USAGE views

Query Profiler

Look for transactions running too long.

C. TRANSACTIONS WITH STORED PROCEDURES
❓ How are transactions handled?

By default — auto commit.

Procedures can manually control commit if needed.

❓ Can procedures commit/rollback?

Yes — but only when absolutely required.

❓ What if procedure fails?

Snowflake rolls back uncommitted state.

❓ Best practice?

Keep transaction scope small.
Let Snowflake manage when possible.

D. FAILED TRANSACTIONS & RECOVERY
❓ What if transaction partially succeeds?

It doesn’t — Snowflake rolls back.

❓ How does Time Travel help?

Allows rollback to previous state when data was committed earlier.

❓ Can fail-safe recover?

Yes — only in critical cases with Snowflake support.

🔹 3️⃣ SCENARIO-BASED QUESTIONS
⭐ ETL inserts half and fails — what happens?

Rollback. Nothing is committed.

⭐ Developer forgets COMMIT — is data visible?

No. Session closes → rollback.

⭐ Accidentally truncated table — recovery?

Use Time Travel or UNDROP.

⭐ Procedure crashed mid-process — corrupted?

No — snapshot isolation prevents partial commits.

⭐ Two users update same table?

MVCC maintains snapshots. No dirty reads.

⭐ Long transaction blocking?

Check query profile + MVCC version retention.

⭐ Script runs CREATE + INSERT + UPDATE?

DDL commits instantly. The rest depends on auto-commit.

⭐ Auto-commit disabled accidentally?

Commit intentionally or rollback. Do not leave dangling.

🔹 4️⃣ TRICK / MISCONCEPTION QUESTIONS
❌ “We always need BEGIN and COMMIT”

No — auto-commit exists and is default.

❌ “SELECT needs transactions”

SELECT doesn’t change data.

❌ “Snowflake locks tables”

Snowflake avoids heavy locking via MVCC.

❌ “Concurrency causes dirty reads”

Never — snapshot isolation protects.

❌ “Rollback undoes DDL”

No — use Time Travel / UNDROP.

❌ “Uncommitted data is visible to others”

Never visible.

❌ “Time Travel replaces transactions”

Time Travel is recovery — not transaction control.

❌ “Transactions are slower in Snowflake”

They are optimized differently — still efficient.

❌ “Snowflake works same as OLTP databases”

Transaction behavior is optimized for analytics workloads.