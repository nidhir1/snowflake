# ❄️ Snowflake — Tasks, Partitions, Stages & Bulk Loading
## (Questions Only — Interview & Study Reference)

---

## 🔹 1️⃣ SNOWFLAKE TASKS & PARTITIONS (TASK AUTOMATION)

### A. TASKS — BASICS
What is a Snowflake Task?  
Why do we use tasks?  
What kind of SQL statements can tasks run?  
How do tasks help automate ETL?  
Do tasks require a warehouse?  
What happens if warehouse is suspended?  
What is task owner?  

### B. REAL-TIME USES OF TASKS
What is scheduled ETL using tasks?  
How do tasks integrate with streams?  
Can tasks trigger incrementally?  
How do tasks ensure exactly-once processing?  
What is task history?  
How do you identify failed tasks?  
What happens when task execution overlaps?  
Can tasks be paused?  

### C. DAG (DIRECTED ACYCLIC GRAPH)
What is a DAG?  
Why do tasks use DAG dependency?  
What is parent task?  
What is child task?  
What happens when parent task fails?  
Why circular dependencies are not allowed?  
How do dependent tasks pass execution order?  
How do you visualize DAG execution?  

### D. TASK SCHEDULES & RESUME
What scheduling options exist for tasks?  
What is CRON scheduling?  
What is time-based scheduling?  
Can tasks run every minute?  
What is automatic resume?  
What happens if system is down during schedule?  
Who can enable/disable tasks?  

---

## 🔹 2️⃣ PARTITIONING — MICRO-PARTITIONS & CLUSTERING

### A. MICRO-PARTITIONS — BASICS
What are micro-partitions?  
How big is a micro-partition typically?  
How are rows assigned to micro-partitions?  
Does Snowflake require manual partitioning?  
What metadata is stored about micro-partitions?  
What is pruning?  
Why does pruning improve performance?  
How does CDC affect partitions?  

### B. CLUSTERING
What is a cluster key?  
How cluster keys differ from partitions?  
When do we need clustering manually?  
What is reclustering?  
What is automatic clustering?  
Does clustering cost credits?  
When should we NOT use clustering?  
How clustering helps heavy scan workloads?  
What indicates a clustering problem?  
How to monitor clustering depth?  

### C. INTERNAL PARTITION TYPES & USAGE
What internal partition structures exist?  
How does Snowflake internally manage partitions?  
What happens when tables grow very large?  
How does Time Travel interact with partitions?  
How does delete/update create partition overhead?  
Why do poorly designed keys increase cost?  

---

## 🔹 3️⃣ STAGES & BULK LOADING

### A. STAGES — BASICS
What is a stage?  
Why are stages used instead of loading directly?  
What are internal stages?  
What are external stages?  
What is a user stage?  
What is a table stage?  
What is a named stage?  
Which stage is used when GUI uploads files?  

### B. FILE FORMATS & COPY COMMAND
What is a file format object?  
What formats are supported?  
Why define reusable file formats?  
What is COPY INTO table?  
What does ON_ERROR do?  
What is VALIDATION MODE?  
How to avoid duplicate loads?  
What are metadata columns ($FILE, $ROW)?  
How to load semi-structured data?  

### C. BULK LOAD WORKFLOWS
What is bulk loading?  
Why small files hurt performance?  
Why Snowflake prefers compressed files?  
What is parallel loading?  
How does COPY scale automatically?  
How to monitor load performance?  
What is unloading data?  

---

## 🔹 4️⃣ SCENARIO-BASED QUESTIONS
Need to automate CDC ingestion — which features?  
ETL should run after upstream load — how do you chain tasks?  
Table scan takes too long — what tuning steps?  
Lots of DELETE operations slow queries — why?  
Need fast loading from S3 — which stage setup?  
Data duplicated after load — how to prevent?  
Task ran but target table didn’t update — where to debug?  
Queries reading unnecessary partitions — cause & fix?  
Bulk load jobs failing due to file size — what adjustment?  
Want daily automated load — task or manual scripts?  

---

## 🔹 5️⃣ TRICK / MISCONCEPTION QUESTIONS
Do tasks replace Airflow or ETL tools?  
Do tasks run without warehouses?  
Do micro-partitions behave like traditional indexes?  
Does clustering always make queries faster?  
Does Time Travel create physical duplicate partitions?  
Do stages store permanent data copies?  
Does COPY INTO automatically delete source files?  
Are smaller files always faster to load?  
Does clustering fix bad SQL?  
Does every table require clustering?  

---

## 🎯 TEACHING SUMMARY
✔ Tasks automate pipelines and support DAG execution  
✔ Micro-partitions enable pruning, not manual management  
✔ Clustering is optional and situational  
✔ Stages enable scalable loading  
✔ COPY INTO drives reliable bulk ingress
