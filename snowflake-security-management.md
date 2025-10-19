# 🛡️ Snowflake Security Management

## 1️⃣ Security Management Concepts

### 🔹 Security Entities in Snowflake
Snowflake’s security model revolves around four core entities:
- **Users** → Represent individual human or service accounts that connect to Snowflake.  
- **Roles** → Define sets of privileges. Each user is assigned one or more roles.  
- **Privileges** → Specific permissions (e.g., `SELECT`, `USAGE`, `CREATE TABLE`) assigned to roles.  
- **Securable Objects** → Any object that can be accessed or managed (e.g., databases, schemas, tables, warehouses).

---

### 🔹 Securable Objects, Users, and Roles
Every object in Snowflake is *securable*, including:
- **Global**: Account, Warehouses
- **Database-level**: Database, Schema, Table, View, Stage, File Format, etc.

Users and roles interact via **role-based grants**:
- You *create users* → assign *roles* → which grant *privileges* → on *securable objects.*

```sql
CREATE USER analyst PASSWORD='StrongPass#2025' DEFAULT_ROLE=ANALYST;
CREATE ROLE ANALYST;
GRANT USAGE ON DATABASE SALES_DB TO ROLE ANALYST;
GRANT ROLE ANALYST TO USER analyst;
```

---

### 🔹 Privileges and Privilege Groups
Privileges define what a role can do. Examples:

| Privilege | Object | Description |
|------------|---------|-------------|
| `USAGE` | Database, Schema, Warehouse | Enables visibility and access |
| `SELECT` | Table, View | Allows reading data |
| `INSERT`, `UPDATE`, `DELETE` | Table | Enables data modification |
| `OWNERSHIP` | All | Full control including grant/revoke rights |

Privileges are **hierarchical** — granting at a higher level (e.g., schema) cascades visibility to child objects.

---

### 🔹 Snowflake Security Hierarchy
Snowflake implements a **top-down security hierarchy**:
```
ACCOUNT
 ├── DATABASE
 │    ├── SCHEMA
 │    │    ├── TABLE
 │    │    ├── VIEW
 │    │    └── STAGE
 │    └── FILE FORMAT
 └── WAREHOUSE
```
Privileges can be inherited downwards in this hierarchy.  
For instance, granting `USAGE` on a schema allows access to objects within it.

---

### 🔹 Creating and Using Roles & Users
Snowflake follows the **Principle of Least Privilege (PoLP)** — each user or service should have only the access necessary to perform their function.

```sql
-- 1. Create custom roles
CREATE ROLE DATA_ENGINEER;
CREATE ROLE DATA_ANALYST;

-- 2. Create users
CREATE USER de_user PASSWORD='Secure123#';
CREATE USER da_user PASSWORD='Secure123#';

-- 3. Grant roles to users
GRANT ROLE DATA_ENGINEER TO USER de_user;
GRANT ROLE DATA_ANALYST TO USER da_user;

-- 4. Assign object privileges
GRANT USAGE ON WAREHOUSE COMPUTE_WH TO ROLE DATA_ANALYST;
GRANT SELECT ON DATABASE SALES_DB TO ROLE DATA_ANALYST;
```

---

## 2️⃣ Role-Based Access Control (RBAC)

### 🔹 RBAC Overview
Snowflake’s **Role-Based Access Control** model provides a flexible mechanism for managing permissions.

- **Roles** act as containers for privileges.
- **Users** are assigned one or more roles.
- Roles can be **nested**, creating a **role hierarchy**.

---

### 🔹 Role Hierarchy and Dependency

Example:
```
SYSADMIN
   ├── DATA_ENGINEER
   │      └── ETL_ROLE
   └── DATA_ANALYST
          └── REPORTING_ROLE
```
- If `ETL_ROLE` has `SELECT` on a schema, and `DATA_ENGINEER` inherits it, users with `DATA_ENGINEER` get access too.
- The `SECURITYADMIN` role manages roles and grants.
- The `SYSADMIN` role manages objects and resources.

**System Roles in Snowflake:**

| Role | Description |
|------|--------------|
| `ACCOUNTADMIN` | Highest-level role, full control of account |
| `SECURITYADMIN` | Manages users, roles, and access |
| `SYSADMIN` | Manages warehouses and databases |
| `USERADMIN` | Manages users only |
| `PUBLIC` | Default role for all users (minimal privileges) |

---

### 🔹 Auditing Users and Password Policy

Snowflake provides **comprehensive auditing** through the **ACCOUNT_USAGE** and **ORGANIZATION_USAGE** views.

| View | Description |
|------|--------------|
| `SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY` | Records all login attempts |
| `SNOWFLAKE.ACCOUNT_USAGE.SESSIONS` | Tracks session details |
| `SNOWFLAKE.ACCOUNT_USAGE.GRANTS_TO_USERS` | Shows which roles are assigned to users |
| `SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY` | Tracks all queries executed |

```sql
SELECT USER_NAME, EVENT_TIMESTAMP, IS_SUCCESS
FROM SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY
WHERE EVENT_TIMESTAMP > DATEADD('day', -7, CURRENT_TIMESTAMP());
```

#### Password Policies
- You can define **password complexity**, **expiry**, and **lockout policies**.
- Managed via `CREATE PASSWORD POLICY`.

```sql
CREATE PASSWORD POLICY strong_policy
  PASSWORD_MIN_LENGTH = 12
  PASSWORD_MAX_AGE_DAYS = 90
  PASSWORD_REQUIRE_UPPER_CASE = TRUE
  PASSWORD_REQUIRE_SPECIAL_CHAR = TRUE;

ALTER USER analyst SET PASSWORD_POLICY = strong_policy;
```

---

## ✅ Best Practices Summary

| Area | Best Practice |
|------|----------------|
| Role Management | Create hierarchical roles per business function |
| Privilege Assignment | Grant privileges only at necessary levels |
| User Auditing | Use `ACCOUNT_USAGE` views for continuous monitoring |
| Password Policy | Enforce strong password rules |
| Access Review | Periodically review user-role assignments |
| Separation of Duties | Distinguish between admin and operational roles |
