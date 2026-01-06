
# ❄️ Snowflake with Python & Other Tools — Interview-Focused Q&A

## 🔹 1️⃣ Using Python with Snowflake

### A. Python + Connector

**How does Python connect to Snowflake?**  
Python connects using the **Snowflake Python Connector**, which opens a secure HTTPS session and sends SQL commands to Snowflake’s compute layer. You control the session programmatically instead of clicking inside the UI.

**What is the Snowflake Python Connector?**  
An official library that lets Python:
- run SQL
- fetch results
- manage transactions
- automate pipelines

**What credentials are required?**
- account
- username
- password OR key-pair (preferred in automation)
- role
- warehouse
- database
- schema

**What are connection parameters?**  
They define *where and how* Python connects, including endpoint and default context.

**How do you execute queries?**
Typical flow:
1. open connection  
2. create cursor  
3. execute SQL  
4. fetch results  
5. close connection

**How are results returned?**  
As Python objects (lists/tuples) — or Pandas frames if requested.

**Common errors to handle**
- invalid credentials
- missing warehouse / role
- expired tokens
- network failure

**Can Python manage transactions?**  
Yes — disable auto‑commit and explicitly use BEGIN / COMMIT / ROLLBACK.

---

### B. Pandas + Snowflake

**What is Pandas?**  
An in‑memory analytics library for Python.

**Why combine Pandas with Snowflake?**
- quick exploration
- ML prep
- lightweight transformation

**What is write_pandas()?**  
Helper that bulk‑loads DataFrames efficiently using staging + COPY internally.

**Best way to push data into Snowflake?**  
Bulk — not row‑by‑row inserts.

**How to read Snowflake into Pandas?**  
Run query → `.fetch_pandas_all()`.

**Why avoid downloading huge tables?**  
Pandas is RAM‑limited — large datasets crash locally.

**When should work stay inside Snowflake?**
- large datasets
- repeatable pipelines
- governed data

---

### C. Snowpark (Python)

**What is Snowpark?**  
A framework that lets Python code run *inside Snowflake compute* rather than locally.

**How is it different from the Python Connector?**
| Connector | Snowpark |
|---|---|
| Sends SQL | Builds execution plans |
| Runs on client | Runs inside Snowflake |
| Great for automation | Great for heavy transforms |

**Why process inside Snowflake?**
- security
- scalability
- zero data movement

**UDF vs Stored Procedure**
- UDF → reusable function inside SQL
- Procedure → multi‑step logic and flow control

**What are Snowpark DataFrames?**  
Logical query plans — executed lazily only when needed.

**Snowpark for ML?**  
Great for feature engineering + scoring. Heavy GPU training usually external.

**When NOT to use Snowpark?**
- small exploratory tasks
- GPU‑heavy deep learning
- when SQL solves the problem already

---

## 🔹 2️⃣ Boto3 & AWS Integration

### A. Boto3 Basics

**What is Boto3?**  
AWS SDK for Python.

**Why connect AWS + Snowflake?**
- move data between S3 and Snowflake
- automate pipelines
- manage data lakes

**Authentication approaches**
1. IAM roles (recommended)
2. Access keys (higher risk)

IAM roles rotate automatically — safer.

---

### B. Data Movement & Automation

**How do we move data?**
- PUT / GET files
- COPY INTO staging
- Python orchestration

**How to unload to S3?**  
COPY INTO external stage.

**Main migration challenges**
- schema mismatches
- null handling
- ordering differences

**How to validate**
- counts
- checksums
- reconciliation queries

---

## 🔹 3️⃣ Building Applications with Snowflake

### A. Pipelines

**What is a pipeline?**  
Ingest → Transform → Publish

**Streams + Tasks + Python**
- Streams detect changes
- Tasks schedule work
- Python orchestrates + logs

---

### B. Machine Learning

**Snowflake for ML?**
Yes — especially:
- feature engineering
- model scoring

**When to export data?**
- GPU training
- custom ML workflows

**Feature Store**
Central governed dataset serving ML models consistently.

---

### C. Web Apps

**Should apps connect directly?**  
Prefer API layer in front.

Benefits:
- protects credentials
- limits traffic
- applies governance

---

## 🔹 4️⃣ BI Tool Integration

**Supported tools**
Power BI, Tableau, Looker, Qlik — via JDBC/ODBC.

**Live vs Extract**
- Live → queries Snowflake each request
- Extract → cached dataset

**Security**
RBAC always applies — BI cannot bypass Snowflake security.

---

## 🔹 5️⃣ Scenario Questions

**ML scoring daily — where?**  
Prefer inside Snowflake using Snowpark to avoid exporting full datasets.

**Python job fails from expired credentials — fix?**  
Move to key‑pair or OAuth automation.

**Pandas crashes — what now?**  
Keep processing inside Snowflake or Snowpark.

**Dashboard slow — where to check?**
- warehouse size
- caching
- joins
- concurrency

**Move Snowflake → S3 → DB**  
Unload using COPY, then load downstream safely.

---

## 🔹 6️⃣ Misconceptions

❌ Python replaces SQL  
✔ Python orchestrates — Snowflake processes

❌ Snowpark = Spark  
✔ Snowpark runs inside Snowflake; Spark runs externally

❌ Boto3 loads automatically  
✔ You still design staging + COPY flows

❌ BI bypasses access  
✔ RBAC enforced always

---

## 🎯 Key Takeaways

✔ Python = automation & orchestration  
✔ Snowflake remains the compute engine  
✔ Snowpark prevents unnecessary data movement  
✔ BI sits on top and respects governance  
✔ Architecture choices are driven by security first
