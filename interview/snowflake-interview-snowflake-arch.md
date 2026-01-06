# ❄️ Snowflake — Editions, Architecture & Pricing
## QUESTIONS ONLY

---

## 🔹 1. SNOWFLAKE EDITIONS / VERSIONS

### ⭐ BASIC
- What are Snowflake editions (versions)?
- Why does Snowflake offer multiple editions?
- Name the main Snowflake editions.
- What is the Standard edition?
- What is Enterprise edition?
- What is Business Critical edition?
- What is Virtual Private Snowflake (VPS) / Government editions?
- Why would companies upgrade editions?
- What features increase as editions increase?
- Do all editions run on the same Snowflake architecture?

### ⭐ INTERMEDIATE
- What extra features come with Enterprise over Standard?
- What is extended Time Travel?
- What is multi-cluster warehouse support?
- What are resource monitors?
- What governance/security features differ by edition?
- What is database replication and when is it available?
- What is failover / failback replication?
- What advanced data protection features exist?
- Why might banks prefer Business Critical edition?
- When should an organization consider upgrading editions?

### ⭐ ADVANCED THINKING
- How do editions affect cost?
- Can you downgrade editions?
- Do all regions support all editions?
- How do editions align with compliance requirements?
- How to choose the right edition for a project?

---

## 🔹 2. SNOWFLAKE ARCHITECTURE

### ⭐ BASIC
- What is Snowflake’s architecture called?
- What are the three main Snowflake layers?
- What is the storage layer?
- What is the compute layer?
- What is the services layer?
- What are micro-partitions?
- What does “shared data” mean?
- What does “compute independent” mean?
- What is a virtual warehouse?
- What happens when you stop a warehouse?

### ⭐ INTERMEDIATE
- How does Snowflake store data internally?
- What does columnar storage mean?
- Why does Snowflake compress data?
- How does Snowflake achieve automatic partitioning?
- What metadata does Snowflake store?
- What role does metadata play in pruning?
- How does Snowflake achieve concurrency?
- What are multi-cluster warehouses?
- What is workload isolation?
- How does Snowflake handle caching (3 levels)?
- What is result cache?
- What is data cache?
- What is metadata cache?
- How does Snowflake ensure high availability?
- What happens if a node fails?

### ⭐ ADVANCED
- Explain multi-cluster shared data architecture.
- How is it different from shared-disk and shared-nothing?
- How does Snowflake replicate data across regions?
- What is cross-cloud replication?
- What is failover & failback?
- How does Snowflake provide zero-downtime upgrades?
- What role does services layer play in optimization?
- How does Snowflake enforce RBAC across architecture?
- How does Snowflake achieve elasticity?
- Why does architecture eliminate need for indexes?

---

## 🔹 3. SNOWFLAKE PRICING MODEL

### ⭐ BASIC
- Is Snowflake subscription-based or consumption-based?
- What is a Snowflake credit?
- What resources consume credits?
- Does storage cost use credits?
- What are the two main billing categories?
- How is storage billed?
- How is compute billed?
- What is pay-as-you-go?
- What is on-demand pricing?
- What is capacity (pre-purchase) pricing?

### ⭐ INTERMEDIATE
- What determines warehouse cost?
- What is warehouse size (XS, S, M, etc.)?
- Does doubling warehouse size double cost?
- Does auto-suspend reduce cost?
- What is auto-resume?
- What happens when warehouses sit idle?
- Why should ETL and BI have separate warehouses?
- Do failed queries still cost money?
- What features increase cost automatically?
- How do materialized views affect cost?
- How does Time Travel impact storage cost?
- How does Fail-safe impact storage?
- What are data transfer costs?
- What is cost of replication?
- What are resource monitors?

### ⭐ ADVANCED COST THINKING
- When should you scale up vs scale out?
- How do concurrency scaling and multi-cluster affect cost?
- Why can query inefficiency increase compute cost?
- Why can bad retention policies increase storage cost?
- How do governance features affect cost?
- How do data sharing costs work?
- What happens to cost when environments are cloned?
- What are best practices to control Snowflake cost?

---

## 🔹 4. SCENARIO-BASED — REAL PROJECT THINKING
- Finance complains about rising Snowflake bill — where do you investigate?
- Warehouses remain idle for long periods — what do you configure?
- ETL pipeline blocks dashboards — what architectural fix helps?
- A company expands globally — which replication features help?
- Application requires strong compliance — which edition fits?
- Queries slowed down, team increased warehouse size — cost exploded — what mistake?
- Data retention needs increased from 1 year to 7 — architectural impact?
- Users create many clones — what should be monitored?
- Real-time ingestion needed — how does pricing behave?
- Need disaster recovery cross-region — cost considerations?

---

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS
- Does bigger warehouse always mean faster queries?
- Does auto-suspend stop storage charges?
- Is Fail-safe a free backup solution?
- Does cloning duplicate full storage?
- Does Time Travel store extra copies of data always?
- Does Snowflake charge when no queries are running?
- Does Snowflake automatically optimize every bad query?
- Do larger editions always perform faster?
- Does scaling compute reduce storage costs?
- Is Snowflake always cheaper than on-prem?
- Does replication guarantee zero cost impact?
- Are all regions priced the same?
