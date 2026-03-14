# ❄️ Snowflake --- Security Management & RBAC



# 🔹 1️⃣ SECURITY ENTITIES IN SNOWFLAKE --- BASICS

### Ques: What is security in Snowflake meant to protect?

**Ans:**\
Security in Snowflake protects three major aspects of a data platform:

1.  **Data confidentiality** -- ensuring only authorized users can
    access sensitive data.
2.  **Data integrity** -- preventing unauthorized modification or
    deletion of data.
3.  **Operational control** -- ensuring only approved users can manage
    resources like warehouses, schemas, and roles.

Security controls prevent data leaks, unauthorized access, and
accidental data loss.

------------------------------------------------------------------------

### Ques: What are the main security entities in Snowflake?

**Ans:**

Snowflake security is built around a few core entities:

-   Users
-   Roles
-   Privileges
-   Securable objects

These work together in the following model:

User → Role → Privileges → Securable Object

------------------------------------------------------------------------

### Ques: What is an object in Snowflake?

**Ans:**

An object is any resource stored in Snowflake.

Examples include:

-   databases
-   schemas
-   tables
-   views
-   warehouses
-   stages
-   functions
-   procedures

Objects store data or provide compute or management functionality.

------------------------------------------------------------------------

### Ques: What is a securable object?

**Ans:**

A securable object is any Snowflake object on which **access permissions
can be granted**.

Examples include:

-   database
-   schema
-   table
-   warehouse
-   view
-   stage
-   function

Privileges determine what operations users can perform on these objects.

------------------------------------------------------------------------

### Ques: What is authentication?

**Ans:**

Authentication verifies **who the user is**.

Snowflake supports multiple authentication mechanisms:

-   username and password
-   SSO (Single Sign-On)
-   OAuth
-   key-pair authentication
-   multi-factor authentication (MFA)

Authentication ensures only valid identities can log in.

------------------------------------------------------------------------

### Ques: What is authorization?

**Ans:**

Authorization determines **what a user is allowed to do**.

Once a user is authenticated, Snowflake checks:

-   active role
-   privileges assigned to that role
-   access granted on objects

This decides whether the action is allowed.

------------------------------------------------------------------------

### Ques: What is the least privilege principle?

**Ans:**

Least privilege means giving users **only the permissions they need to
perform their job**.

Example:

An analyst might receive:

SELECT on reporting tables

But should not receive:

DROP TABLE\
ALTER DATABASE\
ACCOUNTADMIN privileges

------------------------------------------------------------------------

### Ques: Why is least privilege important?

**Ans:**

Least privilege prevents:

-   accidental deletion of data
-   unauthorized access to sensitive data
-   privilege abuse

It also helps organizations meet compliance requirements such as:

-   SOC2
-   HIPAA
-   GDPR

------------------------------------------------------------------------

### Ques: What are roles in Snowflake?

**Ans:**

Roles are containers that hold privileges.

Privileges are assigned to roles, and roles are assigned to users.

Example:

Role: ANALYST_ROLE\
Privileges: SELECT on analytics tables

Users assigned this role can query those tables.

------------------------------------------------------------------------

### Ques: What are users in Snowflake?

**Ans:**

Users represent **people or applications that log into Snowflake**.

A user account stores:

-   username
-   authentication method
-   default role
-   default warehouse
-   session parameters

Users receive privileges indirectly through roles.

------------------------------------------------------------------------

# 🔹 2️⃣ SECURABLE OBJECTS, USERS & ROLES

### Ques: What are securable objects?

**Ans:**

Securable objects are Snowflake resources that can have privileges
assigned.

Access to these objects is controlled through roles.

------------------------------------------------------------------------

### Ques: Examples of securable objects in Snowflake.

**Ans:**

Common securable objects include:

-   databases
-   schemas
-   tables
-   views
-   warehouses
-   stages
-   file formats
-   functions
-   procedures

Each object type supports specific privileges.

------------------------------------------------------------------------

### Ques: Can databases be securable objects?

**Ans:**

Yes.

Privileges such as:

USAGE\
CREATE SCHEMA

can be granted on databases.

------------------------------------------------------------------------

### Ques: Can warehouses be securable objects?

**Ans:**

Yes.

Warehouses support privileges such as:

USAGE\
OPERATE\
MONITOR

These control who can run queries or manage compute resources.

------------------------------------------------------------------------

### Ques: Can schemas be securable objects?

**Ans:**

Yes.

Schema privileges include:

USAGE\
CREATE TABLE\
CREATE VIEW\
CREATE FUNCTION

These determine who can create objects inside the schema.

------------------------------------------------------------------------

### Ques: Are tables securable objects?

**Ans:**

Yes.

Table privileges include:

SELECT\
INSERT\
UPDATE\
DELETE\
TRUNCATE

These control how users interact with table data.

------------------------------------------------------------------------

### Ques: Can stored procedures be securable objects?

**Ans:**

Yes.

Privileges such as EXECUTE can be granted on stored procedures.

This controls who can run automated logic.

------------------------------------------------------------------------

### Ques: Who owns a securable object?

**Ans:**

Every object has an **owner role**.

The owner has full control over the object including:

-   granting privileges
-   modifying the object
-   transferring ownership

------------------------------------------------------------------------

### Ques: What does object ownership mean?

**Ans:**

Ownership gives a role **complete control over the object**.

Only the owning role can:

-   grant privileges to other roles
-   alter the object definition
-   transfer ownership

