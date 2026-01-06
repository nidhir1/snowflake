# ❄️ Snowflake — Procedures & Views
## Full Q&A — Teacher Style (Clear Questions + Detailed Answers)

---
## 1️⃣ STORED PROCEDURES — BASICS

### ❓ What is a stored procedure in Snowflake?
A stored procedure is a reusable program stored inside Snowflake that executes one or more SQL statements — plus logic such as loops, conditions, validation, and error handling — as a single unit. Instead of manually running multiple commands, we package them into a callable object.

### ❓ Why do we use stored procedures?
We use procedures when logic:
- repeats frequently
- touches multiple tables
- needs orchestration
- needs audit and error handling

They centralize business rules instead of scattering SQL scripts everywhere.

### ❓ What kind of logic is best suited?
Procedures are best for orchestration, not raw analytics:
- ETL pipelines
- data cleanup workflows
- slowly changing loads
- admin automation

### ❓ Where do procedures execute?
The control logic runs inside Snowflake services. The SQL inside uses a virtual warehouse — meaning compute billing still applies.

### ❓ Supported languages?
- SQL procedures
- JavaScript procedures
(Snowpark expands options, but core concepts remain these two.)

### ❓ SQL vs JavaScript procedures?
SQL procedures = SQL with some procedural abilities.  
JavaScript procedures = programming logic with embedded SQL — better for complex flows.

### ❓ What is a handler function?
In JavaScript, it is the function Snowflake calls when executing the procedure — like the entry point.

### ❓ Return value?
All procedures must declare a return type and return something meaningful such as status text or JSON summary.

### ❓ Required privileges?
- USAGE on DB + schema
- CREATE PROCEDURE on schema
- privileges for objects touched inside the procedure

### ❓ Can procedures modify data?
Yes — procedures can run DML and DDL. Because of that, they must be role‑controlled.

---
## 2️⃣ CREATING & USING PROCEDURES

### ❓ Core components?
Name, params, return type, language, execution context, and body.

### ❓ What are parameters?
External inputs passed to influence procedure behavior.

### ❓ Input vs Output?
Inputs are parameters. Output is the return value — Snowflake does not use OUT parameters.

### ❓ How do we declare variables?
SQL uses DECLARE blocks. JavaScript uses normal JS variable syntax.

### ❓ Loops?
Available — essential for batch jobs and metadata‑driven logic.

### ❓ IF / ELSE logic?
Used to branch business rules (full vs incremental loads, exception logic, etc.).

### ❓ Can procedures call other procedures?
Yes — modular design encourages reuse.

### ❓ Dynamic SQL?
Supported — but should be validated to avoid unsafe construction.

### ❓ EXECUTE AS OWNER vs CALLER?
OWNER runs with creator privileges. CALLER respects caller permissions. Critical governance concept.

### ❓ Transactions?
Procedures can wrap steps in transactions so partial failures don’t corrupt data.

---
## 3️⃣ CALL COMMAND

### ❓ What is CALL used for?
To execute a stored procedure and return results.

### ❓ Syntax?
CALL proc_name(arg1,arg2);

### ❓ How are arguments passed?
By position. Casting happens automatically where safe.

### ❓ Wrong parameter?
Automatic conversion if possible, otherwise error.

### ❓ Where can CALL run?
Worksheet, SnowSQL, APIs, orchestration tools, notebooks — anywhere SQL runs.

### ❓ What happens when procedure fails?
Execution stops and transaction is rolled back unless errors are handled explicitly.

### ❓ Capturing results?
Either via return values or logging to audit tables.

### ❓ Logging messages?
Yes — strongly recommended.

### ❓ Debugging CALL?
Logs, try/catch, query history, isolating failing logic.

### ❓ Best practices?
Document behavior, validate inputs, secure carefully, avoid unnecessary OWNER mode.

---
## 4️⃣ VIEWS — BASICS

### ❓ What is a view?
A saved SQL query that presents data like a table without storing another copy.

### ❓ Why do we use views?
To simplify logic, protect sensitive data, and create semantic business layers.

### ❓ Logical vs physical?
Views are logical. Data remains physically stored only in tables.

### ❓ Regular view?
Runs the SELECT every time — always current.

### ❓ Do views store data?
No — unless they’re materialized views.

### ❓ Base table changes?
View reflects new data immediately; dropping the table breaks the view.

### ❓ Can views be updated?
Generally read‑only. Updates should modify base data.

### ❓ What is a secure view?
A hardened view designed to safely expose shared data without leaking internal logic.

### ❓ Why secure views?
Essential when exposing data to partners or external users.

### ❓ Permissions?
Create requires CREATE VIEW + SELECT. Query requires SELECT on view.

---
## 5️⃣ VIEW STORAGE & BEHAVIOR

### ❓ Does Snowflake store view SQL?
Yes, as metadata.

### ❓ Where?
Internal catalog — accessible via INFORMATION_SCHEMA and ACCOUNT_USAGE.

### ❓ Do views benefit from result cache?
Yes — same rules apply as regular queries.

### ❓ Does optimizer still work?
Yes — Snowflake pushes filters, prunes partitions, rewrites queries.

### ❓ Masking policies?
Applied automatically whether querying tables or views.

### ❓ Materialized on regular views?
Possible but avoided — stability matters.

### ❓ Dropping base tables?
View becomes invalid — it is not a backup.

