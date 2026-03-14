# ❄️ Snowflake --- RBAC (Role‑Based Access Control)



------------------------------------------------------------------------

# 1️⃣ RBAC FUNDAMENTALS

### Ques: What is RBAC?

**Ans:**\
RBAC (Role‑Based Access Control) is a security model where **permissions
are assigned to roles instead of individual users**.\
Users are then assigned roles that contain those permissions.

Access flow:

User → Role → Privileges → Objects

Example:

User: Alice\
Role: ANALYST_ROLE\
Privileges: SELECT on sales tables

Alice can query the sales tables because the **ANALYST_ROLE has SELECT
privileges**.

Benefits:

-   Centralized security management
-   Easier auditing
-   Reduced privilege duplication

------------------------------------------------------------------------

### Ques: Why does Snowflake use RBAC instead of user-based security?

**Ans:**\
User-based access does not scale well.

Imagine:

-   500 users\
-   2000 tables

Granting permissions individually would require thousands of statements.

Instead:

    GRANT SELECT ON TABLE sales TO ROLE analyst_role;
    GRANT ROLE analyst_role TO USER alice;

Now every analyst receives the same privileges automatically.

------------------------------------------------------------------------

### Ques: What problem does RBAC solve in large organizations?

**Ans:**

RBAC solves several governance challenges:

1.  **Scalability** -- Manage access through roles rather than
    individuals.\
2.  **Consistency** -- Everyone with the same role has identical
    permissions.\
3.  **Security** -- Least‑privilege policies are easier to enforce.\
4.  **Auditing** -- Easier to see which roles have access to which data.

Typical enterprise roles:

-   DATA_ENGINEER
-   DATA_ANALYST
-   BI_USER
-   SECURITY_ADMIN

------------------------------------------------------------------------

### Ques: What is the difference between a role and a user?

**Ans:**

User: - Represents a person or application that logs into Snowflake.

Role: - A container of privileges.

Example:

User: Ravi\
Role: DATA_ANALYST

Privileges attached to DATA_ANALYST determine what Ravi can do.

------------------------------------------------------------------------

### Ques: What is the principle of least privilege?

**Ans:**

The **principle of least privilege** means giving users only the
permissions they absolutely need.

Example:

An analyst may need:

    SELECT on reporting tables

But should not receive:

    DROP TABLE
    ALTER DATABASE
    ACCOUNTADMIN

This prevents accidental damage or unauthorized access.

------------------------------------------------------------------------

### Ques: Why is least privilege critical in data platforms?

**Ans:**

Data platforms store sensitive information such as:

-   customer records
-   financial data
-   personal identifiers

Too much access can cause:

-   accidental deletion
-   data leaks
-   compliance violations

Least privilege protects both **data integrity and regulatory
compliance**.

------------------------------------------------------------------------

# 2️⃣ ROLE HIERARCHY & DEPENDENCY

### Ques: What is a role hierarchy?

**Ans:**

Snowflake allows roles to **inherit privileges from other roles**.

Example:

    ANALYST_ROLE
          ↑
    DATA_TEAM_ROLE
          ↑
    ENGINEERING_ROLE

ENGINEERING_ROLE automatically receives privileges from DATA_TEAM_ROLE
and ANALYST_ROLE.

------------------------------------------------------------------------

### Ques: What does it mean when one role inherits another role?

**Ans:**

Inheritance occurs when a role is granted to another role.

Example:

    GRANT ROLE analyst_role TO ROLE reporting_role;

Now `reporting_role` inherits all privileges assigned to `analyst_role`.

------------------------------------------------------------------------

### Ques: How does inheritance work in Snowflake roles?

**Ans:**

Privileges flow **up the hierarchy**.

Example:

    GRANT SELECT ON TABLE sales TO ROLE analyst_role;
    GRANT ROLE analyst_role TO ROLE reporting_role;
    GRANT ROLE reporting_role TO USER Ravi;

Ravi can now query the sales table even though the privilege originated
in analyst_role.

------------------------------------------------------------------------

### Ques: What is a parent role?

**Ans:**

A **parent role** is the role that inherits privileges from another
role.

Example:

    GRANT ROLE analyst_role TO ROLE reporting_role;

`reporting_role` becomes the parent role.

------------------------------------------------------------------------

### Ques: What is a child role?

**Ans:**

A **child role** provides privileges to another role.

In the previous example:

`analyst_role` is the child role.

