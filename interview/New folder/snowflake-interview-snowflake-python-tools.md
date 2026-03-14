
# ❄️ Snowflake with Python & Other Tools — Question Bank (Questions Only)

## 🔹 1️⃣ Using Python with Snowflake
### A. Python + Connector
- How does Python connect to Snowflake?
- What is the Snowflake Python Connector?
- What credentials are required for Python authentication?
- What are connection parameters?
- How to execute queries from Python?
- How to fetch results into Python objects?
- What are common error handling considerations?
- Can Python manage transactions in Snowflake?

### B. Pandas + Snowflake
- What is Pandas?
- Why integrate Pandas with Snowflake?
- What is write_pandas()?
- What is the best way to push data frames to Snowflake?
- How to read Snowflake tables into Pandas?
- Why avoid pulling huge datasets into Pandas?
- When should we keep processing inside Snowflake instead?

### C. Snowpark (Python)
- What is Snowpark?
- How is Snowpark different from Python Connector?
- Why process data inside Snowflake instead of locally?
- What is UDF vs Stored Procedure in Snowpark?
- What are DataFrames in Snowpark?
- What is lazy evaluation?
- Can Snowpark run ML libraries?
- Why is Snowpark good for governance and security?
- When NOT to use Snowpark?

## 🔹 2️⃣ Boto3 & Snowflake (AWS Integration)
### A. Boto3 Basics
- What is Boto3?
- Why integrate Snowflake with AWS?
- What are common scenarios (S3 upload/download)?
- How does authentication work with AWS + Snowflake?
- What is IAM role?
- What is key-based access?
- Why prefer IAM roles over access keys?

### B. Data Movement & Automation
- How can Python move data between Snowflake and S3?
- How to unload Snowflake data to S3?
- How to trigger Snowflake loads from AWS pipelines?
- What about migrating between RDBMS systems?
- What challenges occur with schema mismatch?
- How to validate migrations?

## 🔹 3️⃣ Building Applications with Snowflake
### A. Data Pipelines
- What is a pipeline?
- How do Streams + Tasks + Python work together?
- Why use Python orchestration instead of SQL-only solutions?
- How to log pipeline execution?
- What are retries and error recovery strategies?

### B. Machine Learning & AI
- Can Snowflake be used for ML workloads?
- What ML tasks run best inside Snowflake?
- When to export data out to ML platforms?
- How does Snowpark support ML?
- Can models be deployed back into Snowflake?
- What is feature store concept?

### C. Web Applications
- Can apps directly connect to Snowflake?
- When should Snowflake sit behind an API?
- What performance considerations exist?
- How to secure application credentials?
- How to manage connection pooling?

## 🔹 4️⃣ BI Tool Integration
### A. Connecting BI Tools
- What BI tools integrate with Snowflake?
- What is ODBC vs JDBC?
- How do Tableau and Power BI connect?
- What is live connection vs extract?
- Which approach is better and when?
- Does BI access respect RBAC rules?

### B. Reports & Dashboards
- How do dashboards refresh from Snowflake?
- Why warehouse choice matters for BI?
- What is query caching benefit in BI dashboards?
- What limits exist for high concurrency dashboards?
- When to separate BI warehouse from ETL warehouse?
- How to troubleshoot slow reports?

## 🔹 5️⃣ Scenario-Based Questions
- Need daily ML feature update — export or process inside Snowflake?
- Python job failing due to credential expiry — fix?
- Large Pandas dataframe crashes memory — alternative?
- BI dashboard suddenly slow — tuning checklist?
- Move data from Snowflake → S3 → another DB — safest workflow?
- Real-time ingestion required — Snowpark or connector?
- Business wants ML scoring in Snowflake — options?

## 🔹 6️⃣ Trick / Misconception Questions
- Does Python always replace SQL in Snowflake?
- Is Snowpark just like Spark?
- Does Boto3 automatically move data into tables?
- Can BI tools bypass access controls?
- Is Pandas faster than Snowflake always?
- Can you run Python code directly inside the Snowflake UI?
- Are APIs safer than direct database access by default?
- Does Snowflake become an application server?
