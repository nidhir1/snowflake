# ❄️ Snowflake — Procedures & Views  
## Question Bank (Questions Only)

---

🔹 1️⃣ STORED PROCEDURES — BASICS

What is a stored procedure in Snowflake?

Why do we use stored procedures?

What kind of logic is best suited for procedures?

Where do stored procedures execute?

What languages can procedures be written in?

Difference between SQL procedures & JavaScript procedures.

What is a handler function?

What is return type of procedure?

What privileges are required to create procedures?

Can stored procedures modify data?

---

🔹 2️⃣ CREATING & USING PROCEDURES

What are the main components of a procedure definition?

What are parameters in stored procedures?

What is input parameter vs output?

How do you declare variables in a stored procedure?

How do loops work in procedures?

How to handle conditional logic in procedures?

Can procedures call other procedures?

Can procedures execute dynamic SQL?

What is EXECUTE AS OWNER vs EXECUTE AS CALLER?

How are transactions handled inside procedures?

---

🔹 3️⃣ USING CALL COMMAND

What is CALL command used for?

What is the syntax of CALL?

How are arguments passed to CALL?

What happens if incorrect parameter type is passed?

Can CALL be executed from UI, SnowSQL, or tools?

What happens when procedure throws error?

How can we capture results from CALL?

Can we log messages inside procedures?

How do we debug failing CALL executions?

What are best practices when exposing procedures to others?

---

🔹 4️⃣ VIEWS — BASICS

What is a view?

Why are views used instead of tables?

What is logical vs physical storage?

What is a regular (non-materialized) view?

Do views store data physically?

What happens when the base table changes?

Can views be updated?

What is a secure view?

Why are secure views important for data sharing?

What permissions are required for views?

---

🔹 5️⃣ QUERY STORAGE & VIEW BEHAVIOR

Does Snowflake store the SQL definition of a view?

Where is view definition metadata stored?

Do queries through views use result cache?

Are performance optimizations applied to views?

Does Snowflake rewrite queries from views automatically?

How do masking policies interact with views?

Can materialized views exist on top of regular views?

What happens when we drop underlying tables?

How does dependency tracking work for views?

Can views reference views?

---

🔹 6️⃣ SYSTEM PREDEFINED VIEWS

What are system views in Snowflake?

What are INFORMATION_SCHEMA views?

What are ACCOUNT_USAGE views?

What are ORGANIZATION_USAGE views?

When should we use system views?

What monitoring queries rely on system views?

How long does history persist in system views?

Are system views free or billable to query?

Are permissions required to query system views?

Examples of useful system view queries.

---

🔹 7️⃣ SCENARIO-BASED QUESTIONS — REAL PROJECT THINKING

You need reusable ETL logic that executes multiple SQL steps — what do you use?

You need to mask PII but allow analysts to query — what do you build?

You want to enforce row-level filtering — view or table?

You need to automate data cleanup daily — procedure or task?

You need a “read-only” abstraction layer for dashboards — view or clone?

A developer wants to execute code with elevated privileges — which execution model?

Base table is dropped accidentally — what happens to view?

Queries using views suddenly slow — what should you check first?

Team needs query history monitoring — where to look?

You want governance rules visible across schemas — which system views help?

---

🔹 8️⃣ TRICK / MISCONCEPTION QUESTIONS

Are stored procedures required to run SQL in Snowflake?

Does a view physically store query results?

Do views automatically improve performance?

Are materialized views the same as regular views?

Does CALL bypass role-based security?

Are procedures faster than SQL scripts automatically?

If you drop a table, does the view data still exist?

Are system views editable?

Do system views update in real-time always?

Is procedure code visible to everyone by default?
