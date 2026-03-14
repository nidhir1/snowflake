# INTRODUCTION TO DATA WAREHOUSING — FULL QUESTION BANK (DETAILED ANSWERS)

---

## 🔹 1. BASIC — FOUNDATIONAL QUESTIONS

### What is a data warehouse?
A data warehouse is a centralized system that stores large amounts of cleaned, organized, and historical data from many source systems.  
It is designed mainly for analysis, dashboards, reporting, and decision-making — not for daily transactions.

### Why do organizations need data warehouses?
Different systems hold bits of data (sales, HR, finance, CRM). A warehouse integrates them into one trusted place.  
It helps leaders compare trends, measure performance, and make consistent business decisions without slowing production systems.

### Difference between operational databases and data warehouses.
Operational databases run business (transactions). Data warehouses analyze business.  
Operational DBs store current data and change constantly, while warehouses store history and optimize for reading, aggregating, and reporting.

### What is OLTP?
OLTP systems support day-to-day business operations — placing orders, banking transfers, ticket bookings, user logins.  
They require fast writes, strict accuracy, and small transactions.

### What is OLAP?
OLAP systems support analysis — dashboards, trends, KPIs, forecasting.  
They read large volumes, summarize data, and answer business questions.

### Why are OLTP and OLAP separated?
Reporting on OLTP creates locks, slows users, and risks outages.  
Separating keeps production fast and analytics powerful, without conflict.

### What is historical data?
Historical data is past data preserved over months or years — showing how values changed over time instead of only current state.

### Why do we store historical data in warehouses?
Business questions depend on change over time:  
Who improved? What declined? What patterns exist?  
OLTP usually overwrites old values — warehouses keep history.

### What is a data mart?
A smaller, topic-focused warehouse — for example Sales Mart, Finance Mart, or HR Mart — usually serving a specific team.

### Difference between data mart and data warehouse.
A data warehouse is enterprise-wide (all domains).  
A data mart focuses on one business function and often pulls trusted data from the warehouse.

### What is ETL?
Extract → Transform → Load  
Data is taken from sources, cleaned and shaped, then loaded into the warehouse.  
Transformations happen **before** loading.

### What is ELT?
Extract → Load → Transform  
Data is first landed in the warehouse (often raw), then transformed using warehouse compute — common in modern cloud systems.

### Examples of BI / reporting tools.
Power BI, Tableau, Looker, Qlik, SAP BO, MicroStrategy, Excel dashboards — tools used to visualize and explore data.

### What is structured data?
Data organized in fixed rows and columns with defined schema — like relational tables.

### What is semi-structured data?
Data with readable structure but flexible format — JSON, XML, log files, web events.

### What is unstructured data?
Data without fixed structure — documents, PDFs, emails, images, audio, social posts.

### Why is consistency important in reporting?
If teams report different numbers for the same metric, trust disappears.  
Consistency ensures every team uses the same logic, data, and definitions.

### What is batch data processing?
Data is collected and processed at scheduled times (hourly, nightly, weekly).  
Good for payroll, billing, reports where real-time isn’t required.

### What is real-time / streaming processing?
Data is processed as soon as it arrives (seconds or milliseconds).  
Used for fraud detection, live dashboards, notifications.

### What is metadata in a data warehouse?
Metadata describes the data — meaning, source, refresh frequency, owners, lineage, data types.  
It helps users understand **what the data represents** and how to use it correctly.

---

## 🔹 2. INTERMEDIATE — HOW IT REALLY WORKS

### Why don’t we directly report from OLTP systems?
OLTP is optimized for fast transactions, not heavy analytics.  
Complex queries slow applications, cause locks, and impact customers.

### What performance challenges exist in OLTP for reporting?
Large scans, expensive joins, blocking locks, contention, timeouts, degraded response times — users feel system “hangs”.

### What are facts?
Facts are measurable numbers — revenue, quantity sold, discount, units returned.  
They answer “how much?” or “how many?”.

### What are dimensions?
Dimensions describe context — product, store, customer, region, time.  
They answer “who?”, “what?”, “where?”, “when?”, “why?”.

### What is a fact table?
Fact tables store numeric measures plus foreign keys to dimensions.  
They can grow very large and are queried frequently for reporting.

### What is a dimension table?
Dimension tables store descriptive attributes (names, categories, hierarchies).  
They make reports readable and filterable.

