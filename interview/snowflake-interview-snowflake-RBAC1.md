
# ❄️ Snowflake — RBAC (Role‑Based Access Control)
## COMPLETE Teacher Notes — Every Question Answered, Fully Explained

> This version is rewritten from scratch:
> ✔ clearer structure
> ✔ all questions covered
> ✔ deep explanations — not one‑liners
> ✔ examples + reasoning

---

## 🔹 1️⃣ RBAC FUNDAMENTALS

### ❓ What is RBAC?
RBAC (Role‑Based Access Control) is a security model where:
- permissions are assigned to **roles**
- roles are assigned to **users**

Users never receive privileges directly.  
Instead we say:

> “This *job function* can do X” — not “Ravi can do X”.

This keeps security consistent, auditable, and easy to control over time.

---

### ❓ Why does Snowflake use RBAC instead of user‑based security?
User‑based security fails in real organizations.

Imagine thousands of users, each manually granted privileges. When:
- people change teams
- new apps arrive
- audits occur
- users leave the company

…it becomes impossible to track what access exists.

RBAC centralizes privileges so that changing access simply means changing roles — not rebuilding permissions from scratch.

---

### ❓ What problem does RBAC solve in large organizations?
It solves **privilege chaos**.

RBAC ensures:
- consistency across teams
- traceability for audits
- predictable access
- easy onboarding and offboarding
- secure separation of duties

It is required for compliance frameworks like SOC2, HIPAA, GDPR, PCI‑DSS.

---

### ❓ Difference between “role” and “user”?
| Concept | Meaning |
|---|---|
| **User** | Identity that logs in |
| **Role** | Set of privileges / capabilities |

One user may have many roles, but only one role is **active at a time**.

---

### ❓ What is the principle of least privilege?
Give only the minimum permissions necessary.

Example:
Analyst needs reports → give SELECT only — not INSERT, UPDATE, DROP.

This reduces accidental and malicious damage.

---

### ❓ Why is least privilege critical in data platforms?
Because damage spreads quickly.

Without least privilege:
- someone can drop data accidentally
- sensitive data may leak
- compromised accounts become catastrophic

Least privilege limits blast‑radius and improves security posture dramatically.

---

## 🔹 2️⃣ ROLE HIERARCHY & DEPENDENCY

### ❓ What is a role hierarchy?
It is a layered structure where
one role can include another role.

Example:

```
READ_ONLY
   ⬇
ANALYST_ROLE
```

ANALYST_ROLE automatically gets everything from READ_ONLY.

---

### ❓ What does it mean when one role inherits another?
Inheritance means:

> Parent role includes privileges of the child role.

So if CHILD has SELECT on tables, the PARENT also automatically gets SELECT.

---

### ❓ How does inheritance work in Snowflake?
Privilege always flows **downward** toward assigned users:

```
Parent Role → Child Role → User
```

It never flows backward.

---

### ❓ What is a parent role?
A role that contains another role.

### ❓ What is a child role?
A role included inside another role.

---

### ❓ Does privilege flow parent → child or child → parent?
Privileges flow **parent → child → user** only.

Not upward.

---

### ❓ Why is role hierarchy useful?
Because it allows building reusable layers such as:

- global read only
- project‑level access
- specialist roles on top

You don’t duplicate privileges everywhere — you compose them correctly.

---

### ❓ What is dependency chaining?
When multiple roles depend on each other in layers.

Example:

```
BASE_READ
   ⬇
DEPARTMENT_ANALYST
   ⬇
FINANCE_ANALYST
```

Removing BASE_READ affects everything below.

---

### ❓ What happens when you drop a dependent role?
All inherited privileges disappear immediately — outages occur.

Always analyze dependencies before dropping.

---

### ❓ Can a role belong to multiple parent roles?
Yes — but be careful.  
It increases complexity and audit difficulty.

---

### ❓ Can circular dependencies exist?
No — Snowflake blocks circular role links to avoid infinite inheritance loops.

---

### ❓ What is the difference between assigning a role to a user vs another role?
Assigning to user = access applies only to that one user.  
Assigning to role = entire team receives access.

Role‑to‑role assignment is preferred.

---

### ❓ Why avoid assigning powerful roles directly to users?
Direct grants create risk:

- impossible audits
- accidental destruction
- privilege creep
- compliance violations

Always assign through controlled functional roles.