### ❓ Dependency tracking?
Snowflake tracks lineage for impact analysis.

### ❓ Views referencing views?
Allowed — but stacking too many hurts clarity.

---
## 6️⃣ SYSTEM PREDEFINED VIEWS

### ❓ What are system views?
Built‑in metadata and governance query views.

### ❓ INFORMATION_SCHEMA?
Describes structure of DB objects.

### ❓ ACCOUNT_USAGE?
Provides account‑wide history, query logs, storage metrics, warehouse usage.

### ❓ ORGANIZATION_USAGE?
Cross‑account governance and billing.

### ❓ When should we use them?
Auditing, optimization, troubleshooting, lineage, chargeback analysis.

### ❓ Monitoring use cases?
Slow queries, expensive workloads, growth tracking, security reviews.

### ❓ Retention?
Varies — generally months to around a year.

### ❓ Cost?
Compute is billed when queried.

### ❓ Permissions?
Yes — roles determine what you see.

### ❓ Examples?
Query history, privilege audits, dependency checks.

---
## 7️⃣ SCENARIOS

Each scenario reinforces design decisions using procedures, views, and system metadata.

---
## 8️⃣ MISCONCEPTIONS

Stored procedures are not required for SQL.  
Views do not store data.  
Materialized views are different.  
CALL respects security.  
System views are read‑only.  
Dropping tables breaks views.

---
## 🎯 FINAL TAKEAWAY

Procedures automate workflows.  
Views create safe semantic layers.  
System views provide visibility and governance.



# ❄️ Snowflake — Procedures & Views
## Expanded Sections: Scenarios + Misconceptions (Teacher Style)

---

## 7️⃣ SCENARIO‑BASED QUESTIONS — REAL PROJECT THINKING

### ❓ You need reusable ETL logic that runs multiple SQL steps. What should you use?
Use a **stored procedure**.  
A procedure lets you package staging, validation, inserts, updates, and logging into one reusable workflow. Instead of scripts everywhere, logic lives in Snowflake and can be versioned, secured, and reused.

---

### ❓ Analysts must see data but PII must be hidden. What do you build?
Use a **secure view** with **masking policies**.  
Secure views hide underlying objects and logic, and masking ensures only allowed users see sensitive data. This is safer than giving table access.

---

### ❓ You must restrict users so they only see their own rows. View or table?
Use **views + row access policies**.  
Views provide filtered projections. Tables should remain raw and complete.

---

### ❓ Daily cleanup job runs automatically. Procedure or task?
Use **stored procedure + task**.  
Procedure = logic.  
Task = scheduling.  
This prevents cron jobs outside Snowflake and centralizes governance.

---

### ❓ You need read‑only layer for dashboards. View or clone?
Use **views**.  
Views create a stable contract for BI tools while allowing backend tables to evolve.

---

### ❓ Developer must run code with elevated privileges. What model?
Use **EXECUTE AS OWNER**, carefully controlled.  
The procedure runs with the owner’s permissions while callers cannot directly touch objects.

---

### ❓ Base table accidentally dropped — what happens to the view?
The view still exists, but it fails when queried.  
Views are not backups. Restore the table with Time Travel or recreate it.

---

### ❓ Views suddenly slow — what to check first?
Check:
1️⃣ query plan (PROFILE)  
2️⃣ whether data volume changed  
3️⃣ whether filters can be pushed down  
4️⃣ whether warehouse size is appropriate  
Views don’t cause slowness — underlying queries do.

---

### ❓ Team wants query history and usage visibility. Where do we look?
Use **ACCOUNT_USAGE** and **QUERY_HISTORY**.  
These provide who ran, what ran, cost, duration, and failures.

---

### ❓ Need governance visibility across schemas. What helps?
Use:
- OBJECT_PRIVILEGES  
- ROLE_GRANTS  
- ACCESS_HISTORY  

This reveals who has access to what and how data flows.

---

## 8️⃣ TRICK / MISCONCEPTION QUESTIONS (FULL EXPLANATIONS)

### ❌ “Stored procedures are required to run SQL.”
False. SQL runs normally without procedures. Procedures exist only to automate workflows.

---

### ❌ “Views physically store data.”
False. Regular views do not store data — they only store SQL definitions. Data always lives in tables.

---

### ❌ “Views automatically improve performance.”
False. Views do not make queries faster. Performance comes from the warehouse, pruning, and table design — not from using a view.

---

### ❌ “Materialized views are the same as normal views.”
False. Materialized views precompute and store results and have maintenance overhead. Regular views do not.

---

### ❌ “CALL bypasses security.”
False. Security still applies. Execution context (OWNER vs CALLER) determines privilege scope — it is controlled, not bypassed.

---

### ❌ “Procedures are always faster.”
False. Procedures don’t speed SQL — they coordinate it. Poor SQL still runs poorly.

---

### ❌ “If a table is dropped, the view still holds data.”
False. The view breaks. It does not preserve data.

---

### ❌ “System views can be edited.”
False. System views are read‑only. They exist for monitoring.

---

### ❌ “System views update in real time.”
Not always. Some have delay (minutes‑hours). They are near‑real‑time, not instantaneous.

---

### ❌ “Everyone can see stored procedure code.”
False. Visibility depends on privileges. Sensitive logic is protected by role security.

---


