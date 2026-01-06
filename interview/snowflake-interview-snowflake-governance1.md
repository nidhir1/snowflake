
# ❄️ Snowflake — Data Governance, Compliance & Use Cases  
### (Full Detailed Q&A — Teaching + Interview Guide)

---

## 1️⃣ DATA GOVERNANCE IN SNOWFLAKE

### A. Basics — What is Governance?

**What is data governance?**  
Data governance is the set of rules, processes, and controls that define **how data is created, stored, accessed, protected, and retired**.  
It ensures data remains **trustworthy, secure, compliant, and auditable** throughout its life cycle.

**Why do organizations need governance?**  
Without consistent rules, different teams store and interpret data differently. Governance ensures:  
- consistent definitions  
- controlled access  
- clear ownership  
- predictable quality  
- regulatory compliance  

**What happens without governance?**  
Common failures include:  
- duplicate data everywhere  
- inconsistent numbers in reports  
- uncontrolled access to sensitive data  
- higher security risk  
- inability to pass audits  

**Who is responsible for governance?**  
Typical roles include:  
- Chief Data Officer (CDO)  
- Data Governance Committee  
- Security & Compliance Teams  
- Data Owners & Stewards  
- Platform / Snowflake Admins  

**How does governance relate to security and compliance?**  
Governance defines *rules*.  
Security enforces *technical controls*.  
Compliance ensures those controls satisfy legal requirements.

---

### B. Setting Up Governance Policies

**What is a governance policy?**  
A governance policy defines **who can do what with which data** and under what conditions.

**How are governance policies implemented in Snowflake?**  
Snowflake governance uses:  
- RBAC (role‑based access control)  
- masking policies  
- row access policies  
- secure views  
- tags and classifications  
- auditing and logs  

**What is policy‑driven security?**  
Rather than granting permissions case‑by‑case, rules are centrally defined and applied consistently.

**What role does hierarchy play in governance?**  
Roles inherit permissions — so design must reflect your organization structure, not one‑off grants.

**What are masking policies?**  
Masking policies hide sensitive fields dynamically based on role.  
Example: SSN shows full only to auditors, partially masked to analysts.

**What are row‑access policies?**  
They restrict rows based on conditions — for example, a manager only sees employees in their region.

**What is object tagging?**  
Tags label objects (PII, Confidential, Finance, etc.). Tags help:  
- automate policies  
- classify risks  
- track sensitive fields  

**What is data classification?**  
Snowflake can scan fields and flag likely PII or financial content using machine learning.

**How do we restrict access to PII?**  
Using:  
- masking policies  
- secure views  
- RBAC roles  
- row access rules  

**How do we audit sensitive data usage?**  
ACCOUNT_USAGE / ACCESS_HISTORY shows who queried what and when.

**What is metadata governance?**  
Managing descriptions, lineage, tags, owners — so data remains understandable and traceable.

---

### C. Ensuring Data Quality & Consistency

**What is data quality?**  
The accuracy, completeness, and reliability of data.

**Why does poor data quality cost money?**  
Bad decisions, failed analytics, and incorrect reports all have financial impact.

**How can constraints help?**  
NOT NULL, UNIQUE, CHECK constraints describe expectations — validation tools ensure enforcement.

**How do streams and tasks assist validation?**  
They track changes and automate checks before publishing into curated zones.

**What is observability?**  
Continuous monitoring of freshness, volume, schema drift, anomalies, failures.

**What is lineage and why it matters?**  
Lineage shows **where data originated** and **how transformations changed it** — critical for audits and debugging.

---

## 2️⃣ COMPLIANCE & SECURITY

### A. Regulatory Frameworks

**What is GDPR / HIPAA?**  
Frameworks protecting European personal data (GDPR) and U.S. healthcare data (HIPAA).

**What types of data are protected?**  
PII, financial data, medical details, customer identifiers.

**What is the right‑to‑be‑forgotten?**  
Users can demand deletion — governance ensures traceability and controlled removal.

**Why compliance is not security**  
Compliance proves controls exist. Security ensures they are actually effective.

---

### B. Implementing Security Best Practices

Defense‑in‑depth means multiple protective layers:  
RBAC → policies → encryption → monitoring → auditing.

Snowflake automatically encrypts data:  
- at rest  
- in transit  
- with managed key rotation  

Shared accounts must be avoided — auditing becomes impossible.

Masking hides values at query time. Encryption protects values at storage level.

Audit logs must be retained because they reconstruct user activity during investigations.

---

## 3️⃣ SCENARIO‑BASED GOVERNANCE QUESTIONS

**Auditor asks who accessed SSN — solution:**  
Query ACCESS_HISTORY joined with tags on sensitive columns.

**Two teams need different visibility:**  
Secure views + masking + row access policy.

**Sensitive column leaked:**  
Apply masking, revoke roles, review grants, notify audit teams.

**Customer deletion:**  
Trace lineage, ensure downstream purges, maintain records of deletion logs.

---

## 4️⃣ GOVERNANCE MISCONCEPTIONS

- Encryption alone is not compliance.  
- RBAC alone is not governance.  
- Governance cannot be “enabled” with a switch.  
- Masking does not change stored data.  
- Snowflake does not classify everything automatically.  
- Governance involves business + IT.  
- Audit logs are never optional.

---

# ❄️ REAL‑WORLD USE CASES & BEST PRACTICES

## 5️⃣ USE CASES

Snowflake enables:  
centralized analytics, ML feature stores, financial reporting, CDC ingestion, retail insights, healthcare data lakes — all while governed.

---

## 6️⃣ BEST PRACTICES

Layered data zones, separated warehouses, RBAC enforcement, version control, CI/CD, cost monitoring, and automation — build mature platforms.

---

## 7️⃣ SCENARIOS & 8️⃣ MISCONCEPTIONS

Real‑life troubleshooting covers:  
cost spikes, broken dashboards, migration duplications, governance gaps, unrealistic expectations about “real‑time everything.”

Snowflake is powerful — but design discipline matters.
