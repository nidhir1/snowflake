# SnowSQL and Scripting

## 1. SnowSQL Concepts and Client Installation

### SnowSQL: Configuration Options
**SnowSQL** is Snowflake’s command-line client that enables users to execute SQL statements, manage sessions, and automate operations.  
It can be used for both **interactive querying** and **batch scripting**.

**Installation Steps:**
1. Download from: [https://docs.snowflake.com/en/user-guide/snowsql-install-config](https://docs.snowflake.com/en/user-guide/snowsql-install-config)
2. Configure connection using account and user credentials.

**Configuration Example (`~/.snowsql/config`):**
```ini
[connections.myaccount]
accountname = xy12345.us-east-1
username = user_name
password = my_password
warehouse = compute_wh
database = sales_db
schema = public
role = sysadmin
```
You can then connect directly using:
```bash
snowsql -a xy12345 -u user_name
```

### Account Authorization
SnowSQL uses Snowflake’s **OAuth 2.0**, **SSO**, or **Key Pair Authentication** for secure access.

**Example using Key Pair Auth:**
```ini
[connections.secure_conn]
accountname = xy12345.us-east-1
username = secure_user
private_key_file = /Users/secure_user/rsa_key.p8
```
Then connect via:
```bash
snowsql -c secure_conn
```

---

## 2. Working with SnowSQL

### DDL, DML, and SELECT Operations
You can run SQL commands interactively or in scripts.

**Example:**
```sql
CREATE OR REPLACE TABLE employee (id INT, name STRING, salary FLOAT);
INSERT INTO employee VALUES (1, 'John', 55000), (2, 'Emma', 72000);
SELECT * FROM employee;
```

**Command-Line Usage:**
```bash
snowsql -q "SELECT COUNT(*) FROM employee;"
```

### Snowflake Variables and Batch Processing
SnowSQL supports variable substitution and batch processing using the `!set` command.

**Example:**
```bash
!set region='US_EAST'
SELECT * FROM sales WHERE region = $region;
```

You can execute multiple commands from a file:
```bash
snowsql -f load_data.sql
```
Where `load_data.sql` contains:
```sql
USE DATABASE sales_db;
INSERT INTO orders VALUES (101, 'Widget', 500);
SELECT COUNT(*) FROM orders;
```

### Snowflake SQL Query Syntax Format
Snowflake supports ANSI SQL with extensions for cloud-specific functionality (e.g., `COPY INTO`, `STREAM`, `TASK`).

**Best Practices:**
- Always terminate commands with semicolons `;`
- Use uppercase for SQL keywords (`SELECT`, `INSERT`, etc.)
- Use `!help` within SnowSQL for available commands and parameters

**Example Script Format:**
```sql
-- Script: daily_refresh.sql
USE WAREHOUSE etl_wh;
USE DATABASE finance_db;

BEGIN TRANSACTION;
DELETE FROM temp_table WHERE load_date < CURRENT_DATE() - 30;
INSERT INTO temp_table SELECT * FROM staging_table;
COMMIT;
```

**Execution:**
```bash
snowsql -f daily_refresh.sql -o output_file=refresh_log.txt
```

---

## Summary

| Concept | Description |
|----------|--------------|
| **SnowSQL** | Command-line client for Snowflake operations |
| **Configuration File** | Stores credentials and session defaults |
| **Authentication** | OAuth, SSO, or Key Pair authentication |
| **DDL/DML** | Supports full SQL execution from CLI or scripts |
| **Variables** | Dynamic parameter substitution for queries |
| **Batch Scripts** | Automate ETL or maintenance tasks |

---

**References:**
- [SnowSQL Documentation](https://docs.snowflake.com/en/user-guide/snowsql)
- [Snowflake Authentication Options](https://docs.snowflake.com/en/user-guide/key-pair-auth)
- [Snowflake SQL Reference](https://docs.snowflake.com/en/sql-reference)