### What is granularity (grain) in a fact table?
Grain defines the level of detail — per order, per line item, per day, per store.  
Clear grain avoids double counting and confusion.

### Why is granularity important?
Wrong grain leads to inflated totals, missing detail, and wrong decisions.  
It also controls storage, performance, and accuracy.

### What is star schema?
A central fact table surrounded by denormalized dimensions.  
Simple to query, fast, intuitive for BI tools.

### What is snowflake schema?
Dimensions are normalized into multiple linked tables.  
Saves storage but increases query complexity and joins.

### Difference between star & snowflake schema.
Star is simpler and faster for analytics; snowflake is more normalized and storage-efficient.  
Choice depends on performance needs and design philosophy.

### What is denormalization in data warehousing?
We intentionally store some repeated data to speed queries and reduce joins.  
We trade storage for performance.

### What are surrogate keys?
System-generated numeric keys used instead of business identifiers — stable and efficient for joins.

### Why don’t we always use natural keys?
Business keys change, are long, inconsistent across systems, and break relationships when modified.  
Surrogates remain stable.

### What is Slowly Changing Dimension (SCD)?
A method to manage changes in dimension values over time — preserving or overwriting history depending on the need.

### What are SCD Types 0, 1, 2, 3?
Type 0 — never change stored values  
Type 1 — overwrite with latest value  
Type 2 — add new row to preserve history  
Type 3 — store both current and previous values

### What is a staging area?
Temporary zone where raw data lands before transformation.  
Used for backup, troubleshooting, and reprocessing.

### Why is staging required?
Prevents direct dependency on source, helps recover failures, compare old vs new, audit changes, and validate before loading.

### What is data cleansing?
Fixing missing, incorrect, duplicate, inconsistent data — improving reliability.

### What is data transformation?
Standardizing formats, mapping codes, deriving metrics, joining systems, converting data types — preparing data for analytics.

### What is data validation in ETL?
Ensuring accuracy: row counts, referential checks, duplicates, range checks, checksum comparisons.

### What is data loading strategy?
Deciding how and when data moves — full reloads, incremental refresh, CDC, micro-batches, streaming.

### What is full load?
Rebuilds table from scratch — useful first time or when data fully changes, but expensive.

### What is incremental load?
Loads only new or changed data — faster, cheaper, safer for big datasets.

### What is change data capture (CDC)?
Detects real source changes (inserts, updates, deletes) using logs, timestamps, or triggers — prevents re-processing entire datasets.

### Why do we need indexes in some systems?
Indexes speed up searches and filters, especially on large fact tables — but must be tuned carefully to avoid slow loads.

### Why is data quality important?
Bad data leads to wrong KPIs, wrong strategies, audit failures, and loss of trust.  
A warehouse is only as good as the correctness of its data.

---

## 🔹 3. ADVANCED — DESIGN & ARCHITECTURE THINKING

### Explain the components of a data warehouse architecture.
Typical flow: source systems → staging → ETL/ELT pipelines → warehouse core → data marts → BI dashboards → metadata + governance.  
Each layer has a purpose: ingest, clean, integrate, analyze, govern.

### What is a data lake vs data warehouse?
A data lake stores raw, large, diverse data cheaply.  
A warehouse stores curated, structured, trusted analytical data ready for reporting.

### Can both coexist? Why?
Yes — most modern architectures use both.  
The lake collects everything, the warehouse delivers high-quality analytics.

### What is a lakehouse?
A hybrid model combining lake flexibility and warehouse performance 
— often built on modern cloud platforms.

### What is enterprise data warehouse (EDW)?
A single, centralized, integrated warehouse for the whole company 
— the “single source of truth”.

### What are conformed dimensions?
Dimensions reused across marts — same customer, product, date everywhere.  
They ensure reports align across departments.

### Why are conformed dimensions important?
Without them, teams report different numbers for the same metric which leads to chaos.  
They enforce consistency.

### What is a bus architecture?
A standardized framework of conformed dimensions and fact tables that allows scalable, modular expansion.

### What is data lineage?
A full trace of where data came from, how it transformed, and where it is used 
— essential for debugging and audits.

### What is master data management (MDM)?
Processes and tools to maintain golden, authoritative records for core entities like customer, product, vendor.

### What is data governance?
Policies for ownership, privacy, security, retention, definitions, and compliance — ensuring data is handled responsibly.

