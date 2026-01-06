
# ❄️ Snowflake DML — Complete Teaching Guide (INSERT | UPDATE | DELETE)

This version expands each topic with **clear explanations, examples, mistakes, recovery tips, and interview depth**.

---

## 🔹 A. What is DML?

### ❓ What is DML?
DML stands for **Data Manipulation Language**.  
It includes commands that change the contents of a table without changing its structure.

DML commands:
- INSERT — add rows
- UPDATE — modify existing rows
- DELETE — remove rows
- MERGE — upsert logic
- COPY INTO — bulk loading

Snowflake stores data in **micro‑partitions**, so every DML operation writes new partitions instead of modifying existing ones.

---

### ❓ How is DML different from DDL?
DDL = defines structure (tables, schemas, roles).  
DML = manipulates data inside those objects.

| Topic | DDL | DML |
|---|---|---|
| Purpose | Structure | Data |
| Examples | CREATE, ALTER, DROP | INSERT, UPDATE, DELETE |
| Commit behavior | Mostly auto‑commit | Transaction controlled |
| Impact | Metadata changes | Data rewrite |
| Time Travel recovery | Often possible | Very useful |

---

### ❓ Does DML auto‑commit in Snowflake?
By default: **Yes — auto‑commit is ON.**  
Each DML executes as its own transaction.

But you can disable it when needed:

```sql
BEGIN;
UPDATE ...
DELETE ...
COMMIT;
```

If anything fails before `COMMIT`, everything rolls back.

---

### ❓ Do multiple users conflict when editing?
Snowflake uses **MVCC (Multi‑Version Concurrency Control)**:

✔ readers never block writers  
✔ writers don’t block readers  
✔ no dirty reads  
✔ consistent snapshots per query

Every session sees the table **as it existed when the query started**.

---

### ❓ How does Time Travel help?
Time Travel lets you read or restore past versions:

```sql
SELECT * FROM orders AT (OFFSET => -60);
```

Or restore objects accidentally changed or dropped.

This is the real safety net for DML mistakes.

---

## 🔹 B. INSERT

### ⭐ Basics

#### ❓ Insert one row
```sql
INSERT INTO customers VALUES (1, 'Alex', 'NY');
```

#### ❓ Insert specific columns
```sql
INSERT INTO customers (id, name)
VALUES (2, 'Maria');
```

Columns not listed →
- take default values
- otherwise become NULL

#### ❓ Insert multiple rows
```sql
INSERT INTO customers VALUES
(3, 'Sam', 'TX'),
(4, 'John', 'CA');
```

---

### ⭐ Defaults & NULLs

#### ❓ Default values example
```sql
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

If no value is provided, Snowflake fills it automatically.

#### ❓ NOT NULL interaction
If you try inserting NULL into NOT NULL → **Snowflake rejects the row**.

```sql
SQL compilation error: NULL result in a NOT NULL column
```

NOT NULL is one of the few constraints Snowflake enforces physically.

---

### ⭐ INSERT…SELECT

#### ❓ Why use INSERT…SELECT?
Copy data from one table into another:

```sql
INSERT INTO archived_orders
SELECT * FROM orders WHERE status='CLOSED';
```

Common use cases:
- archiving
- migrations
- transformations

⚠️ Risk: inserting millions of rows = expensive + slow.  
Prefer batching or staged loads when possible.

---

### ⭐ COPY INTO — Bulk loading

#### ❓ INSERT vs COPY INTO
| INSERT | COPY INTO |
|---|---|
| manual values | loads files |
| row‑based | massively parallel |
| smaller volumes | very large volumes |
| slower | optimized + scalable |

Use COPY when loading data from:
- internal stage
- S3 / Azure / GCS

Example:
```sql
COPY INTO sales
FROM @mystage/sales_files/
FILE_FORMAT=(TYPE=CSV);
```

---

## 🔹 C. UPDATE

### ⭐ Basics

#### ❓ Standard update
```sql
UPDATE employees
SET salary = salary * 1.10
WHERE department='IT';
```

#### ❓ Forgot WHERE by mistake?
Every row updates — and likely commits.

Recovery path:
1️⃣ Time Travel (preferred)  
2️⃣ Rollback (only if still in transaction)

---

### ⭐ Expressions & joins

#### ❓ Update using another column
```sql
UPDATE orders
SET total = price * quantity;
```

#### ❓ Update using another table
```sql
UPDATE t
SET t.city = s.city
FROM customers t
JOIN staging s USING(id);
```

---

### ⭐ Updating VARIANT / JSON

#### ❓ Modify JSON key
```sql
UPDATE logs
SET data:event='login'
WHERE id=10;
```

Snowflake updates only affected element, not full JSON blob.

---

### ⭐ UPDATE vs MERGE

Use UPDATE when:
✔ rows already exist  
✔ you only modify values

Use MERGE when:
✔ sometimes insert, sometimes update

```sql
MERGE INTO target t
USING staging s
ON t.id=s.id
WHEN MATCHED THEN
  UPDATE SET t.amount=s.amount
