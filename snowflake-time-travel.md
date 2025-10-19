# Snowflake Time Travel Feature Tutorial

## Table of Contents
1. [Introduction](#introduction)  
2. [Understanding Time Travel](#understanding-time-travel)  
3. [DML Operations and Silent Audits](#dml-operations-and-silent-audits)  
4. [Continuous Data Protection Life Cycle](#continuous-data-protection-life-cycle)  
5. [Invoking Time Travel Feature](#invoking-time-travel-feature)  
6. [Best Practices](#best-practices)  

---

## Introduction

Snowflake's **Time Travel** feature provides the ability to access historical data for a specified retention period. It allows users to query, restore, or clone data as it existed in the past, making it a powerful tool for auditing, error recovery, and compliance.

---

## Understanding Time Travel

- **Purpose**:
  - Recover accidentally deleted or updated data.
  - Audit changes silently.
  - Facilitate data versioning and cloning.

- **Retention Periods**:
  - Default: 1 day (24 hours) for standard accounts.
  - Enterprise and above: Can extend up to 90 days.

- **Scope**:
  - Applies to databases, schemas, tables, and stages.

**Example**:
```sql
-- Query historical data from 2 hours ago
SELECT * FROM orders AT (TIMESTAMP => CURRENT_TIMESTAMP - INTERVAL '2 HOURS');
```

---

## DML Operations and Silent Audits

Time Travel captures changes from **DML operations** (INSERT, UPDATE, DELETE, MERGE). This enables **silent audits** without explicit logging.

**Example DML Operations**:
```sql
-- Update a record
UPDATE customer_data SET email='newemail@example.com' WHERE customer_id=101;

-- Delete a record
DELETE FROM customer_data WHERE customer_id=102;
```

**Audit using Time Travel**:
```sql
-- Retrieve deleted records
SELECT * FROM customer_data BEFORE (STATEMENT => 'DELETE FROM customer_data WHERE customer_id=102');
```

- All changes are versioned and can be queried or restored within the retention period.

---

## Continuous Data Protection Life Cycle

Snowflake ensures **continuous data protection** through:

1. **Time Travel**:
   - Allows querying historical versions.
   - Retention period governs recoverable window.

2. **Fail-safe**:
   - 7-day period after Time Travel ends.
   - Snowflake-managed for disaster recovery.

**Data Protection Flow:**
```
Data Insert/Update/Delete --> Time Travel (Retain History) --> Fail-safe (Emergency Recovery) --> Permanent Storage
```

- Time Travel + Fail-safe ensures near-zero data loss.

---

## Invoking Time Travel Feature

### Querying Historical Data
```sql
-- Using TIMESTAMP
SELECT * FROM orders AT (TIMESTAMP => '2025-10-10 12:00:00');

-- Using Statement ID
SELECT * FROM orders BEFORE (STATEMENT => '010101-abcd-1234-5678');
```

### Restoring Tables
```sql
-- Restore dropped table within retention period
UNDROP TABLE customer_data;

-- Clone table as of specific point in time
CREATE TABLE customer_data_clone CLONE customer_data AT (TIMESTAMP => '2025-10-10 12:00:00');
```

### Restoring Schemas or Databases
```sql
-- Restore schema
CREATE SCHEMA analytics_clone CLONE analytics AT (TIMESTAMP => '2025-10-10 12:00:00');

-- Restore database
CREATE DATABASE sales_db_clone CLONE sales_db AT (TIMESTAMP => '2025-10-10 12:00:00');
```

---

## Best Practices

1. **Plan Retention**: Set Time Travel retention based on business recovery requirements.
2. **Use Cloning for Development**: Use historical clones for testing and auditing.
3. **Monitor Costs**: Extended Time Travel retention increases storage costs.
4. **Combine with Fail-safe**: Ensure complete disaster recovery strategy.
5. **Audit DML Changes**: Leverage Time Travel for silent audits and compliance reporting.

---

## References

- [Snowflake Time Travel Documentation](https://docs.snowflake.com/en/user-guide/data-time-travel.html)  
- [UNDROP and Cloning Tables](https://docs.snowflake.com/en/sql-reference/sql/undrop-table.html)

