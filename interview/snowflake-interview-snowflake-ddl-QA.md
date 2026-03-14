
# ❄️ Snowflake DDL — CREATE / ALTER / DROP (Full Q&A Guide)

## 🔹 A. FOUNDATIONS — WHAT IS DDL?

### ❓ What is DDL?
DDL (Data Definition Language) controls the structure of database objects — not the data inside them.  
It is used to create, modify, and remove Snowflake objects (tables, schemas, warehouses, views, etc.)

Examples: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`.

---
### ❓ How does DDL differ from DML?
DDL changes **structure**.  
DML changes **data**.

| DDL | DML |
|---|---|
| create objects | insert data |
| modify structure | update data |
| drop objects | delete data |

---
### ❓ Does DDL commit automatically in Snowflake?
Yes.  
DDL operations are auto‑committed — they are not part of user transactions.

---
### ❓ Can DDL be rolled back?
Generally **no**.  
Because objects are metadata‑driven, once structure changes, it is committed instantly.

However, Snowflake has safety features:

✔ `UNDROP` (within retention)  
✔ Time Travel (for data objects)

---
### ❓ Does DDL block other users?
No locking like traditional RDBMS.  
Snowflake is metadata‑driven — DDL is fast and non‑blocking (with rare exceptions like ongoing loads).

---
### ❓ Who is allowed to run DDL?
Only users with appropriate ownership or privileges such as:

- OWNERSHIP
- CREATE on schema/database
- ACCOUNTADMIN / SYSADMIN roles

---
### ❓ Where is metadata stored?
In Snowflake’s centralized metadata layer.  
This is why:

✔ DDL is fast  
✔ Time Travel works  
✔ Cloning uses zero‑copy

---
### ❓ What happens after failed DDL?
No changes occur — structure remains unchanged.

> **Teacher tip**  
> Snowflake DDL = metadata operations, not heavy storage rewrites.

---

## 🔹 B. CREATE — EVERYTHING STUDENTS MUST KNOW

### ❓ What can be created?
- Databases, Schemas
- Tables (Permanent / Transient / Temporary / External)
- Views (Secure / Materialized)
- Warehouses
- Stages
- Procedures, Functions, Streams, Tasks
- Roles, Users

---
### ❓ What does CREATE OR REPLACE do?
Drops the existing object and recreates it.

Risk:
- ALL DATA may be lost
- GRANTS reset
- Dependencies may break

Use sparingly.

---
### ❓ Difference between create variations

| Command | Behavior |
|---|---|
| CREATE | fails if exists |
| CREATE IF NOT EXISTS | creates only if missing |
| CREATE OR REPLACE | replaces + destroys existing |

---
### ❓ Who becomes object owner?
The role running the CREATE statement.

---
### ❓ What if DB/schema is not selected?
Object is created in the current session namespace, e.g.:

```
USE DATABASE TRAINING;
USE SCHEMA LAB;

CREATE TABLE T1(...)
```

If namespace isn’t set, mistakes happen — objects land in wrong schemas.

---
### CREATE TABLE — Important Concepts

#### Table types

| Type | Use Case |
|---|---|
| Permanent | long‑term production |
| Transient | staging / non‑critical |
| Temporary | session‑only |
| External | reading from cloud storage |

---
### ❓ Default behaviors
- Columns allow NULL unless specified
- No PK/UK enforcement physically
- Retention defaults from database/schema

Defaults can be expressions such as:

```
created_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP()
```

---
### ❓ Create table from select
```
CREATE TABLE sales_copy AS
SELECT * FROM sales;
```

---
### ❓ Cloning tables
Metadata‑only, instant copy:

```
CREATE TABLE t_clone CLONE t_original;
```

No extra storage until changed.

---

## 🔹 CREATE VIEW

### ❓ Logical vs physical storage
Views do NOT store data — they store queries.

Materialized views store **results**.

---
### ❓ Secure views
Hide definition and protect sensitive logic.

---
### ❓ Dependency risk
If base table drops or changes — view can break.

---

## 🔹 CREATE WAREHOUSE
Warehouses handle compute — not storage.

Key settings:

- size
- auto suspend
- min/max clusters

Design separately to control cost.

---

## 🔹 C. ALTER — MODIFYING OBJECTS SAFELY

### ❓ What is ALTER used for?
Modify existing objects safely without recreating.

---
### ALTER TABLE rules

| Action | Notes |
|---|---|
| Add column | allowed |
| Drop column | sometimes blocked |
| Rename | safe but impacts tools |
| Change datatype | allowed when compatible |
| Add NOT NULL | fails if existing NULLs |
| Move across schema | allowed |
| Change retention | allowed |

---

### ALTER VIEW
You can modify using:

```
CREATE OR REPLACE VIEW
```

Risk: dependent dashboards may break.

---

### ALTER WAREHOUSE
You can:

- resize
- suspend/resume
- change scaling
- set auto suspend

Billing starts only while running.

---

## 🔹 D. DROP — DELETING OBJECTS

### ❓ DROP vs TRUNCATE
DROP deletes the object.  
TRUNCATE deletes data only.

---
### ❓ Does DROP free space?
Yes — after retention and fail‑safe complete.

---
### ❓ Can we UNDROP?
Yes — within Time Travel period:

```
UNDROP TABLE my_table;
```

---
### ❓ Dependent objects
Views, tasks, pipelines may fail if sources drop.

---

## 🎯 Scenario Questions

(teacher use — discussion prompts)

1️⃣ Developer used OR REPLACE — table gone → recover via UNDROP / Time Travel  
2️⃣ NOT NULL added — failed → existing NULL data  
3️⃣ Rename broke dashboards → dependencies tied to names  
4️⃣ Dropped table by mistake → UNDROP or restore clone  
5️⃣ Table created wrong schema → search session namespace  
6️⃣ Datatype change allowed in one place but not another → incompatible conversion

---

> If you want, I can expand this more with diagrams, SQL labs, and interview notes.

