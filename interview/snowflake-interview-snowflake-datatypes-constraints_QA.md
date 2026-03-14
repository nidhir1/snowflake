# Snowflake — Constraints & Data Validation  
## Full Teacher-Style Q&A (Detailed)

... (content above remains the same until scenario section) ...

## 🔟 SCENARIO‑BASED QUESTIONS

### ❓ Customer emails should never be NULL — how do we ensure this?
In Snowflake, NOT NULL will not physically block NULL values.  
So the correct mindset is:

**Design rule lives in metadata — enforcement lives in pipelines.**

Best practice approach:
1. Define the column as NOT NULL (for documentation + BI tools).  
2. Add validation in ETL:
   - reject records with missing email
   - or route them to a quarantine/error table
3. Include monitoring so violations are detected early.

Lesson:
> Constraints describe expectations. Pipelines enforce behavior.

---

### ❓ Orders table must always link to a customer — what constraint concept applies?
This is a **FOREIGN KEY relationship**.

Conceptually:
orders.customer_id → customers.customer_id

But Snowflake won’t stop an “orphan order” from being inserted.

So ETL should:
- validate customer exists before inserting
- log failures
- optionally block load

---

### ❓ Numbers are rounding incorrectly — which type should we use?
Use **NUMBER/DECIMAL**, not FLOAT.

FLOAT is approximate and introduces rounding noise, which becomes very visible in financial reporting.

Teach your team:
> FLOAT = science calculations  
> NUMBER = business and money

---

### ❓ Financial calculations show precision issues — FLOAT or NUMBER?
Always NUMBER.

Finance requires auditability and exact precision.

---

### ❓ JSON logs contain dynamic fields — how should we store them?
Store initially in **VARIANT**.

Then:
- identify frequently-used attributes
- model those fields into structured columns later

This is the healthy lifecycle:
1. land JSON fast
2. observe real usage
3. normalize what matters

---

### ❓ ETL inserts empty strings instead of NULL — what is the impact?
Dashboards show blanks instead of “missing” values.

This leads to:
- incorrect counts
- misinterpreted completeness
- silent data quality problems

Fix in ETL:
- standardize blanks → NULL where appropriate

---

### ❓ Need to analyze delivery routes — which data type should we use?
Use **GEOGRAPHY**.

Benefits:
- validates shapes
- supports distance calculations
- allows advanced spatial queries

Avoid storing coordinates as plain text when spatial analysis is required.

---

### ❓ Team wants constraints enforced — where should they enforce instead?
Enforce using:
- ETL pipelines
- dbt tests
- DQ frameworks (Great Expectations, Soda, etc.)

Snowflake stores rules — pipelines enforce rules.

---

### ❓ Analysts need to understand relationships — how do constraints help?
Even though not enforced, constraints:

- reveal PK/FK relationships visually
- improve BI auto-joins
- reduce onboarding time
- communicate business meaning

They are part of good data modeling discipline.

---

## 1️⃣1️⃣ TRICK / MISCONCEPTIONS

### ❌ “PRIMARY KEY enforces uniqueness in Snowflake.”
Wrong — it does NOT enforce anything.

PRIMARY KEY is **informational only**.

---

### ❌ “Snowflake rejects invalid foreign key relationships.”
Wrong — orphan rows are allowed.

That is why pipeline validation exists.

---

### ❌ “NOT NULL guarantees clean data.”
No — NULLs can still appear if upstream systems insert them.

---

### ❌ “VARIANT means schema-less forever.”
No — schema simply moves from **write time** to **read time**.

Eventually, high‑value JSON fields should be modeled.

---

### ❌ “Using JSON means we don’t need data modeling anymore.”
Completely false.

Modeling becomes **more important**, not less — otherwise analytics becomes unmanageable.

---

### ❌ “Constraints are useless in a data warehouse.”
Incorrect.

They provide:
- documentation
- clarity
- BI intelligence
- lineage support
- shared understanding

Snowflake intentionally separates **description** from **enforcement** — but both still matter.

---

## 🎯 Key Takeaway
> Constraints tell the truth about how data *should* behave.  
> Pipelines ensure data actually behaves that way.