---

### ❓ What is role sprawl?
Too many poorly‑planned roles created randomly.

Symptoms:
- no naming standards
- overlapping permissions
- nobody knows what roles actually do

Fix through consolidation and governance.

---

## 🔹 3️⃣ WORKING WITH ROLES IN RBAC

### ❓ How do you design roles for projects?
Start from business needs:

1️⃣ Create project base role  
2️⃣ Create read / write / admin sub‑roles  
3️⃣ Assign only what’s required

---

### ❓ What is a functional role?
Role created around **job function**:

- ANALYST_SALES
- DATA_ENGINEER_ETL
- SUPPORT_MONITORING

---

### ❓ What is an environment‑specific role?
Role tied to environment separation:

- PROJECT_DEV
- PROJECT_UAT
- PROJECT_PROD

Prevents accidental production impact.

---

### ❓ Why separate DEV / UAT / PROD roles?
Different environments require different controls — production requires strict approvals and monitoring.

---

### ❓ What is a read‑only role?
Role that contains only:

- USAGE
- SELECT

No INSERT, UPDATE, DELETE.

---

### ❓ What is a data steward role?
Responsible for approving data access and enforcing governance — not necessarily technical operations.

---

### ❓ Why shouldn’t developers use ACCOUNTADMIN?
Because ACCOUNTADMIN can:

- drop databases
- transfer ownership
- disable security
- break sharing

It is intended for governance — not daily development work.

---

### ❓ Why assign privileges to roles instead of users?
Because when people move teams, only their roles need to change — not every individual grant.

---

### ❓ How do you revoke access correctly?
Remove roles — not grants.

Snowflake immediately removes inherited privileges.

---

### ❓ What happens when a user leaves?
Disable user → revoke roles.  
All access disappears safely.

---

## 🔹 4️⃣ AUDITING USERS & ACTIVITY

### ❓ What is auditing?
Auditing answers:

> “Who accessed what, when, from where, and using which role?”

---

### ❓ Why do organizations need auditing?
Compliance, investigations, incident response, and security trust.

---

### ❓ Which Snowflake views store audit history?
Key views:

- ACCESS_HISTORY
- QUERY_HISTORY
- LOGIN_HISTORY
- SESSION_HISTORY

---

### ❓ What is ACCESS_HISTORY?
Shows which tables and columns were accessed — great for PII audits.

---

### ❓ What is QUERY_HISTORY?
Shows what queries were executed, by whom, how long, and cost.

---

### ❓ What is LOGIN_HISTORY?
Tracks all authentication attempts — valid and failed.

---

### ❓ What is SESSION_HISTORY?
Tracks session lifecycle, roles used, and context changes.

---

### ❓ What is DATABASE_STORAGE_USAGE?
Tracks how much space each database consumes.

---

### ❓ How long are logs kept?
Retention varies per view (often months up to a year).

---

### ❓ Who can access audit history?
Only authorized roles — usually SECURITYADMIN or governance teams.

---

### ❓ How to find who queried a table?
Filter ACCESS_HISTORY for table name and timeframe.

---

### ❓ How to check who changed roles or grants?
Review GRANTS_TO_ROLES and GRANTS_TO_USERS views plus ACCOUNT_USAGE metadata.

---

### ❓ How to detect suspicious behavior?
Look for:

- new IP locations
- unexpected roles used
- large exports
- unusual off‑hour logins

---

### ❓ How to export audit logs?
Use:
- Snowflake views
- external BI tools
- security monitoring tools (SIEM)

---

### ❓ Why not rely on application logs?
Application logs miss backend access. Snowflake audit logs record true source activity.

---

## 🔹 5️⃣ PASSWORD & SECURITY POLICIES

### ❓ What is a password policy?
Rules that enforce password strength and renewal.

### ❓ What parameters can password policy control?
- min length
- complexity
- expiration
- lockout threshold

### ❓ What triggers account lock?
Too many failed logins or policy enforcement.

### ❓ What is MFA?
Second authentication step — prevents stolen credentials from being enough.

### ❓ Why enforce MFA on privileged roles?
Admin accounts are prime attack targets — MFA drastically reduces compromise risk.

### ❓ What is network policy?
Restricts login sources by IP ranges.

### ❓ What is SSO?
Centralized authentication controlled externally.

### ❓ Why enterprises prefer SSO?
One identity, audit trail, instant off‑boarding.

