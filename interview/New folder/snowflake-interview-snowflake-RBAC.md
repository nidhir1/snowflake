
# ❄️ Snowflake — RBAC (Role‑Based Access Control)
## Question Bank (Questions Only)

---
## 1️⃣ RBAC FUNDAMENTALS

What is RBAC?
Why does Snowflake use RBAC instead of user-based security?
What problem does RBAC solve in large organizations?
What is the difference between “role” and “user”?
What is the principle of least privilege?
Why is least privilege critical in data platforms?

---
## 2️⃣ ROLE HIERARCHY & DEPENDENCY

What is a role hierarchy?
What does it mean when one role inherits another role?
How does inheritance work in Snowflake roles?
What is a parent role?
What is a child role?
Does privilege flow from parent → child or child → parent?
Why is role hierarchy useful?
What is dependency chaining?
What happens when you drop a role that others depend on?
Can a role belong to multiple parent roles?
Can circular dependencies exist?
What is the difference between granting a role to a user vs to another role?
Why avoid giving powerful roles directly to users?
What is the risk of “role sprawl”?

---
## 3️⃣ WORKING WITH ROLES IN RBAC

How do you design roles for projects?
What is a functional role?
What is an environment-specific role?
Why create separate roles for DEV / UAT / PROD?
What is a read-only role?
What is a data steward role?
Why shouldn’t developers use ACCOUNTADMIN?
Why assign privileges to roles instead of users?
How to revoke access correctly?
What happens when a user leaves the organization?

---
## 4️⃣ AUDITING USERS & ACTIVITY

What is auditing?
Why do organizations need auditing?
Which Snowflake views store audit history?
What is ACCESS_HISTORY?
What is QUERY_HISTORY?
What is LOGIN_HISTORY?
What is SESSION_HISTORY?
What is DATABASE_STORAGE_USAGE?
How long audit logs are retained?
Who can access audit history?
How do you find who queried a specific table?
How do you check who changed a role or grant?
How do you detect suspicious access behavior?
How can audit results be exported?
Why should audits never rely on application logs alone?

---
## 5️⃣ PASSWORD & SECURITY POLICIES

What is a password policy?
What parameters can password policies control?
What is password expiration?
What is password complexity?
What is account locking?
What triggers account lock?
What is MFA (Multi-Factor Authentication)?
Why enforce MFA for privileged roles?
What is network policy?
What is IP whitelist / blacklist?
What is session timeout?
What is SSO integration?
Why do enterprises prefer SSO?
Who can create password or network policies?
Can different users have different policies?

---
## 6️⃣ SCENARIO-BASED QUESTIONS — REAL SECURITY THINKING

Developer accidentally accessed production data — what security gap exists?
User left company — how do we immediately revoke access?
Team created too many ad‑hoc roles — how do we fix governance?
A suspicious user login appears at midnight — how do you audit?
Compliance asks: “who accessed customer PII in last 30 days?” — where to look?
Passwords are weak across team — what Snowflake feature fixes it?
Organization wants IP‑restricted access only — what to configure?
Admin wants to track every login attempt — what to use?
Audit found shared logins — what is the correct Snowflake approach?
Business wants row‑level security — RBAC or policies?

---
## 7️⃣ TRICK / MISCONCEPTION QUESTIONS

Does RBAC mean users never get direct privileges?
Does granting a child role automatically downgrade access?
Does ACCOUNTADMIN override everything forever?
Does having a role assigned mean it's always active?
Does SELECT privilege automatically imply USAGE privilege?
Does revoking role automatically drop the user?
Can a user switch roles anytime?
Does enabling MFA replace passwords?
Does audit history contain query results?
Are password policies automatically applied to all users?
