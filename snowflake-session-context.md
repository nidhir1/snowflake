# Snowflake Session Context Tutorial

## Table of Contents
1. [Introduction](#introduction)  
2. [Snowflake Sessions](#snowflake-sessions)  
3. [Session Context](#session-context)  
    - [Role](#role)  
    - [Warehouse](#warehouse)  
    - [Database](#database)  
    - [Schema](#schema)  
4. [Working with Fully Qualified Names](#working-with-fully-qualified-names)  
5. [Best Practices](#best-practices)  

---

## Introduction

Understanding session context is crucial in Snowflake to ensure queries run in the correct environment with appropriate permissions and resources. This tutorial explains sessions, context components, and fully qualified naming.

---

## Snowflake Sessions

- A **session** in Snowflake is an interactive workspace where SQL statements are executed.
- Sessions are temporary and tied to a specific user connection.
- Each session has its own context including role, warehouse, database, and schema.

**Example: Starting a Session**
```sql
-- When connecting via Snowflake UI, Python connector, or SnowSQL, a session starts automatically
```

---

## Session Context

### Role

- Determines **permissions** and **access control** within a session.
- Users may have multiple roles; one role is active per session.

**Commands:**
```sql
-- Check current role
SELECT CURRENT_ROLE();

-- Switch role
USE ROLE analyst;
```

### Warehouse

- Defines the **compute resources** used to execute queries.
- Required for query execution and DML operations.

**Commands:**
```sql
-- Check current warehouse
SELECT CURRENT_WAREHOUSE();

-- Set active warehouse
USE WAREHOUSE analytics_wh;
```

### Database

- Defines the **current database** for the session.
- Queries without fully qualified names refer to objects in this database.

**Commands:**
```sql
-- Check current database
SELECT CURRENT_DATABASE();

-- Set active database
USE DATABASE sales_db;
```

### Schema

- Defines the **current schema** within the active database.
- Objects created or queried without a schema prefix will be assumed to exist here.

**Commands:**
```sql
-- Check current schema
SELECT CURRENT_SCHEMA();

-- Set active schema
USE SCHEMA marketing;
```

---

## Working with Fully Qualified Names

- Fully qualified names avoid ambiguity when multiple databases or schemas exist.
- Structure: **`<database>.<schema>.<object>`**

**Examples:**
```sql
-- Select from a table with fully qualified name
SELECT * FROM sales_db.public.customer_data;

-- Insert into a specific schema table
INSERT INTO analytics.orders (order_id, customer_id, order_total)
VALUES (101, 2001, 150.75);
```

- Using fully qualified names ensures your queries run correctly regardless of the current database or schema context.

---

## Best Practices

1. Always **verify session context** before running critical queries.
2. Prefer **fully qualified names** in scripts and ETL pipelines.
3. Use **roles** strategically to enforce least privilege access.
4. Assign **warehouses** based on workload to optimize costs.
5. Keep session context **consistent** across development, testing, and production.

---

## References

- [Snowflake Sessions and Context](https://docs.snowflake.com/en/user-guide/session.html)  
- [Using Fully Qualified Names](https://docs.snowflake.com/en/sql-reference/identifiers.html)

