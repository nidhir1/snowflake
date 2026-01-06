
# ❄️ Snowflake — Security Management & RBAC
## Deep Teacher-Style Q&A (Fully Expanded)

> These notes are meant to *teach*, not just memorize.  
> Each section explains **what, why, and real project impact**.

---

## 🔹 1️⃣ SECURITY ENTITIES IN SNOWFLAKE — BASICS

### ❓ What is security in Snowflake meant to protect?
Snowflake security protects three core dimensions:

1️⃣ **Data confidentiality** — only the right people see the right data.  
2️⃣ **Data integrity** — nobody can accidentally corrupt, drop, or change data.  
3️⃣ **Controlled actions** — every access is audited and tied to a user + role.

In short:

> Security ensures that data is safe, intentional, auditable, and governed.

---

### ❓ What are the main security entities in Snowflake?
Snowflake security is built around:

- **Users** → identities that sign in
- **Roles** → permission containers
- **Privileges** → allowed actions
- **Securable objects** → things that are protected (tables, DBs, warehouses, etc.)

These work together so administrators can control access without chaos.

---

### ❓ What is a securable object?
A securable object is anything in Snowflake that requires **explicit permission** to see or use.

Examples: database, schema, table, stage, warehouse, view, task, pipe, UDF, stream.

When something is securable, Snowflake tracks:

- who owns it
- who can read/write it
- who can grant access

---

### ❓ What is authentication?
Authentication answers one question:

> **“Who are you?”**

Snowflake supports:

- Passwords
- MFA (mobile approvals, tokens)
- Key-pair authentication
- SSO / SAML
- OAuth

Authentication ensures the logged-in identity is legitimate.

---

### ❓ What is authorization?
Authorization answers:

> **“What are you allowed to do?”**

You are authorized through:

- assigned roles
- privileges granted to those roles

A user without the right role simply cannot access the object.

---

### ❓ Why is least privilege important?
Least privilege means granting **only what is necessary**.

If a role only needs SELECT, don’t grant INSERT or OWNERSHIP.

Benefits:

✔ limits accidental damage  
✔ prevents data exposure  
✔ reduces security breach impact  
✔ simplifies audits  

In regulated industries, least‑privilege is a compliance requirement.

---

## 🔹 2️⃣ SECURABLE OBJECTS, USERS & ROLES

### ❓ Who owns a securable object?
Whoever created the object becomes its owner **by default** — unless ownership is transferred.

Ownership grants:

- full control
- ability to grant & revoke
- ability to drop
- ability to transfer ownership

Because ownership is powerful, it should be assigned to controlled admin roles — not individuals.

---

## 🔹 3️⃣ PRIVILEGES & PRIVILEGE GROUPS

### ❓ What is a privilege?
A privilege defines an **allowed operation**.

> Examples: SELECT, INSERT, UPDATE, OWNERSHIP, USAGE

Privileges are always attached to roles — never directly to people in mature environments.

---

### ❓ Why shouldn’t privileges be assigned to users directly?
Direct user privileges create chaos:

❌ Difficult to audit  
❌ Impossible to scale  
❌ Very hard to remove access later  

Correct model:

> Grant privileges → to roles → assign roles → to users.

This makes access reusable and governed.

---

### ❓ What is global privilege vs scoped privilege?
Global privilege = applies across the whole account  
Example: CREATE DATABASE

Scoped privilege = applies to specific objects  
Example: SELECT ON TABLE sales.orders

Global = wide power  
Scoped = limited to small scope

---

## 🔹 4️⃣ SNOWFLAKE SECURITY HIERARCHY

Snowflake separates responsibilities into roles to avoid misuse.

### 🏛 Built‑in admin roles

| Role | Responsibility |
|---|---|
| ACCOUNTADMIN | overall governance, billing, account configuration |
| SECURITYADMIN | manages roles and grants |
| SYSADMIN | manages databases, schemas, warehouses |
| USERADMIN | manages user accounts |

Best practice:

> Use ACCOUNTADMIN rarely — only for governance tasks.  
> Daily work should happen using SYSADMIN / SECURITYADMIN appropriately.

---

## 🔹 5️⃣ CREATING & USING ROLES

### ❓ Why create custom roles?
Default roles are **not** designed for business functions.

Custom roles align access to:

- Finance
- Marketing
- ETL
- Support
- Data Science

