# ❄️ Snowflake — Components, Objects & Platform Concepts
## QUESTIONS ONLY

---

## 🔹 1. BASIC — CORE COMPONENTS OVERVIEW

What are the main components of Snowflake?

What is a Snowflake account?

What is a region in Snowflake?

What is a database in Snowflake?

What is a schema?

What is a table?

What are views?

What are temporary tables?

What are transient tables?

What is a virtual warehouse?

What is a stage?

What is a file format?

What is a role?

What is a user?

What is a session context?

What is the services layer?

What is the compute layer?

What is the storage layer?

What are system views / information schema?

What tools can connect to Snowflake?

---

## 🔹 2. INTERMEDIATE — DATABASES, SCHEMAS, OBJECTS

Difference between database and schema.

Why separate environments (DEV, UAT, PROD)?

What is a fully qualified object name?

Why is naming convention important?

What is a permanent table?

When do we use transient tables?

When do we use temporary tables?

What is a view and why use it?

What is a secure view?

What are materialized views?

What are sequences?

What are stages inside databases vs schemas?

What is the purpose of information_schema?

How does Snowflake manage metadata?

What happens when you drop a table?

How does Time Travel affect dropped objects?

What is UNDROP?

What is clustering key?

Why would you define clustering on a table?

What objects support Time Travel?

---

## 🔹 3. VIRTUAL WAREHOUSES (COMPUTE)

What is a virtual warehouse used for?

Difference between warehouse and storage.

How are warehouse sizes defined?

What is auto-suspend?

What is auto-resume?

What is warehouse scaling policy?

When do you scale up vs scale out?

What is multi-cluster warehouse?

How does concurrency affect warehouses?

Can multiple warehouses query same data?

Why separate ETL and BI warehouses?

What happens when a warehouse is paused?

How to monitor warehouse usage?

What are resource monitors?

Can warehouses be shared between accounts?

---

## 🔹 4. STAGES & FILE HANDLING

What is a stage?

What are internal stages?

What are external stages?

What is a user stage?

What is a table stage?

What is a named internal stage?

Why do we use stages instead of direct loading?

What is COPY INTO command?

What file formats does Snowflake support?

What is a file format object?

What is VALIDATION MODE in COPY?

What is ON_ERROR option?

How to handle duplicate file loads?

What is metadata column $FILE_NAME?

How to unload data back to files?

---

## 🔹 5. SERVICES & PLATFORM FEATURES

What is Query History?

What is Query Profile?

What are tasks?

What are streams?

What is Snowpipe?

What is data sharing?

What is reader account?

What is replication?

What is failover / failback?

What is masking policy?

What is row access policy?

What are tags?

What is governance in Snowflake?

What is Snowpark?

What are warehouses vs services responsibilities?

---

## 🔹 6. SCENARIO-BASED QUESTIONS — REAL PROJECT THINKING

Developer needs a sandbox copy of data — what feature helps?

BI team complains ETL jobs slow dashboards — what to configure?

Files arrive daily in S3 — how to automate ingestion?

Accidental table drop — how to recover?

You need temporary working data for transformations — which table type?

You need persistent but cheaper storage — which table type?

Duplicate file loaded twice — how to prevent?

Data scientists want JSON logs — how do you store them?

Multiple users cause query queueing — what do you change?

External partner needs read-only access — how to share data?

Big historical table queries slow — what tuning option helps?

Migration team needs DEV/UAT/PROD quickly — how to provision?

A warehouse is running all day idle — what controls stop waste?

Need audit history of queries — where to check?

Team requests secure masking of PII — what do you use?

---

## 🔹 7. TRICK / MISCONCEPTION QUESTIONS

Does stopping warehouse delete data?

Are indexes required on tables?

Does materialized view always improve performance?

Is Time Travel same for all table types?

Do transient tables support fail-safe?

Does Snowflake automatically prevent duplicate file loads?

Does Snowflake charge for paused warehouses?

Are stages only for loading data?

Does cloning copy physical data?

Can we change warehouse while a query runs?

Are views faster than tables?

Does every object support UNDROP?

Are tasks the same as cron jobs?

Does every file require COPY INTO?

Can Snowflake replace ETL tools automatically?
