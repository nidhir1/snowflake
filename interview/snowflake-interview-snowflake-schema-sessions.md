# Snowflake — Schemas, Session Context and Data Loading
Questions Only

---

## 1. SCHEMAS IN SNOWFLAKE

### A. BASIC — What schemas are
- What is a schema in Snowflake?
- How is a schema different from a database?
- Why do we use schemas?
- Can a database have multiple schemas?
- What kinds of objects can exist inside schemas?
- What is the default schema when none is selected?
- Can two schemas have tables with the same name?
- What is schema namespace?
- Who can create schemas?
- What privilege is required to create a schema?

### B. CREATION & REAL-TIME USAGE
- How do you create a schema in SQL?
- How do you switch to a schema?
- How do you view all schemas in a database?
- How do you view objects inside a schema?
- Why do organizations create DEV / QA / PROD schemas?
- Why do teams separate RAW, STAGING, and CURATED schemas?
- What happens when you drop a schema?
- Can schemas be cloned?
- Can schemas be renamed?
- How do you prevent accidental schema deletion?

### C. PERMANENT vs TRANSIENT SCHEMAS
- What is a permanent schema?
- What is a transient schema?
- What is the difference between permanent and transient schemas?
- Do transient schemas support Time Travel?
- Do transient schemas support Fail-safe?
- When should you prefer transient schemas?
- What risks exist with transient schemas?
- Can transient schemas contain permanent tables?
- Can schemas be converted from permanent to transient?
- Why do transient schemas reduce storage cost?

### D. MANAGED SCHEMAS
- What is a managed schema?
- How are managed schemas created?
- Which Snowflake services use managed schemas?
- What happens if you drop objects in a managed schema?
- Why are managed schemas not recommended for manual operations?
- How do managed schemas help automation?
- Are managed schemas allowed for cloning?
- Can we modify managed schemas manually?
- What governance controls are needed for managed schemas?
- When should students avoid managed schemas?

---

## 2. SESSION CONTEXT

### A. BASIC — Understanding sessions
- What is a Snowflake session?
- How is a session created?
- What happens when a session closes?
- What is session timeout?
- What is the difference between a session and a user?

### B. SESSION CONTEXT COMPONENTS
- What is session context in Snowflake?
- What is current role?
- What is current warehouse?
- What is current database?
- What is current schema?
- Why is session context important?
- How do you set a role in session?
- How do you set a warehouse in session?
- How do you set database and schema together?
- What happens if required context isn’t set?
- What is the default context on login?
- What commands show current session context?
- Can multiple tabs have different contexts?
- What are fully qualified names?
- When should you use fully qualified names?

### C. FULLY QUALIFIED NAMES
- What is object full path format?
- Give an example of a fully qualified name for a table.
- Why do we sometimes prefer fully qualified names?
- When does Snowflake require full qualification?
- How do roles interact with fully qualified objects?
- Can fully qualified names bypass context selection?
- What happens if schema or database is not found in the path?
- What are best practices for beginners when using full paths?

---

## 3. DATA LOADING (INTRO)

### A. DATA LOADING METHODS
- What are the main ways to load data into Snowflake?
- What is loading via the Web UI (GUI)?
- What is loading using COPY INTO?
- What is SnowSQL loading?
- What is the difference between staging and loading?
- What is file staging before loading?
- Why should we avoid inserting data row by row?
- What file formats are commonly loaded into Snowflake?

### B. DATA LOADING USING GUI
- How do you upload a CSV from local machine?
- Which stage is used when uploading via GUI?
- What role and warehouse are required?
- What happens behind the scenes when using GUI load?
- What happens if file load fails?
- Where do we see the details of a load job?

### C. QUERY & HISTORY TAB
- What is the Query tab used for?
- What is the History tab used for?
- What details can we see in query history?
- How do we debug load failures from history?
- Can we see COPY INTO commands in history?
- What is execution time vs compilation time?
- How do we find which warehouse executed the query?
- How long does Snowflake retain query history?

---

## 4. SCENARIO-BASED QUESTIONS

- You created a table but cannot find it — what should you check?
- You get “object does not exist” — what could be the reason?
- Two tables exist with the same name — how is that possible?
- You cannot drop a schema — what might be wrong?
- You want cheaper working storage — what type of schema should you use?
- A script fails because the wrong schema was active — how do you fix it?
- Data was accidentally loaded into the wrong schema — what do you do?
- A file didn’t load — how do you check what happened?
- GUI upload shows success but table is empty — what should you inspect?
- A student overwrote a table accidentally — how can Time Travel help?

---

## 5. TRICK / MISCONCEPTION QUESTIONS

- Does schema mean the same as database?
- Does dropping a schema always permanently delete data?
- Does transient schema mean temporary?
- Does transient schema provide free storage?
- Do managed schemas behave like normal schemas?
- Does session context remain forever?
- Does changing session role give access to everything?
- Do fully qualified names ignore roles and privileges?
- Does GUI loading avoid stages?
- Does the History tab show only failed queries?
- Does Time Travel work for every schema type?
- Do we need a warehouse for reading only?
- Does uploading through GUI cost nothing?
- Is COPY INTO required for all loads?
- Does Snowflake automatically correct wrong schema usage?
