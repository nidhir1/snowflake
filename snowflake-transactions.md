# Snowflake Transactions

## 1. Working with Transactions (ACID)

### ACID Properties
Snowflake supports full **ACID compliance**, ensuring reliable transaction processing:

- **Atomicity** – All operations within a transaction succeed or none do.  
- **Consistency** – Ensures the database remains in a valid state before and after a transaction.  
- **Isolation** – Transactions operate independently without interference.  
- **Durability** – Once committed, changes are permanently recorded and recoverable.

### Transaction Types in Snowflake
Snowflake supports the following transaction modes:

1. **Implicit Transactions**  
   - Default behavior when executing individual DML statements.  
   - Each statement (e.g., `INSERT`, `UPDATE`, `DELETE`) runs in its own transaction and commits automatically.  

2. **Explicit Transactions**  
   - Initiated using `BEGIN TRANSACTION` or `BEGIN`.  
   - Multiple SQL statements can be grouped together and committed or rolled back as a single unit.  

3. **Auto Commit Mode**  
   - Controlled at the session level using `AUTOCOMMIT` parameter (`TRUE` or `FALSE`).  
   - When `TRUE`, every statement automatically commits upon successful execution.

### DDL Statements and Transactions
- DDL statements like `CREATE TABLE`, `ALTER TABLE`, and `DROP TABLE` are **auto-committed** in Snowflake.  
- These statements cause an implicit commit before and after execution, which means they **cannot be rolled back** as part of a transaction.

---

## 2. Using Transactions

### BEGIN TRANSACTION & COMMIT
Explicit transaction management commands:

```sql
BEGIN TRANSACTION;
    INSERT INTO customers VALUES (101, 'John Doe');
    UPDATE orders SET status = 'Shipped' WHERE order_id = 12345;
COMMIT;
```

If any error occurs before `COMMIT`, you can issue a `ROLLBACK`:

```sql
ROLLBACK;
```

### current_transaction() and Usage
The **`current_transaction()`** function returns the active transaction ID for the session.  
Useful for debugging and tracking transaction scopes:

```sql
SELECT current_transaction();
```

If no explicit transaction is open, it returns `NULL`.

### Failed Transactions with Stored Procedures
When transactions are used inside **Stored Procedures**:

- If a DML statement fails, Snowflake automatically **rolls back** the entire transaction unless exception handling logic is implemented.  
- For JavaScript-based stored procedures:
  - Use `snowflake.execute()` within a `try...catch` block to manage rollbacks explicitly.

**Example:**
```javascript
try {
    snowflake.execute({sqlText: "BEGIN"});
    snowflake.execute({sqlText: "INSERT INTO my_table VALUES (1, 'test')"});
    snowflake.execute({sqlText: "COMMIT"});
} catch (err) {
    snowflake.execute({sqlText: "ROLLBACK"});
    return "Transaction failed: " + err.message;
}
```

### Transactions with Stored Procedures
- Stored procedures can **span multiple statements** within a single transaction.  
- Use explicit `BEGIN` and `COMMIT` to control the scope.  
- For nested procedure calls:
  - Inner procedures cannot commit independently; they participate in the caller’s transaction.  
  - Committing or rolling back inside nested procedures may cause errors if not properly handled.

---

## Summary

| Concept | Description |
|----------|--------------|
| **ACID** | Ensures transactional integrity and reliability |
| **Implicit Transactions** | Auto-committed after each DML |
| **Explicit Transactions** | Controlled via `BEGIN` / `COMMIT` / `ROLLBACK` |
| **DDL Behavior** | Auto-committed; not part of transactional rollback |
| **Stored Procedures** | Use JavaScript error handling for transaction safety |

---

**References:**
- [Snowflake Documentation – Transactions](https://docs.snowflake.com/en/sql-reference/sql/begin)
- [Snowflake Stored Procedures](https://docs.snowflake.com/en/developer-guide/snowflake-scripting/using-transactions)
