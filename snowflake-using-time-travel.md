# Using Time Travel in Snowflake Tutorial

## Table of Contents
1. [Introduction](#introduction)  
2. [Time Travel using Offset and Query ID Features](#time-travel-using-offset-and-query-id-features)  
    - [Using OFFSET](#using-offset)  
    - [Using Query ID](#using-query-id)  
3. [Data Recovery using TIMESTAMP](#data-recovery-using-timestamp)  
4. [Fail Safe and UNDROP Operations](#fail-safe-and-undrop-operations)  
5. [Practical Scenarios](#practical-scenarios)
6. [Best Practices](#best-practices)  
7. [References](#references)  

---

## Introduction

Snowflake's **Time Travel** feature allows querying and recovering historical data at any point within the defined retention period. This enables:

- Recovery from accidental DML operations (`INSERT`, `UPDATE`, `DELETE`, `MERGE`).
- Auditing and analysis of past data.
- Cloning and testing without affecting current production data.

Time Travel retention defaults to **1 day (24 hours)** and can be extended up to **90 days** in Enterprise accounts.

---

## Time Travel using Offset and Query ID Features

### Using OFFSET

- **OFFSET** specifies the number of seconds before the current time to access historical data.
- Useful for recent changes where exact timestamps are unknown.

**Syntax:**
```sql
SELECT * FROM <table_name>
   AT (OFFSET => -<seconds>);
```

**Example:**
```sql
-- Query the table state 2 hours ago
SELECT * FROM customer_data
   AT (OFFSET => -7200);  -- 7200 seconds = 2 hours
```

**Notes:**
- Only works within the Time Travel retention period.
- Can be used for tables, schemas, and databases.

### Using Query ID

- Allows querying the state of a table immediately after a specific query executed.
- Helps in **auditing** and **reverting specific operations**.

**Steps:**
1. Retrieve the query ID from the **History** tab or `QUERY_HISTORY` function.
2. Reference the table state using `AT (STATEMENT => '<query_id>')`.

**Example:**
```sql
-- Get state of table after a specific query executed
SELECT * FROM customer_data
   AT (STATEMENT => '01a2b3c4-5678-90de-f123-4567890abcde');
```

**Notes:**
- Query ID must be within the Time Travel retention period.
- Supports auditing and validation without modifying current data.

---

## Data Recovery using TIMESTAMP

- Using a **specific timestamp** allows point-in-time recovery.
- Ideal when the exact time of accidental changes is known.

**Syntax:**
```sql
SELECT * FROM <table_name>
   AT (TIMESTAMP => '<YYYY-MM-DD HH24:MI:SS>');
```

**Example:**
```sql
-- Restore table to its state at a specific datetime
CREATE OR REPLACE TABLE customer_data AS
SELECT * FROM customer_data
   AT (TIMESTAMP => '2025-10-12 14:00:00');
```

**Notes:**
- Timestamp must be within the retention period.
- Can be combined with cloning to test recovery before replacing production data.

---

## Fail Safe and UNDROP Operations

### Fail Safe

- **Fail Safe** provides an additional **7-day period** beyond Time Travel for disaster recovery.
- Managed entirely by Snowflake.
- Users cannot query Fail Safe directly; it ensures data can be recovered by Snowflake support in catastrophic failures.

### UNDROP Operation

- Allows recovery of **dropped tables, schemas, and databases** within the Time Travel period.

**Syntax:**
```sql
UNDROP TABLE <table_name>;
UNDROP SCHEMA <schema_name>;
```

**Example:**
```sql
-- Recover accidentally dropped table
UNDROP TABLE customer_data;

-- Recover accidentally dropped schema
UNDROP SCHEMA analytics;
```

**Notes:**
- UNDROP restores the object to its last consistent state.
- Cannot be used after Time Travel retention ends, Fail Safe applies afterward.

---

## Practical Scenarios

1. **Accidental DELETE**
```sql
DELETE FROM customer_data WHERE customer_id = 101;
-- Recover deleted row
SELECT * FROM customer_data AT (OFFSET => -600);
```

2. **Accidental UPDATE**
```sql
UPDATE customer_data SET email='invalid@example.com';
-- Recover previous data
CREATE TABLE customer_data_restore AS
SELECT * FROM customer_data AT (TIMESTAMP => '2025-10-12 10:30:00');
```

3. **Recover Dropped Table**
```sql
DROP TABLE customer_data;
-- Recover the table
UNDROP TABLE customer_data;
```

4. **Auditing Past Queries**
```sql
SELECT * FROM customer_data AT (STATEMENT => '02a3b4c5-6789-0def-a123-4567890bcdef');
```

---

## Best Practices

1. Always verify the **current Time Travel retention period**.
2. Use **OFFSET** for recent recoveries and **TIMESTAMP** for precise point-in-time recovery.
3. Track **Query IDs** for auditing and targeted recovery.
4. Prefer **cloning** over `CREATE OR REPLACE` when testing recovery.
5. Combine **Time Travel** and **Fail Safe** for comprehensive data protection.
6. Document recovery operations to maintain audit trail.

---

## References

- [Snowflake Time Travel Documentation](https://docs.snowflake.com/en/user-guide/data-time-travel.html)  
- [Undrop and Fail Safe](https://docs.snowflake.com/en/user-guide/data-failsafe.html)  
- [Querying Historical Data Using Offset and Query ID](https://docs.snowflake.com/en/sql-reference/sql/select.html#querying-historical-data)

