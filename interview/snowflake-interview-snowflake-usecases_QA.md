
# Snowflake — Real‑World Use Cases & Best Practices (Detailed Q&A)

## 1️⃣ Real‑World Use Cases

### A. Case Studies

**Q: Migration from on‑prem warehouse to Snowflake — what problems does it solve?**  
Snowflake eliminates hardware capacity limits, maintenance overhead, complex scaling, and slow provisioning cycles.  
Teams gain elastic compute, separation of storage/compute, better concurrency, and built‑in security — while retiring costly legacy infrastructure.

**Q: How does Snowflake support unified marketing analytics?**  
Snowflake centralizes data from CRM, ads platforms, web logs, and email systems, enabling attribution modeling, segmentation, ROAS reporting, and real‑time campaigns without exporting datasets across tools.

**Q: What is Customer 360 in Snowflake?**  
Customer 360 combines behavioral, transactional, and demographic signals into one governed profile used by analytics, ML, and personalization — reducing fragmented customer views across systems.

**Q: How is streaming CDC handled?**  
Streams + Tasks enable incremental ingestion and transformation so only new/changed rows are processed — reducing cost and latency versus full reloads.

**Q: Why modernize financial reporting into Snowflake?**  
Finance teams gain auditability, governed access, consistent definitions, and faster closing cycles — without manual CSV merges or spreadsheet risk.

**Q: What is a Snowflake ML feature store scenario?**  
Snowflake stores curated feature tables, refreshes them incrementally, and serves them consistently to training and inference pipelines — improving reproducibility and governance.

**Q: How does Snowflake help retail inventory analytics?**  
It combines POS, logistics, vendor, and forecast data — supporting stock optimization, shrink analysis, replenishment planning, and demand forecasting.

**Q: Healthcare compliance lake — how does Snowflake fit?**  
Snowflake supports HIPAA‑aligned architectures, secure views, tokenization, and controlled sharing — enabling collaboration without exposing PHI directly.

---

## 2️⃣ Best Practices

### A. Data Management

**Q: Why use RAW → STAGED → CURATED layers?**  
Because data should evolve from untouched source → cleaned → business‑ready. This prevents accidental overwrites, supports lineage, and isolates transformations.

**Q: Why separate ETL and BI warehouses?**  
ETL workloads are heavy and long‑running; BI workloads are bursty. Separating avoids dashboard delays and improves cost tracking.

**Q: Why rely on RBAC?**  
Role‑based models avoid one‑off grants, simplify auditing, and reduce insider‑risk exposure.

**Q: Why track lineage and governance?**  
Auditors, data owners, and engineers need to see where data originated and who transformed it — especially around PII.

**Q: Why version control for SQL?**  
So changes are reviewable, reversible, and traceable — supporting DevOps workflows.

**Q: Why automate deployments?**  
Manual migrations introduce risk. CI/CD ensures consistent promotion across environments.

### B. Optimization

**Q: Why avoid SELECT * ?**  
It increases scan size, breaks downstream apps when schemas change, and prevents cache reuse.

**Q: Why partition‑aware filtering?**  
Filtering on natural keys (dates, ids) enables pruning — reducing compute.

**Q: When avoid clustering?**  
On small, volatile tables — clustering adds cost without benefit.

**Q: Why monitor credits?**  
To detect inefficient queries, runaway tasks, or oversized warehouses early.

**Q: When reconsider materialized views?**  
They cost storage + maintenance — only use when queries repeatedly benefit.

**Q: When is Snowpark better?**  
When advanced logic must run inside governed data — avoiding data export.

**Q: Why avoid recursive tasks?**  
They can create infinite loops and uncontrolled spend.

**Q: Why alerts?**  
Early detection prevents silent data corruption or missing pipelines.

---

## 3️⃣ Scenarios

**Q: CFO asks about rising Snowflake cost — what do you check?**  
Start with warehouse usage, auto‑suspend settings, failed tasks looping, materialized view refreshes, and query scans — not storage first.

**Q: Dashboards keep breaking — why?**  
Typically missing contract layers (views), uncontrolled schema changes, and lack of versioning.

**Q: Migration created duplicates — what helps?**  
Define natural/business keys, use MERGE, add dedupe logic, and validate counts vs source.

**Q: Highly governed access — what architecture?**  
Secure views, RBAC hierarchy, masking + row policies, lineage tagging, and separated environments.

**Q: “Real‑time everything” — what to say?**  
Challenge requirements. Many workloads only need near‑real‑time (minutes). True streaming adds cost and complexity — use it only where value exists.

---

## 4️⃣ Misconceptions

**Q: Is Snowflake automatically cheaper?**  
No — poor design wastes credits fast. Architecture discipline matters.

**Q: More warehouses = faster?**  
Not necessarily — sizing, pruning, and query design matter more.

**Q: Governance slows teams down?**  
Healthy governance enables faster, safer collaboration — not bureaucracy.

**Q: Can BI bypass governance?**  
They technically can — but it introduces legal and risk exposure. Use secure layers instead.

**Q: Do tools alone guarantee governance?**  
No. Governance is policies + process + architecture — tools only help implement it.
