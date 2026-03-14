# ❄️ Snowflake — Transactions (ACID)
## Question Bank — Questions Only

---

## 🔹 1️⃣ WORKING WITH TRANSACTIONS (ACID)

### A. BASICS — WHAT IS A TRANSACTION?
What is a transaction?  
Why are transactions important?  
What does ACID stand for?  
Why does Snowflake support ACID?  
How are transactions different in analytics vs OLTP systems?

### B. ACID PROPERTIES IN SNOWFLAKE
What is Atomicity in Snowflake?  
What happens if one statement in a transaction fails?  
What is Consistency?  
How does Snowflake ensure data consistency?  
What is Isolation?  
What isolation model does Snowflake use?  
What is multi-version concurrency control (MVCC)?  
How does MVCC help avoid locks?  
What is Durability?  
How does Snowflake guarantee durability?

### C. TRANSACTION TYPES IN SNOWFLAKE
What are implicit transactions?  
What are explicit transactions?  
What is auto-commit mode?  
Which statements auto-commit by default?  
What is the difference between auto-commit and manual commit?  
When should auto-commit be disabled?  
When do long-running transactions cause problems?

### D. DDL STATEMENTS & TRANSACTIONS
Are DDL statements transactional in Snowflake?  
What happens if CREATE TABLE fails halfway?  
Are schema changes automatically committed?  
Can DDL be rolled back?  
Are grants/privileges part of transaction scope?  
Do DDLs block other readers/writers?  
Why do DDLs behave differently in Snowflake compared to RDBMS?

---

## 🔹 2️⃣ USING TRANSACTIONS — PRACTICAL

### A. MANAGING TRANSACTIONS
What does BEGIN / START TRANSACTION do?  
What does COMMIT do?  
What does ROLLBACK do?  
What happens if session closes without commit?  
Can we nest transactions in Snowflake?  
What is savepoint?  
Are savepoints supported natively?

### B. CURRENT_TRANSACTION() & MONITORING
What is current_transaction()?  
What information does it return?  
Why is it useful?  
How do we detect currently open transactions?  
How to identify long-running transactions?  
How to troubleshoot blocked operations?  
How can query profiler help?

### C. TRANSACTIONS WITH STORED PROCEDURES
How are transactions handled inside stored procedures?  
Do procedures auto-commit?  
Can procedures explicitly control COMMIT/ROLLBACK?  
What happens if an error occurs in a procedure?  
How to log failed transactions from procedures?  
What is best practice for transaction control in procedures?  
When NOT to manage transactions manually inside procedures?

### D. FAILED TRANSACTIONS & RECOVERY
What happens if transaction partially succeeds?  
How does Snowflake guarantee rollback?  
How does Time Travel help recover mistakes?  
Can we see previous committed states?  
Can fail-safe recover transactions?

---

## 🔹 3️⃣ SCENARIO-BASED QUESTIONS
ETL inserts half the data and fails — what happens?  
Developer forgot COMMIT — is data visible?  
Accidentally truncated table — how to recover?  
Stored procedure crashed mid-process — is data corrupted?  
Two users update same table at once — how does Snowflake handle it?  
Long-running transaction blocks other work — what to check?  
Script executes CREATE, INSERT, UPDATE together — commit behavior?  
Auto-commit was disabled accidentally — how to fix without data loss?

---

## 🔹 4️⃣ TRICK / MISCONCEPTION QUESTIONS
Do you always need BEGIN/COMMIT in Snowflake?  
Are transactions required for SELECT queries?  
Does Snowflake lock tables for writes?  
Does multi-user concurrency cause dirty reads?  
Can auto-commit be disabled globally?  
Does ROLLBACK undo DDLs?  
Can uncommitted data be shared across sessions?  
Does Time Travel replace transactions?  
Are transactions slower in Snowflake?  
Are transactions identical to OLTP systems?

---

## 🎯 TEACHING SUMMARY — KEY TAKEAWAYS
✔ Snowflake is ACID  
✔ Uses MVCC to avoid heavy locking  
✔ Auto-commit ON by default, but explicit control is available  
✔ DDL behaves differently — usually auto-commit  
✔ Stored procedures handle transactions carefully  
✔ Time Travel helps recover after mistakes, but not a substitute for transactions