------------------------------------------------------------------------

### Ques: Does privilege flow from parent → child or child → parent?

**Ans:**

Privileges flow:

child role → parent role → user

This means higher-level roles accumulate privileges from lower roles.

------------------------------------------------------------------------

### Ques: Why is role hierarchy useful?

**Ans:**

Hierarchy simplifies privilege management.

Instead of granting privileges repeatedly:

    GRANT SELECT ON TABLE sales TO ROLE analyst_role;

All higher roles automatically inherit that privilege.

Benefits:

-   simpler administration
-   fewer grant statements
-   cleaner security architecture

------------------------------------------------------------------------

### Ques: What is dependency chaining?

**Ans:**

Dependency chaining occurs when multiple inheritance levels exist.

Example:

    Role A → Role B → Role C

Role C inherits privileges from both B and A.

------------------------------------------------------------------------

### Ques: What happens when you drop a role that others depend on?

**Ans:**

If a role is dropped:

-   dependent roles lose inherited privileges
-   user access may break

Administrators should review dependencies before removing roles.

------------------------------------------------------------------------

### Ques: Can a role belong to multiple parent roles?

**Ans:**

Yes.

Example:

    GRANT ROLE analyst_role TO ROLE reporting_role;
    GRANT ROLE analyst_role TO ROLE finance_role;

Both parent roles inherit privileges from `analyst_role`.

------------------------------------------------------------------------

### Ques: Can circular dependencies exist?

**Ans:**

No.

Snowflake prevents structures like:

    Role A → Role B → Role A

Circular dependencies would create infinite privilege loops.

------------------------------------------------------------------------

# 3️⃣ WORKING WITH ROLES IN RBAC

### Ques: Why assign privileges to roles instead of users?

**Ans:**

Assigning privileges to roles ensures:

-   easier maintenance
-   consistent permissions
-   faster onboarding/offboarding

When a user joins or leaves the organization, administrators simply
assign or revoke roles.

------------------------------------------------------------------------

### Ques: Why shouldn't developers use ACCOUNTADMIN?

**Ans:**

ACCOUNTADMIN has full administrative power including:

-   creating databases
-   managing users
-   altering system configuration

Using this role for daily work increases the risk of accidental changes.

Instead, developers should use **least‑privileged functional roles**.

------------------------------------------------------------------------

# 4️⃣ AUDITING USERS & ACTIVITY

### Ques: What is auditing?

**Ans:**

Auditing tracks system activity to ensure:

-   security compliance
-   accountability
-   monitoring of suspicious activity

Snowflake provides several system views for auditing.

------------------------------------------------------------------------

### Ques: What is QUERY_HISTORY?

**Ans:**

QUERY_HISTORY records details about executed queries including:

-   query text
-   user
-   execution time
-   warehouse used

This helps administrators analyze system activity.

------------------------------------------------------------------------

### Ques: What is LOGIN_HISTORY?

**Ans:**

LOGIN_HISTORY records authentication attempts including:

-   login timestamp
-   user
-   IP address
-   success or failure

This helps detect unauthorized access attempts.

------------------------------------------------------------------------

# 5️⃣ PASSWORD & SECURITY POLICIES

### Ques: What is a password policy?

**Ans:**

A password policy defines rules controlling password security such as:

-   minimum length
-   complexity requirements
-   expiration period
-   account lockout rules

These policies protect user accounts from weak credentials.

------------------------------------------------------------------------

### Ques: What is MFA (Multi‑Factor Authentication)?

**Ans:**

MFA requires users to provide multiple authentication factors when
logging in.

Example:

Password + mobile verification code

This significantly improves account security.

------------------------------------------------------------------------

### Ques: What is a network policy?

**Ans:**

A network policy restricts which IP addresses can access Snowflake.

Organizations can allow only trusted networks to connect to their
Snowflake environment.

------------------------------------------------------------------------

# 6️⃣ TRICK / MISCONCEPTION QUESTIONS

### Ques: Does having a role assigned mean it is always active?

**Ans:**

No.

A user may have multiple roles but only **one active role at a time**
unless secondary roles are enabled.

------------------------------------------------------------------------

### Ques: Does audit history contain query results?

**Ans:**

No.

Audit views store metadata such as query text and user information, but
not the returned data.

------------------------------------------------------------------------

### Ques: Are password policies automatically applied to all users?

**Ans:**

No.

Password policies must be explicitly assigned to users or roles.
