
# ❄️ Snowflake — Data Governance, Compliance & Use Cases  
### (Questions Only — Interview & Training Reference)

---

## 1️⃣ DATA GOVERNANCE IN SNOWFLAKE

### A. Basics — What is Governance?
- What is data governance?
- Why do organizations need governance?
- What problems occur when governance does not exist?
- Who is typically responsible for governance?
- How is governance different from security?
- How does governance relate to compliance?

### B. Setting Up Governance Policies
- What is a governance policy?
- How are governance policies implemented in Snowflake?
- What does policy‑driven security mean?
- What role does role hierarchy play in governance?
- What is a masking policy?
- What is a row‑access policy?
- What is object tagging?
- How do tags support governance?
- What is data classification?
- How can Snowflake automatically detect sensitive fields?
- How do we prevent unauthorized access to PII?
- How do we audit PII usage?
- What is metadata governance and why is it important?

### C. Ensuring Data Quality & Consistency
- What is data quality?
- Why does poor data quality create financial risk?
- What are common data quality issues?
- How do database constraints help?
- How do streams and tasks assist in validation pipelines?
- What is data observability?
- Which tools support observability?
- What is data lineage?
- Why does lineage matter for audits?
- How does governance integrate into DevOps pipelines?

---

## 2️⃣ COMPLIANCE & SECURITY

### A. Regulatory Frameworks
- What is GDPR?
- What is HIPAA?
- Which data types are considered sensitive?
- What is the “right to be forgotten”?
- How does Snowflake help organizations stay compliant?
- Why is compliance different from security?

### B. Implementing Security Best Practices
- What is defense‑in‑depth?
- Why is RBAC critical to governance?
- What is encryption at rest?
- What is encryption in transit?
- Does Snowflake manage encryption keys?
- What is key rotation?
- Why should shared accounts be avoided?
- Data masking vs encryption — when to use which?
- What tools help with compliance reporting?
- Why must logs and audit data be retained?

---

## 3️⃣ SCENARIO‑BASED GOVERNANCE QUESTIONS

- Auditor asks: “Who accessed SSN last month?” — where do you look?
- Two teams need the same data but different column visibility — solution?
- Sensitive columns were exposed accidentally — what is the fix?
- Need to trace lineage of a data field — where is it captured?
- Customer requests data deletion — governance implications?
- Migrating from on‑prem to Snowflake — governance checklist?

---

## 4️⃣ GOVERNANCE MISCONCEPTIONS

- Does encryption alone guarantee compliance?
- Does RBAC automatically solve governance?
- Can compliance be achieved by simply enabling features?
- Do masking policies modify stored data?
- Does Snowflake automatically classify every dataset?
- Is governance only the IT department’s responsibility?
- Are audit logs optional?

---

# ❄️ REAL‑WORLD USE CASES & BEST PRACTICES

## 5️⃣ REAL‑WORLD USE CASES

### A. Case Studies
- Migrating on‑prem warehouse to Snowflake
- Centralizing marketing analytics
- Customer 360 analytics platform
- CDC ingestion and reporting
- Modern financial reporting
- AI / ML feature engineering
- Retail inventory analytics
- Healthcare and compliance data lake

### B. Industry‑Specific Use
- Banking & financial risk
- Retail & ecommerce analytics
- Telecom usage patterns
- Healthcare data exchange
- Insurance claims and pricing
- Manufacturing IoT analytics
- Media advertising pipelines
- Government & public analytics

---

## 6️⃣ BEST PRACTICES IN SNOWFLAKE

### A. Data Management
- Why use raw → staged → curated layers?
- Why separate ETL vs BI warehouses?
- Why enforce RBAC‑driven access models?
- Why track lineage from day one?
- Why use version control for SQL?
- Why automate deployments?

### B. Optimization
- Why avoid SELECT * ?
- Why use partition‑aware filtering?
- When to avoid clustering?
- How to monitor credit usage?
- When to use materialized views carefully?
- When Snowpark helps governance?
- How to schedule tasks safely?
- Why alerts and monitoring matter?

---

## 7️⃣ SCENARIO‑BASED — USE CASE THINKING

- CFO complains Snowflake cost is rising — where do you begin?
- Dashboards keep breaking — what governance gaps exist?
- Duplicates during migration — what to check?
- Need highly governed data lake — how do you design?
- Business requests “real‑time everything” — how do you respond?

---

## 8️⃣ COMMON MISCONCEPTIONS — USE CASES

- Is Snowflake always cheaper?
- Do more warehouses guarantee speed?
- Does governance slow delivery?
- Can BI bypass governance if needed?
- Do tools alone solve governance?