### What is data catalog?
A searchable library of datasets with definitions, owners, lineage, examples — helping users discover and trust data.

### Why do companies need audit trails?
To track who changed what, when, and why — required for compliance, fraud prevention, and accountability.

### How do we manage slowly changing historical data?
By applying SCD strategies, effective dating, and history tables — never blindly overwriting valuable changes.

### What is partitioning?
Splitting large tables logically (by date, region, etc.) to improve performance and manageability.

### Why is partitioning useful?
Queries scan smaller chunks, loads become faster, archiving and purging become easier.

### What are materialized views?
Pre-calculated query results stored physically, refreshed periodically — dramatically speed heavy reports.

### Difference between logical and physical data models?
Logical model describes business structure.  
Physical model describes technical implementation — indexes, partitions, storage engines.

### What is surrogate vs business key debate?
Business keys hold meaning but change; surrogate keys are meaningless but stable.  
Warehouses usually prefer surrogates while still storing business keys as attributes.

### Why is performance tuning critical?
Data grows endlessly — without tuning, dashboards slow, ETL fails, users lose faith.

### What are common ETL failures?
Schema drift, missing files, bad formats, network outages, logic errors, data type mismatches.

### Why is monitoring important?
Early detection prevents bad data from entering reports and helps recover before users notice issues.

### What is workload isolation?
Separating compute for ETL vs analytics so neither impacts the other — common in modern cloud platforms.

---

## 🔹 4. SCENARIO-BASED QUESTIONS

### Trend analysis for 10 years — design?
Use fact tables at proper grain, store history, partition by time, build summary tables for speed.

### Reports slow after more data — causes?
Missing partitions, poor indexing, wrong grain, heavy joins, unoptimized queries, lack of summaries.

### Sales data across systems — consolidate?
Standardize business keys, build conformed dimensions, integrate via ETL pipelines into unified facts.

### Duplicate records in analytics — why?
No deduping, inconsistent keys, poorly designed ETL, missing constraints, source duplication.

### Customer keeps changing — preserve?
Use SCD Type-2 to retain historical versions.

### ETL failed mid-run — recovery?
Checkpointing, restartable jobs, staging replays, idempotent design.

### Real-time dashboards requested — consider?
Latency expectations, cost, reliability, streaming tools, hybrid architecture.

### Single source of truth — steps?
Governance, conformed dimensions, EDW strategy, standard metric definitions.

### Data mismatch — debug?
Compare lineage, filters, time zones, transformation rules, refresh windows.

### Old vs new warehouse reports differ?
Check aggregation logic, dimension joins, calendar schemas, historical rebuilds.

### Source schema changed — impact?
Downstream breaks — adjust mappings, validations, transformations.

### New country onboard — how?
Extend dimensions, handle new currencies/taxes/calendars, validate mappings.

### Data volume doubled — response?
Scale compute, repartition, archive cold data, optimize queries.

### Sensitive fields showing — fix?
Masking, encryption, RBAC, row/column-level security.

### Execs want summarized KPIs?
Build summary fact tables or materialized views — avoid raw detail scans.

---

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS

### Is a warehouse only for structured data?
Mostly — but modern platforms handle semi-structured too.

### Is warehouse same as database?
No — it is a specialized analytical system designed differently from OLTP.

### Does normalization always help?
For OLTP yes, for DW often no — denormalization improves read performance.

### Is ELT always better?
Cloud helps, but ETL may still be needed for complex transformations and governance.

### Are indexes used like OLTP?
Less often — warehouses rely more on partitions, clustering, and columnar storage.

### Does deleting reduce storage instantly?
Usually no — space is reused or reclaimed later.

### Is star schema always best?
Often yes for analytics, but not always depending on needs and data model.

### Are historical snapshots in OLTP?
Rarely — OLTP overwrites, warehouses preserve.

### Does every warehouse need staging?
Highly recommended, but real-time ingestion may skip it intentionally.

### Can spreadsheets replace warehouses?
Never — no governance, security, scalability, or history.

### Do real-time systems remove warehouses?
No — they work together.

### Does every organization need SCD?
Only when business requires history.

### Should surrogate key change when business key changes?
No — surrogate stays stable; business key updates as attribute.

### Does reporting always use fact tables?
Often yes, but sometimes summary layers, cubes, or materialized views are used.

### Are dimensions always small?
No — customer, product, and account dimensions can be massive.
