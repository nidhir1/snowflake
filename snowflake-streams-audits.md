# Snowflake Streams & Audits

## 1. Working with Snowflake Streams

### Stream Object and DML Auditing
A **Snowflake Stream** is a change data capture (CDC) mechanism that tracks DML changes (INSERT, UPDATE, DELETE) made to a table.  
It enables incremental data processing by capturing only changed rows since the last consumption.

When you query a stream, Snowflake returns the **delta**—rows inserted, updated, or deleted—since the stream’s offset.  
This is essential for **data auditing, replication, and ETL pipelines**.

**Example:**
```sql
CREATE OR REPLACE STREAM customer_stream ON TABLE customers;
SELECT * FROM customer_stream;
```

### Stream Types
Snowflake supports different types of streams to suit various use cases:

1. **Standard Stream**  
   - Captures all DML changes (INSERT, UPDATE, DELETE).  
   - Provides metadata columns like `METADATA$ACTION`, `METADATA$ISUPDATE`, and `METADATA$ROW_ID`.

2. **Append-Only Stream**  
   - Tracks only **inserts** to the source table.  
   - Useful for systems where deletes and updates are not required.

3. **Insert-Only Stream**  
   - Captures only new rows added (like append-only), ignoring updates or deletes.  
   - Common in ingestion pipelines and event-driven architectures.

### Data Flow with Snowflake Streams
Typical flow for using streams in ETL/Audit pipelines:

1. Source table receives DML operations.  
2. A **stream** is defined on this source table to track changes.  
3. Downstream ETL process reads from the stream and loads deltas into a target table.  
4. Once consumed, the stream offset advances automatically.

**Example Flow:**
```sql
-- Step 1: Create table
CREATE OR REPLACE TABLE orders (order_id INT, status STRING);

-- Step 2: Create stream on table
CREATE OR REPLACE STREAM order_stream ON TABLE orders;

-- Step 3: Insert new data
INSERT INTO orders VALUES (1, 'Pending'), (2, 'Confirmed');

-- Step 4: Query stream for captured changes
SELECT * FROM order_stream;

-- Step 5: Consume stream and insert into audit log
INSERT INTO order_audit_log SELECT *, CURRENT_TIMESTAMP() FROM order_stream;
```

---

## 2. Time Travel with Streams

### Using Stream Tables
Streams leverage Snowflake’s **Time Travel** feature to track table versions.  
The stream maintains an **offset** relative to the table’s change history, so it can efficiently detect changes since the last read.

**Example:**
```sql
-- Check table history with Time Travel
SELECT * FROM orders AT (OFFSET => -60*5); -- 5 minutes ago
```

When a stream reads changes, it uses this versioning to identify what has changed since the previous offset.

### Auditing INSERT, UPDATE, DELETE
To audit all DML activity using streams:

```sql
CREATE OR REPLACE TABLE order_audit AS
SELECT 
    METADATA$ACTION AS action_type,
    METADATA$ISUPDATE AS is_update,
    METADATA$ROW_ID AS row_id,
    o.*,
    CURRENT_TIMESTAMP() AS audit_time
FROM order_stream o;
```

**Explanation:**
- `METADATA$ACTION` → Shows action type (`INSERT`, `DELETE`).  
- `METADATA$ISUPDATE` → Indicates if the record is part of an update.  
- `METADATA$ROW_ID` → Provides a unique identifier for change tracking.  
- Useful for **change audit tables**, **incremental loading**, and **data reconciliation**.

---

## Summary

| Concept | Description |
|----------|--------------|
| **Stream Object** | Tracks DML changes for auditing and ETL |
| **Standard Stream** | Captures all DML changes |
| **Append-Only / Insert-Only** | Tracks only inserts (no updates or deletes) |
| **Time Travel** | Maintains table version history for delta detection |
| **Audit Log Example** | Use streams to build insert/update/delete history tables |

---

**References:**
- [Snowflake Streams Documentation](https://docs.snowflake.com/en/user-guide/streams)
- [Snowflake Time Travel](https://docs.snowflake.com/en/user-guide/data-time-travel)