WHEN NOT MATCHED THEN
  INSERT (id, amount) VALUES (s.id, s.amount);
```

MERGE is the backbone of **CDC pipelines**.

---

## 🔹 D. DELETE

### ⭐ Basics

#### ❓ Delete some rows
```sql
DELETE FROM customers WHERE inactive=TRUE;
```

#### ❓ Delete everything
```sql
DELETE FROM customers;
```

#### ❓ TRUNCATE vs DELETE
| DELETE | TRUNCATE |
|---|---|
| logs row changes | metadata drop |
| supports WHERE | removes all |
| slower | instant |

Logical delete pattern:
```sql
UPDATE customers SET is_deleted=TRUE;
```

Used when regulatory history is required.

---

### ⭐ Time Travel support

Deleted data can be restored until:
- retention period expires
- fail‑safe expires

Otherwise recovery is impossible.

---

## 🔹 E. Transactions & Safety

### ❓ What is auto‑commit?
Auto‑commit runs each statement as a standalone transaction.

Disable when performing multi‑step changes.

---

### ❓ BEGIN / COMMIT / ROLLBACK
```sql
BEGIN;
DELETE FROM t1;
UPDATE t2;
ROLLBACK;
```

Other sessions **never see partial changes**.

---

## 🔹 F. Performance Considerations

### ❓ Why can DML be expensive?
Every change rewrites micro‑partitions. This creates:
- more storage
- more metadata
- fail‑safe copies

### ❓ Why frequent deletes hurt?
Deletes mark rows invisible — partitions stay.  
More partitions → more scanning → slower queries.

Better patterns:
- batch processing
- staging tables
- MERGE instead of many updates

---

## 🔹 G. Scenario Questions

### ❓ Deleted wrong rows
Use Time Travel or restore snapshot.

### ❓ Inserted duplicates
Use QUALIFY + ROW_NUMBER to keep latest and remove rest.

### ❓ Massive insert needed
Stage files + COPY INTO — do not loop INSERTs.

### ❓ UPDATE without WHERE
Rollback if still open. If not — recover via Time Travel.

### ❓ Storage keeps growing after deletes
Because rows remain in retention window.

---

## 🔹 H. Trick / Misconceptions

❌ DELETE removes instantly  
✔ It marks data invisible first

❌ TRUNCATE = DELETE  
✔ TRUNCATE drops metadata only

❌ Two users overwrite each other  
✔ MVCC prevents that

❌ MERGE = UPDATE  
✔ MERGE adds conditional insert logic

❌ ROLLBACK works always  
✔ Not after auto‑commit

---

## 🎯 Final Teaching Takeaways
✔ DML changes data, not structure  
✔ MVCC guarantees safe concurrency  
✔ Time Travel protects against disasters  
✔ COPY INTO is preferred for bulk operations  
✔ Too many small DMLs cause fragmentation and cost
