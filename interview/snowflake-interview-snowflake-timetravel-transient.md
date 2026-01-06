# Snowflake — Time Travel, Fail-safe and Transient Tables
Questions Only

---

## 1. TIME TRAVEL FEATURE IN SNOWFLAKE

### A. BASICS — WHAT TIME TRAVEL IS
- What is Time Travel in Snowflake?
- Why do we need Time Travel?
- What kinds of operations does Time Travel help recover from?
- What objects support Time Travel?
- What is retention period in Time Travel?
- What is the default retention for permanent tables?
- Can the retention period be changed?
- Do schemas and databases support Time Travel?
- Does Time Travel store duplicate copies of data?
- Where does Time Travel metadata live?

### B. DML OPERATIONS & SILENT AUDITS
- What are DML operations in Snowflake?
- Which DML operations are tracked for Time Travel?
- What does “silent audit” mean?
- How does Snowflake internally track history?
- Is Time Travel manual or automatic?
- Does Time Travel work for INSERT, UPDATE, DELETE?
- Does TRUNCATE support Time Travel recovery?
- Does DROP TABLE support Time Travel?
- What happens when table structure changes?
- Can we query historical versions without restoring?

### C. CONTINUOUS DATA PROTECTION (CDP)
- What is Continuous Data Protection?
- How does CDP relate to Time Travel?
- How does Time Travel differ from Fail-safe?
- Is CDP the same as backup?
- Why is CDP better than manual backups?
- What are retention tradeoffs (cost vs safety)?
- How long can enterprise plans retain Time Travel data?
- How does retention affect storage billing?
- What happens when retention expires?
- What risks exist if retention is too short?

### D. INVOKING TIME TRAVEL — HOW TO USE
- How do you query older data using Time Travel?
- What is the AT clause?
- What is the BEFORE clause?
- What parameters can be used (timestamp, offset, query ID)?
- What is statement-level restore?
- How do you restore dropped tables?
- What is CREATE TABLE … CLONE used for in recovery?
- Why is cloning safer than replacing?
- What is UNDROP?
- When should you avoid restoring directly over active tables?

---

## 2. USING TIME TRAVEL (PRACTICAL)

### A. OFFSET & QUERY ID
- What is offset-based Time Travel?
- What units are allowed in offset?
- When would you use offset instead of timestamp?
- What is Query ID–based recovery?
- How does Query ID help identify the exact point in time?
- Where can Query IDs be found?
- How do you view query details?
- How to determine which query caused damage?
- How to use Query ID for BEFORE recovery?
- Which approach is safest in production?

### B. DATA RECOVERY USING TIMESTAMP
- What is timestamp-based recovery?
- How do you convert local time to UTC?
- Why use timestamp specifically?
- What timezone does Snowflake store history in?
- Can timezone differences affect restore accuracy?
- Can we restore partial tables using timestamp?
- What are best practices when restoring using timestamp?

### C. FAIL-SAFE & UNDROP
- What is Fail-safe in Snowflake?
- Who controls Fail-safe — user or Snowflake?
- How long is the Fail-safe period?
- What operations are covered by Fail-safe?
- Does Fail-safe cost storage?
- Can users directly query Fail-safe data?
- What is the difference between Fail-safe and backups?
- What is UNDROP used for?
- When does UNDROP fail?
- Can UNDROP be applied to schemas and databases?
- What happens if an object passes both retention and Fail-safe windows?
- Who should be contacted if Fail-safe recovery is needed?

---

## 3. TRANSIENT TABLES

### A. BASICS
- What is a transient table?
- How is a transient table different from a permanent table?
- Do transient tables support Time Travel?
- Do transient tables support Fail-safe?
- Why are transient tables cheaper?
- When should transient tables be used?
- When should transient tables not be used?
- Can transient tables store important historical data?
- Do transient tables survive session close?
- Can transient tables be cloned?

### B. REAL-TIME USAGE
- What are typical staging / intermediate use cases?
- What is temporary load processing?
- How are transient tables useful for scratch or exploration work?
- Why do transient tables help in ETL?
- What risks exist if a transient table is dropped?
- How can transient data be protected if needed?
- What retention options exist for transient tables?
- Are transient schemas required for transient tables?
- Do constraints work on transient tables?
- What happens if a transient table is accidentally truncated?

### C. DIFFERENCES & RESTRICTIONS
- Permanent vs transient — storage billing differences.
- Permanent vs transient — recovery differences.
- Are there privilege differences?
- Can we convert permanent tables to transient?
- Can transient objects participate in replication?
- Are transient objects supported by all Snowflake features?
- Do views behave differently over transient tables?
- How do backups treat transient tables?
- Why do organizations impose governance rules on transient usage?
- What audit risks come from transient data?
- What happens if someone misuses transient tables for critical data?

---

## 4. SCENARIO-BASED QUESTIONS

- Developer accidentally deleted rows — what do you do?
- A TRUNCATE wiped a table — what is the recovery path?
- You dropped a schema by mistake — what options exist?
- A bad ETL script overwrote data — how do you roll back?
- Business wants historical change view — which table type fits?
- Data load corrupted staging — is recovery necessary?
- Sensitive data is temporarily stored — transient or permanent?
- Audit requires restoring last week’s version — is transient suitable?
- Time Travel retention was set too small — what are the consequences?
- Fail-safe is needed — when and why should Snowflake support be contacted?

---

## 5. TRICK / MISCONCEPTION QUESTIONS

- Is Time Travel a replacement for backups?
- Does Fail-safe work for free and unlimited?
- Does Time Travel store full physical copies?
- Does retention mean data is protected forever?
- Can any user run UNDROP?
- Does transient mean temporary?
- Are transient tables automatically deleted?
- Do transient tables cost nothing?
- Does Time Travel slow down queries?
- Is Time Travel available on all Snowflake editions?