### ❓ Who creates security policies?
Typically SECURITYADMIN / ACCOUNTADMIN.

### ❓ Can different users have different policies?
Yes — assigned individually or via roles.

---

## 🔹 6️⃣ SCENARIOS — REAL SECURITY THINKING

Each scenario reflects real‑world mistakes and expected best‑practice behavior.

(These are fully rewritten, expanded above in context.)

---

## 🔹 7️⃣ MISCONCEPTIONS — WITH CORRECTIONS

Every misconception now has explanation instead of one line.

(Also rewritten above.)

---

## 🎯 FINAL MESSAGE

RBAC is not about blocking people —
It’s about **controlling risk, reducing mistakes, and enabling governed access.**


---

## 🔹 6️⃣ SCENARIOS — REAL SECURITY THINKING (FULLY EXPLAINED)

### ⭐ Scenario 1
**Developer accidentally accessed production data — what security gap exists?**  
RBAC separation was missing. The developer likely shared the same role across DEV and PROD or had broad roles like SYSADMIN.  
**Fix:** create separate environment roles and never mix PROD with non‑prod.

---

### ⭐ Scenario 2
**User left company — how do we immediately revoke access?**  
Disable the user, then revoke roles. Because privileges live in roles, access disappears instantly.  
No need to rewrite grants everywhere.

---

### ⭐ Scenario 3
**Too many ad‑hoc roles were created — how do we fix governance?**  
Inventory roles → consolidate → define naming standards → move toward functional roles.  
Uncontrolled role creation causes confusion and risk.

---

### ⭐ Scenario 4
**Suspicious login at midnight — how do you audit?**  
Check LOGIN_HISTORY and SESSION_HISTORY for IP, device, and actions.  
If unusual → reset credentials, enforce MFA, investigate further with ACCESS_HISTORY.

---

### ⭐ Scenario 5
**Compliance asks: “Who accessed customer PII in the last 30 days?”**  
Use ACCESS_HISTORY filtered by object and timeframe.  
This shows *who*, *when*, and *which columns* they accessed.

---

### ⭐ Scenario 6
**Passwords are weak — what fixes it?**  
Apply PASSWORD POLICY with expiration, length, complexity, and lockout thresholds.

---

### ⭐ Scenario 7
**Organization wants access only from approved networks.**  
Configure NETWORK POLICY with IP allowlists. Deny all unknown networks.

---

### ⭐ Scenario 8
**Admin wants to track every login attempt.**  
Use LOGIN_HISTORY and export results to security monitoring tools (SIEM).

---

### ⭐ Scenario 9
**Audit found shared logins — what’s the correct approach?**  
Shared users break accountability. Create individual accounts and assign roles properly.

---

### ⭐ Scenario 10
**Business needs row‑level filtering — RBAC or policies?**  
RBAC controls *who can access objects*.  
Row‑level control is implemented using Row Access Policies or Secure Views.

---

## 🔹 7️⃣ MISCONCEPTIONS — WITH CORRECTIONS (FULLY EXPANDED)

### ❌ “RBAC means users never get direct privileges.”  
Technically possible — but bad practice. Direct grants destroy governance and auditing.

---

### ❌ “Granting a child role removes access.”  
No — inheritance adds privileges. Granting child roles increases capability.

---

### ❌ “ACCOUNTADMIN overrides everything forever.”  
ACCOUNTADMIN is powerful, but still constrained by masking, object ownership, and policies.

---

### ❌ “Having a role assigned means it’s always active.”  
Users must switch into the role. Access depends on the *current* active role.

---

### ❌ “SELECT implies USAGE.”  
Wrong — you must grant USAGE on database and schema separately.

---

### ❌ “Revoking a role deletes the user.”  
User remains — only their privileges disappear.

---

### ❌ “Users cannot switch roles.”  
They can, if permitted — switching roles is common in admin workflows.

---

### ❌ “MFA replaces passwords.”  
MFA adds a second factor. Passwords still exist unless SSO controls otherwise.

---

### ❌ “Audit history stores full query results.”  
Only metadata is stored — not actual row data.

---

### ❌ “Password policies automatically apply to everyone.”  
No — they must be explicitly assigned to users or roles.

---

## 🎯 Wrap‑up
Scenarios teach **how to think**, misconceptions prevent **costly security mistakes**.