This prevents developers from having unnecessary permissions.

---

### ❓ Can roles inherit other roles?
Yes — roles can contain roles.

Example:

```
DATA_ANALYST_ROLE
   ⬇ includes
READ_ONLY_ROLE
```

A user with DATA_ANALYST_ROLE automatically inherits lower‑level privileges.

---

## 🔹 6️⃣ CREATING & MANAGING USERS

### ❓ Why avoid shared users?
Because shared accounts:

❌ remove accountability  
❌ break auditing  
❌ encourage bad security practices

Always create individual named users and grant roles appropriately.

---

## 🔹 7️⃣ SCENARIOS — PRACTICAL THINKING (EXPANDED)

### ⭐ Scenario 1
**Developer needs read‑only access to a database — what do you create?**

Create:

1️⃣ a read‑only role  
2️⃣ grant SELECT on specific schemas/tables  
3️⃣ assign the role to developer

Do NOT grant SYSADMIN or direct table privileges.

---

### ⭐ Scenario 2
**Analyst needs access only to reporting schema.**

Grant:

- USAGE on database
- USAGE on schema
- SELECT on required tables

All through a dedicated reporting role.

---

### ⭐ Scenario 3
**User has role but still cannot query. What to check?**

Check in order:

1️⃣ database USAGE granted?  
2️⃣ schema USAGE granted?  
3️⃣ object SELECT granted?  
4️⃣ correct session role active?

Snowflake enforces top‑down permission flow.

---

### ⭐ Scenario 4
**Developer accidentally dropped a table. What went wrong?**

They had **too much privilege** — likely OWNERSHIP or DROP.

Fix:

- restrict powerful roles
- enforce separation of duties
- introduce approval workflows

---

### ⭐ Scenario 5
**Production data exposed accidentally — what mistake caused it?**

Likely:

- granting broad roles such as SYSADMIN to analysts
- sharing data without secure views
- no masking policies

RBAC wasn’t designed properly.

---

### ⭐ Scenario 6
**New team joins project — how should roles be structured?**

Create:

- project‑level parent role
- functional child roles under it

Example:

```
PROJECT_X
   ⬇
PROJECT_X_READ
PROJECT_X_WRITE
PROJECT_X_ADMIN
```

Assign users the minimum role required.

---

### ⭐ Scenario 7
**Auditors require access logs — what feature helps?**

Use:

- ACCOUNT_USAGE.ACCESS_HISTORY
- QUERY_HISTORY
- LOGIN_HISTORY

These show who accessed which data and when.

---

### ⭐ Scenario 8
**Junior engineer was given ACCOUNTADMIN — risk? fix?**

Risk:

🚨 full destructive power

Fix:

- revoke ACCOUNTADMIN
- create proper functional role
- enforce approval policy

---

### ⭐ Scenario 9
**Need to grant table access to multiple users — how to do correctly?**

Create role → grant table privileges → assign role to users.

Never grant each user separately.

---

### ⭐ Scenario 10
**Role assigned but still no access — why?**

Possible reasons:

- user didn't switch to correct role
- missing USAGE privilege at DB or schema level
- stale session
- grants applied to wrong role

---

## 🔹 8️⃣ MISCONCEPTIONS — FULL EXPLANATIONS

### ❌ “Users own data by default.”
False. Objects are owned by the **creating role**, not the user.

---

### ❌ “Granting SELECT also gives INSERT.”
Incorrect — each privilege is independent.

---

### ❌ “USAGE on a database means I can query tables.”
Wrong — USAGE only allows visibility. You still need SELECT.

---

### ❌ “SYSADMIN can do everything.”
No — SECURITYADMIN controls access grants. Roles are intentionally separated.

---

### ❌ “ACCOUNTADMIN automatically bypasses RBAC.”
Not true — masking policies, secure views, and object ownership still apply.

---

### ❌ “Dropping a user removes their objects.”
Objects remain — ownership transfers, otherwise account breaks.

---

### ❌ “Privileges flow backwards in role hierarchy.”
No — privileges only flow downward, never upward.

---

### ❌ “More roles means better security.”
Usually the opposite — complexity increases risk.

---

### ❌ “Built‑in roles are enough.”
Not for real enterprises. Custom roles are essential.

---

### ❌ “GRANT to role won’t apply automatically to assigned users.”
It does — any newly granted privileges become immediately available when that role is active.

---


