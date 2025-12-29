# Snowflake Procedures & Views

## Table of Contents

1.  [Introduction](#introduction)\
2.  [Creating and Using Stored
    Procedures](#creating-and-using-stored-procedures)\
3.  [Using CALL Command](#using-call-command)\
4.  [Views in Snowflake](#views-in-snowflake)\
5.  [System Views & Query Storage](#system-views--query-storage)\
6.  [Best Practices](#best-practices)

------------------------------------------------------------------------

## Introduction

Snowflake allows you to build reusable logic and reusable query layers
using **Stored Procedures** and **Views**.

-   **Stored Procedures**: encapsulate business logic and automation.
-   **Views**: provide logical representation of data without
    duplicating storage.
-   **System Views**: provide metadata on performance, queries, roles,
    objects, and storage.

Together, they help organize, secure, and standardize analytics across
Snowflake.

------------------------------------------------------------------------

## Creating and Using Stored Procedures

### What is a Stored Procedure?

A stored procedure is a programmatic block (JavaScript or SQL procedural
API) that:

-   Executes one or more SQL statements
-   Supports conditional logic and loops
-   Can return results or messages
-   Can automate tasks (loading data, transformations, auditing,
    cleanup, etc.)

### Syntax (JavaScript Procedure)

``` sql
CREATE OR REPLACE PROCEDURE my_demo_proc()
RETURNS STRING
LANGUAGE JAVASCRIPT
AS
$$
    var result = snowflake.execute(
        { sqlText: "SELECT COUNT(*) FROM orders;" }
    );
    result.next();
    return "Total rows: " + result.getColumnValue(1);
$$;
```

### Execute Procedure

``` sql
CALL my_demo_proc();
```

### Procedure with Parameters

``` sql
CREATE OR REPLACE PROCEDURE update_status(order_id INT, status STRING)
RETURNS STRING
LANGUAGE SQL
AS
$$
    UPDATE orders SET status = status WHERE id = order_id;
    SELECT 'Updated successfully';
$$;
```

Run:

``` sql
CALL update_status(101, 'SHIPPED');
```

------------------------------------------------------------------------

## Using CALL Command

The **CALL** command is used only for Stored Procedures.

``` sql
CALL procedure_name(parameter1, parameter2, ...);
```

Example:

``` sql
CALL load_daily_sales();
```

CALL can be used in worksheets, tasks, pipelines, and automation
workflows.

------------------------------------------------------------------------

## Views in Snowflake

### What is a View?

A view is a **saved SQL query** that behaves like a table but:

-   Does NOT store physical data (except materialized views)
-   Always pulls fresh results from base tables
-   Controls access to specific columns or joins

### Regular View

``` sql
CREATE OR REPLACE VIEW sales_view AS
SELECT order_id, amount, region
FROM sales
WHERE status = 'COMPLETED';
```

Query like a table:

``` sql
SELECT * FROM sales_view;
```

### Secure Views

``` sql
CREATE OR REPLACE SECURE VIEW customer_secure_view AS
SELECT id, name, email
FROM customers;
```

### Materialized Views

``` sql
CREATE MATERIALIZED VIEW mv_sales AS
SELECT region, SUM(amount) AS total_sales
FROM sales
GROUP BY region;
```

------------------------------------------------------------------------

## System Views & Query Storage

Snowflake provides system views under:

    INFORMATION_SCHEMA
    ACCOUNT_USAGE
    ORGANIZATION_USAGE

Examples:

**Query History**

``` sql
SELECT *
FROM SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
ORDER BY START_TIME DESC;
```

**Storage Metrics**

``` sql
SELECT *
FROM SNOWFLAKE.ACCOUNT_USAGE.TABLE_STORAGE_METRICS;
```

**List Views**

``` sql
SELECT *
FROM INFORMATION_SCHEMA.VIEWS;
```

------------------------------------------------------------------------

## Best Practices

1.  Use stored procedures for automation and orchestration.
2.  Use SECURE views for sensitive data.
3.  Use materialized views only for heavy, repeated queries.
4.  Monitor usage via ACCOUNT_USAGE views.
5.  Apply roles and privileges carefully.
6.  Maintain consistent naming conventions across environments.

------------------------------------------------------------------------

## References

-   Snowflake Stored Procedures Docs\
-   Snowflake Views Docs\
-   Snowflake Account Usage Docs
