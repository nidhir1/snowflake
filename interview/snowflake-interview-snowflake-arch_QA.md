# ❄️ Snowflake — Editions, Architecture & Pricing


---

## 🔹 1. SNOWFLAKE EDITIONS / VERSIONS

---

### ⭐ BASIC

#### What are Snowflake editions (versions)?
Snowflake editions are feature tiers built on the same platform engine.  
They don’t change performance — they unlock governance, security, recovery, and enterprise controls.

#### Why does Snowflake offer multiple editions?
Different organizations have different risk and compliance needs.  
A startup should not pay for the same controls required by a bank.

#### Name the main Snowflake editions.
- Standard  
- Enterprise  
- Business Critical  
- Virtual Private Snowflake (VPS) / Government

#### What is Standard edition?
Entry-level production analytics:
- warehouses
- secure storage
- RBAC
- 1-day Time Travel
- basic sharing

#### What is Enterprise edition?
Adds governance and scalability:
- Time Travel up to 90 days
- multi-cluster warehouses
- resource monitors
- replication options
- better auditing

#### What is Business Critical edition?
Designed for regulated industries:
- enhanced encryption
- isolation options
- stronger auditing guarantees
- compliance certifications

#### What is VPS / Government edition?
Highly isolated environment for national/government workloads.

#### Why would companies upgrade editions?
Sensitive data, audits, DR, multiple regions, more users, or stricter governance.

#### What increases when editions increase?
Security, governance, protection, replication, compliance.  
Performance does not change.

#### Do all editions run on the same architecture?
Yes — same engine, different capabilities.

---

### ⭐ INTERMEDIATE

#### What extra features come with Enterprise?
- extended Time Travel
- replication
- failover
- multi-cluster warehouses
- resource monitoring
- masking/classification

#### What is extended Time Travel?
Ability to restore deleted or modified data for up to 90 days.

#### What is multi-cluster warehouse support?
Automatically adds clusters when concurrency spikes.

#### What are resource monitors?
Credit guardrails that alert and suspend workloads when thresholds hit.

#### Governance/security differences?
Higher editions unlock masking, tagging, enhanced encryption, audit depth.

#### What is database replication?
Copies databases across accounts/regions/clouds.

#### What is failover/failback?
Switch operations to replicas and revert later.

#### Why banks prefer Business Critical?
Auditable encryption, isolation, and strict guarantees.

#### When should you upgrade?
When Snowflake becomes core system or compliance increases.

---

### ⭐ ADVANCED THINKING

#### How do editions affect cost?
Higher editions increase per-credit price and unlock features that may introduce costs.

#### Can you downgrade?
Possible, but risky — features relying on higher tiers may stop working.

#### Do all regions support all editions?
Most do — regulated editions limited to specific regions.

#### How do editions align with compliance?
- Standard — minimal  
- Enterprise — corporate baseline  
- Business Critical — regulated  
- VPS — strict isolation

#### How to choose?
Balance risk, cost, DR needs, and governance maturity.

---

## 🔹 2. SNOWFLAKE ARCHITECTURE

---

### ⭐ BASIC

#### What is Snowflake’s architecture called?
Multi-Cluster Shared Data Architecture.

#### Three layers?
Storage • Compute • Services.

#### Storage layer
Columnar, compressed, encrypted, automatically partitioned.

#### Compute layer
Virtual warehouses that execute workloads independently.

#### Services layer
Optimization, metadata, authentication, governance.

#### What are micro-partitions?
Small immutable blocks that enable pruning and eliminate indexing needs.

#### Shared data?
Single version — no duplicate datasets required.

#### Compute independent?
Compute scales or pauses without affecting storage.

#### Virtual warehouse?
Elastic compute engine that resizes, pauses, resumes.

#### Stop a warehouse?
Compute billing stops; storage continues.

---

### ⭐ INTERMEDIATE

#### Internal storage?
Columnar + compressed + encrypted.

