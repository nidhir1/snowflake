
# ❄️ Snowflake — Security Management & RBAC
## Question Bank (Questions Only)

---
## 🔹 1️⃣ SECURITY ENTITIES IN SNOWFLAKE — BASICS

What is security in Snowflake meant to protect?
What are the main security entities in Snowflake?
What is an object in Snowflake?
What is a securable object?
What is authentication?
What is authorization?
What is least privilege principle?
Why is least privilege important?
What are roles in Snowflake?
What are users in Snowflake?

---
## 🔹 2️⃣ SECURABLE OBJECTS, USERS & ROLES

What are securable objects?
Examples of securable objects in Snowflake.
Can databases be securable objects?
Can warehouses be securable objects?
Can schemas be securable objects?
Are tables securable objects?
Can stored procedures be securable objects?
Who owns a securable object?
What does object ownership mean?
Who can change privileges on an object?

---
## 🔹 3️⃣ PRIVILEGES & PRIVILEGE GROUPS

What is a privilege?
Difference between role and privilege.
Examples of privileges (USAGE, SELECT, INSERT, etc.)
What is GRANT?
What is REVOKE?
Can privileges be assigned directly to users?
Why shouldn’t privileges be assigned to users directly?
What is a privilege group?
What are account-level privileges?
What are database-level privileges?
What are schema-level privileges?
What are object-level privileges?
What is global privilege vs scoped privilege?

---
## 🔹 4️⃣ SNOWFLAKE SECURITY HIERARCHY

What is Snowflake security hierarchy?
What is role hierarchy?
What is the top-level role?
What is ACCOUNTADMIN?
What is SECURITYADMIN?
What is SYSADMIN?
What is USERADMIN?
Why should ACCOUNTADMIN be rarely used?
Which role creates users and roles?
Which role grants object access?
Which role typically manages warehouses, DB objects?
Why separate administrative responsibilities?

---
## 🔹 5️⃣ CREATING & USING ROLES

Why create custom roles?
What are best practices for roles?
Steps to create a new role.
How to assign privileges to a role.
How to assign roles to users.
What is a default role?
What is a current role?
How to switch roles in session.
Can one user have multiple roles?
Can roles inherit other roles?
What happens when a role is dropped?
What is role ownership?

---
## 🔹 6️⃣ CREATING & MANAGING USERS

What is a user account?
What attributes can a Snowflake user have?
What is default warehouse / database / schema for user?
How are users authenticated?
What is multi-factor authentication (MFA)?
What is SSO / OAuth?
Why avoid shared user accounts?
What is user locking?
How to reset passwords?
Who can modify users?

---
## 🔹 7️⃣ SCENARIO-BASED QUESTIONS — PRACTICAL THINKING

A developer needs read-only access to a database — what do you create?
Business analyst needs access only to reports schema — what approach?
A user cannot query a table even though they have role — what check?
Developer accidentally dropped a table — what went wrong security-wise?
Production data was exposed accidentally — what RBAC mistake likely?
New project team arrives — how should roles be structured?
Auditors require logging of who accessed sensitive data — what helps?
A junior engineer was given ACCOUNTADMIN — risk and fix?
Need to grant table access to multiple users — how to do it correctly?
Role changes were made but user still cannot access — possible reasons?

---
## 🔹 8️⃣ TRICK / MISCONCEPTION QUESTIONS

Do users own data by default?
Does granting SELECT automatically allow INSERT?
Does granting USAGE on database allow querying tables inside?
Does SYSADMIN override SECURITYADMIN?
Does ACCOUNTADMIN automatically see all data?
Can users bypass roles and access objects directly?
Does dropping a user remove their objects automatically?
Are privileges inherited backwards (child → parent role)?
Do roles exist outside accounts?
Can one role belong to two different accounts?
Does assigning multiple roles increase security automatically?
Are built-in roles enough for enterprise environments?
Does GRANT applied to role update automatically for all users assigned?