------------------------------------------------------------------------

### Ques: Who can change privileges on an object?

**Ans:**

Only:

-   the object owner
-   a role with the appropriate administrative privilege

can grant or revoke privileges.

------------------------------------------------------------------------

# 🔹 3️⃣ PRIVILEGES & PRIVILEGE GROUPS

### Ques: What is a privilege?

**Ans:**

A privilege is a permission that allows a role to perform a specific
action on a Snowflake object.

Example privileges:

SELECT\
INSERT\
USAGE\
CREATE TABLE

------------------------------------------------------------------------

### Ques: Difference between role and privilege.

**Ans:**

Role: A container that groups privileges.

Privilege: A specific permission on an object.

Example:

Role → ANALYST_ROLE\
Privilege → SELECT on sales table

------------------------------------------------------------------------

### Ques: What is GRANT?

**Ans:**

GRANT is a command used to **assign privileges or roles**.

Example:

GRANT SELECT ON TABLE sales TO ROLE analyst_role;

------------------------------------------------------------------------

### Ques: What is REVOKE?

**Ans:**

REVOKE removes privileges previously granted.

Example:

REVOKE SELECT ON TABLE sales FROM ROLE analyst_role;

------------------------------------------------------------------------

### Ques: Can privileges be assigned directly to users?

**Ans:**

Yes technically, but it is discouraged.

Best practice is:

Privileges → Roles → Users

This ensures consistent security management.

------------------------------------------------------------------------

### Ques: Why shouldn't privileges be assigned to users directly?

**Ans:**

Direct grants create security problems:

-   difficult to manage
-   difficult to audit
-   inconsistent permissions

Using roles centralizes access control.

------------------------------------------------------------------------

### Ques: What are account-level privileges?

**Ans:**

Account-level privileges apply to the entire Snowflake account.

Examples:

CREATE USER\
CREATE ROLE\
CREATE DATABASE

------------------------------------------------------------------------

### Ques: What are database-level privileges?

**Ans:**

These privileges control actions on databases.

Examples:

USAGE\
CREATE SCHEMA

------------------------------------------------------------------------

### Ques: What are schema-level privileges?

**Ans:**

Schema privileges control creation of objects within schemas.

Examples:

CREATE TABLE\
CREATE VIEW\
CREATE FUNCTION

------------------------------------------------------------------------

### Ques: What are object-level privileges?

**Ans:**

These privileges apply to specific objects like tables or views.

Examples:

SELECT\
INSERT\
UPDATE\
DELETE

------------------------------------------------------------------------

# 🔹 4️⃣ SNOWFLAKE SECURITY HIERARCHY

### Ques: What is Snowflake security hierarchy?

**Ans:**

Snowflake uses a layered administrative model with built-in roles.

Important system roles include:

ACCOUNTADMIN\
SECURITYADMIN\
SYSADMIN\
USERADMIN

Each role has a defined responsibility.

------------------------------------------------------------------------

### Ques: What is ACCOUNTADMIN?

**Ans:**

ACCOUNTADMIN is the highest-level administrative role.

It has full control over the entire Snowflake account including:

-   billing
-   security
-   infrastructure
-   resource management

This role should be used sparingly.

------------------------------------------------------------------------

### Ques: What is SECURITYADMIN?

**Ans:**

SECURITYADMIN manages:

-   roles
-   privilege grants
-   role hierarchy

It controls access policies across the account.

------------------------------------------------------------------------

### Ques: What is SYSADMIN?

**Ans:**

SYSADMIN typically manages:

-   databases
-   schemas
-   tables
-   warehouses

This role handles most operational tasks.

------------------------------------------------------------------------

### Ques: What is USERADMIN?

**Ans:**

USERADMIN is responsible for:

-   creating users
-   modifying user attributes
-   managing authentication settings

------------------------------------------------------------------------

### Ques: Why should ACCOUNTADMIN be rarely used?

**Ans:**

Because it has unrestricted privileges.

Using it regularly increases risk of:

-   accidental configuration changes
-   privilege abuse
-   security breaches

------------------------------------------------------------------------

# 🔹 7️⃣ SCENARIO-BASED QUESTIONS

### Ques: A developer needs read-only access to a database --- what do you create?

**Ans:**

Create a role with SELECT privileges.

Example approach:

1.  Create role READ_ONLY_ROLE\
2.  Grant SELECT on required tables\
3.  Assign role to developer

------------------------------------------------------------------------

### Ques: A user cannot query a table even though they have role --- what check?

**Ans:**

Check:

-   current active role
-   USAGE privilege on database and schema
-   SELECT privilege on table

All three must be granted.

------------------------------------------------------------------------

### Ques: A junior engineer was given ACCOUNTADMIN --- risk and fix?

**Ans:**

Risk:

Full administrative control of Snowflake.

Fix:

Remove ACCOUNTADMIN and assign a least-privilege role instead.

------------------------------------------------------------------------

# 🔹 8️⃣ TRICK / MISCONCEPTION QUESTIONS

### Ques: Does granting SELECT automatically allow INSERT?

**Ans:**

No.

Each privilege must be granted explicitly.

------------------------------------------------------------------------

### Ques: Does granting USAGE on database allow querying tables inside?

**Ans:**

No.

USAGE allows access to the database structure, but SELECT must be
granted on tables.

------------------------------------------------------------------------

### Ques: Does dropping a user remove their objects automatically?

**Ans:**

No.

Objects owned by the user remain but ownership may need to be
transferred.