#### Columnar?
Reads only needed columns — efficient analytics.

#### Compression?
Reduces cost and speeds I/O.

#### Automatic partitioning?
Handled by Snowflake — no DBA tuning.

#### Metadata?
Stats, partition info, lineage, history.

#### Pruning?
Skip partitions that cannot match filters.

#### Concurrency?
Workload isolation via warehouses/clusters.

#### Multi-cluster warehouses?
Auto add compute when demand increases.

#### Caching layers?
Result cache • Data cache • Metadata cache.

#### High availability?
Self-healing infrastructure — failures recover automatically.

---

### ⭐ ADVANCED

#### Multi-cluster shared data?
Multiple compute clusters share one storage layer — zero contention.

#### Difference from shared-disk/shared-nothing?
Avoids contention and complexity.

#### Cross-region/cloud replication?
Protects availability and supports global use.

#### Failover/failback?
Temporary switch and controlled return.

#### Zero-downtime upgrades?
Rolling updates while workloads continue.

#### Services layer role?
Optimizer, metadata, orchestration, RBAC enforcement.

#### Why no indexes?
Micro-partitions + metadata pruning replace them.

---

## 🔹 3. PRICING MODEL

---

### ⭐ BASIC

#### Subscription or consumption?
Primarily consumption — optional capacity commitment.

#### Credit?
Unit of compute cost — bigger warehouses burn more.

#### What uses credits?
Warehouses, serverless tasks, replication, MVs.

#### Storage charged in credits?
No — billed per TB/month.

#### Two billing categories?
Compute + Storage.

#### Storage billing?
Compressed size averaged daily.

#### Compute billing?
Per second, 60-second minimum.

#### Pay-as-you-go?
Pause compute → stop compute billing.

#### Capacity pricing?
Pre-purchase credits at discount.

---

### ⭐ INTERMEDIATE

#### Warehouse cost depends on?
Size • runtime • cluster count.

#### Double size = double cost?
Per hour yes — but total may drop if jobs finish faster.

#### Auto-suspend?
Stops idle compute charges.

#### Separate ETL/BI?
Prevents contention and clarifies cost.

#### Failed queries?
Still cost compute.

#### Cost-increasing features?
Replication, materialized views, serverless services.

#### Time Travel?
More retention → more storage.

#### Fail-safe?
Emergency recovery layer; adds temporary storage.

#### Data transfer?
Cross-region/cloud egress may cost.

#### Resource monitors?
Budget protection and automatic suspension.

---

### ⭐ ADVANCED

#### Scale up vs out?
Up = faster single query; Out = more users.

#### Concurrency scaling cost?
More clusters → more credits.

#### Inefficient SQL?
Unnecessary scanning increases cost.

#### Retention abuse?
Old data/clones silently inflate storage.

#### Data sharing cost?
Provider pays storage; consumers pay compute.

#### Cloning?
Cheap initially — diverging changes add cost.

#### Best practices?
Short suspend timers, right-size, isolate workloads, monitor, manage lifecycle.

---

## 🔹 4. SCENARIOS (GUIDED THINKING)

- Bill rising → check warehouses, suspend, MVs, replication  
- Idle compute → enable auto-suspend  
- ETL blocking BI → separate warehouses  
- Global rollout → replication + reader accounts  
- Compliance → Business Critical  
- Bigger warehouse no help → fix SQL/model  
- Long retention → archival strategy  
- Too many clones → monitor divergence  

---

## 🔹 5. MISCONCEPTIONS — CLARIFIED

- Bigger warehouse ≠ always faster  
- Auto-suspend stops compute, not storage  
- Fail-safe ≠ backup replacement  
- Cloning not free forever  
- Time Travel stores only changes  
- No queries ≠ zero cost  
- Higher edition ≠ performance boost  
- Cloud ≠ always cheaper  
- Replication always costs  
- Prices vary across regions/clouds

---
