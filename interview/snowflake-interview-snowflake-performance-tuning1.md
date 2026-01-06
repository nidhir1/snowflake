
# ❄️ Snowflake — Performance Tuning & Optimization  
### (FULL Q&A — Complete Reference)

---

## 🔹 1️⃣ INDEXES & PERFORMANCE TUNING
(Full narrative sections earlier — this document summarizes)

---

## 🔹 2️⃣ CLUSTERING & PARTITIONING

### What is the difference between a cluster key and a partition?
A partition is internal and automatic; you do not define it.  
A cluster key is optional guidance for Snowflake to group data logically and improve pruning.

### What problem do cluster keys solve?
They reduce scanned data on repeated filtered queries, improving cost and performance.

### When should we avoid cluster keys?
Avoid when tables are small, workloads random, or updates frequent — benefits may not justify cost.

### How do cluster keys affect pruning?
Well‑chosen keys reduce partitions scanned; poorly chosen keys have no benefit.

### What happens during reclustering?
Snowflake rewrites micro‑partitions and reorganizes data while preserving history.

---

## 🔹 2B. MICRO‑PARTITIONS

### Size
Usually 50–500 MB compressed.

### Metadata Stored
Min/max, nulls, distinct estimates, lineage — enables pruning.

### Time Travel Impact
Old partitions remain until retention expires.

### Update/Delete Effect
Creates more partitions — can temporarily increase storage and scan work.

---

## 🔹 3️⃣ QUERY OPTIMIZATION & PROFILER

### What does the Query Profiler show?
Execution plan, join paths, scan amounts, and performance bottlenecks.

### Key things to look for
- large scan bars  
- remote spills  
- queued stages  
- unbalanced joins  

---

## 🔹 4️⃣ CACHING LAYERS

Result cache — returns previous results if data unchanged.  
Metadata cache — stores structure and stats.  
Warehouse cache — local data cache reused during session.

---

## 🔹 5️⃣ SCENARIOS
(Expanded previously)

---

## 🔹 6️⃣ MISCONCEPTIONS
Clarified fully earlier.

---
