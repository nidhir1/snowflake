# Snowflake Schemas Tutorial

## Table of Contents
1. [Introduction](#introduction)  
2. [What is a Schema in Snowflake?](#what-is-a-schema-in-snowflake)  
3. [Creating Schemas](#creating-schemas)  
    - [Permanent Schemas](#permanent-schemas)  
    - [Transient Schemas](#transient-schemas)  
4. [Real-time Usage of Schemas](#real-time-usage-of-schemas)  
5. [Managed Schemas](#managed-schemas)  
6. [Best Practices](#best-practices)  

---

## Introduction

In Snowflake, a **schema** is a logical grouping of database objects such as tables, views, sequences, and stages. It helps organize and manage data efficiently. Understanding schema types and usage is essential for data engineering and analytics workflows.

---

## What is a Schema in Snowflake?

- **Definition**: A schema is a container within a database that holds tables, views, and other objects.  
- **Purpose**:
  - Organize database objects logically.
  - Apply access control (role-based security).  
  - Enable separation of production, staging, and development environments.  

**Basic Hierarchy**:  
```
Account > Database > Schema > Table/View/Other Objects
```

---

## Creating Schemas

### Syntax

```sql
-- Create a new schema
CREATE SCHEMA <schema_name>;
```

**Example**:

```sql
CREATE SCHEMA analytics;
```

### Using a Schema

```sql
-- Set the active schema
USE SCHEMA analytics;

-- Now all objects will be created in this schema
CREATE TABLE customer_data (
    customer_id INT,
    name STRING,
    email STRING
);
```

---

### Permanent Schemas

- **Default type** in Snowflake.  
- Objects inside are fully recoverable using **Time Travel** and **Fail-safe** features.  
- Useful for critical production data.  

**Creation**:

```sql
CREATE SCHEMA permanent_schema;
```

**Key Features**:
- Supports Time Travel (can query historical versions).  
- Supports Fail-safe (Snowflake-managed recovery).  
- Suitable for long-term storage.

---

### Transient Schemas

- Used for **temporary or intermediate data**.  
- Do **not** support Fail-safe (recovery only via Time Travel).  
- Ideal for staging, ETL pipelines, or temporary calculations.

**Creation**:

```sql
CREATE TRANSIENT SCHEMA transient_schema;
```

**Key Features**:
- Faster and cheaper than permanent schemas for non-critical data.  
- Retains Time Travel support but no Fail-safe.  

---

## Real-time Usage of Schemas

Schemas are actively used in:

1. **ETL/ELT Pipelines**
   - Staging tables are created in transient schemas.
   - Permanent schemas hold curated tables for analytics.

2. **Data Sharing**
   - Schemas can be shared with other Snowflake accounts using **Secure Data Sharing**.

3. **Access Control**
   - Roles and privileges are assigned at the schema level.
   
**Example**:

```sql
-- Grant role access to schema
GRANT USAGE ON SCHEMA analytics TO ROLE analyst;

-- Grant select privileges on all tables in schema
GRANT SELECT ON ALL TABLES IN SCHEMA analytics TO ROLE analyst;
```

---

## Managed Schemas

- Snowflake supports **managed schemas** via **Snowflake Managed Storage**.  
- All underlying storage, metadata, and maintenance is fully managed by Snowflake.  
- Ensures:
  - Automated optimization
  - Consistent metadata
  - Easy scaling  

**Benefits**:
- Users focus on **data modeling and analytics** rather than storage management.  
- Compatible with both permanent and transient schemas.  

**Usage Example**:

```sql
-- Managed schema behaves same as regular schema
CREATE SCHEMA managed_schema;

-- Creating a table inside managed schema
CREATE TABLE managed_schema.orders (
    order_id INT,
    customer_id INT,
    order_total FLOAT
);
```

---

## Best Practices

1. **Separate Environments**:
   - `dev`, `staging`, `prod` schemas for proper lifecycle management.
2. **Use Transient Schemas**:
   - For ETL pipelines or intermediate datasets to save costs.
3. **Grant Minimal Privileges**:
   - Follow the principle of least privilege at the schema level.
4. **Organize by Business Domain**:
   - e.g., `sales`, `marketing`, `hr` schemas.
5. **Monitor Storage and Metadata Usage**:
   - Use `SHOW SCHEMAS` and `DESCRIBE SCHEMA <name>` to review schemas.

---

## References

- [Snowflake Schema Documentation](https://docs.snowflake.com/en/sql-reference/sql/create-schema.html)  
- [Time Travel & Transient Objects](https://docs.snowflake.com/en/user-guide/data-time-travel.html)

