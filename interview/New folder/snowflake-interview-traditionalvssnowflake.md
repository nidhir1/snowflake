# TRADITIONAL RDBMS vs SNOWFLAKE — QUESTION BANK

## 🔹 1. BASIC — FUNDAMENTAL QUESTIONS

What is a traditional RDBMS?

Examples of traditional RDBMS.

What is Snowflake?

Is Snowflake an RDBMS?

What is the basic purpose of an RDBMS?

What is the basic purpose of Snowflake?

What is OLTP?

What is OLAP?

Which systems are generally used for OLTP?

Which systems are generally used for OLAP?

Why do enterprises separate OLTP and OLAP?

What is schema-on-write?

What is schema-on-read?

Which approach is typical for RDBMS?

Which approach is common in Snowflake?

What are indexes in RDBMS?

Does Snowflake require indexes?

What is storage scaling in RDBMS?

What is compute scaling in Snowflake?

Can Snowflake be used for reporting and analytics?

---

## 🔹 2. INTERMEDIATE — UNDERSTANDING KEY DIFFERENCES

How is performance tuning done in RDBMS?

How is performance tuning handled in Snowflake?

What is manual partitioning?

Does Snowflake require partition management?

How are backups handled in RDBMS?

How are backups / recovery handled in Snowflake?

What is clustering in traditional databases?

What is clustering in Snowflake?

What is sharding?

Does Snowflake require manual sharding?

How do RDBMS systems scale — vertically or horizontally?

How does Snowflake scale?

What is high availability in traditional DBs?

How does Snowflake provide high availability?

How are upgrades managed in RDBMS?

Who manages upgrades in Snowflake?

What is hardware dependency in RDBMS?

Does Snowflake need hardware provisioning?

What is concurrency limitation in RDBMS?

How does Snowflake manage concurrency?

---

## 🔹 3. ADVANCED — ARCHITECTURE & DESIGN THINKING

What is shared-disk architecture?

What is shared-nothing architecture?

What is multi-cluster shared data architecture?

Which category does Snowflake belong to?

How does Snowflake separate compute and storage?

Why is separation of compute important?

What happens if multiple teams query the same data in RDBMS?

How does Snowflake avoid resource contention?

What is workload isolation?

Why is workload isolation critical in analytics?

How is ETL handled in RDBMS vs Snowflake?

What is data ingestion simplicity in Snowflake?

How is semi-structured data handled in RDBMS?

How is semi-structured data handled in Snowflake?

Why is JSON difficult in RDBMS?

How does VARIANT simplify JSON handling in Snowflake?

What is data sharing in RDBMS?

How does Snowflake securely share data without copying?

What is replication overhead in RDBMS?

How is replication simplified in Snowflake?

---

## 🔹 4. SCENARIO-BASED — REAL PROJECT THINKING

Your OLTP database is slowing down due to reporting — what would you do?

Data volume keeps increasing — RDBMS performance drops — options?

Business wants analytics across regions — RDBMS vs Snowflake?

You need near real-time dashboards without impacting production — choose what?

Your team needs fast sandbox copies of production — RDBMS vs Snowflake?

Your company acquires another company — need to merge data — what platform fits better?

You receive semi-structured logs — JSON, XML — how would each system handle it?

Management wants zero downtime upgrades — which system is easier?

Data archive requirement extends to 7+ years — RDBMS or Snowflake?

Analyst team requires multiple environments — Dev / UAT / Prod — which platform simplifies?

---

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS

Does Snowflake replace OLTP systems?

Is Snowflake just a faster database?

Do we create indexes in Snowflake for tuning?

Does Snowflake automatically partition like RDBMS range partitions?

Is Time Travel the same as backup?

Can Snowflake be used as an application database?

Is Snowflake only for structured relational data?

Does Snowflake eliminate the need for good data modeling?

Is Snowflake “just another hosted database”?

If Snowflake is cloud-based, does it mean vendor lock-in?
