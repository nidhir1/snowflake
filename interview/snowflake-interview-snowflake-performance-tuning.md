❄️ Snowflake — Performance Tuning & Optimization
(Questions Only — Study & Interview Prep)
🔹 1️⃣ INDEXES & PERFORMANCE TUNING
A. Indexes in Snowflake

Does Snowflake support traditional indexes?

Why doesn’t Snowflake use OLTP-style indexes?

What replaces indexes in Snowflake?

What are micro-partitions?

What is partition pruning and how does it work?

Does Snowflake support secondary indexes?

What happens when workloads require faster lookups?

When is Search Optimization Service appropriate?

B. General Performance Tuning

Why is query design more important than warehouse size?

When should you increase warehouse size?

When should scaling out be used instead of scaling up?

What is the cost vs performance balance in Snowflake?

How does Snowflake parallelize queries?

How does result reuse improve performance?

What factors increase table scan size?

Why must filters be pushed as early as possible?

Why do unnecessary casts break performance?

How do you identify poorly written queries?

What is query history analysis?

Why should SELECT * be avoided?

🔹 2️⃣ CLUSTERING & PARTITIONING
A. Clusters vs Partitions

What is the difference between partitions and cluster keys?

What problem do cluster keys solve?

When should cluster keys be avoided?

How do cluster keys affect pruning?

What happens during reclustering?

B. Micro-Partitions (Deep Dive)

How large is a typical micro-partition?

What metadata does Snowflake store per partition?

How does Time Travel impact partitions?

How do frequent updates affect partitions?

Why do deletes cause fragmentation?

How does Snowflake automatically manage partitions?

C. Automatic Clustering

What is automatic clustering?

Does automatic clustering consume credits?

When does Snowflake trigger reclustering?

Can users control clustering frequency?

How do you monitor clustering quality and depth?

Which workloads benefit most from clustering?

🔹 3️⃣ QUERY OPTIMIZATION
A. Query Profiler

What is the Query Profiler?

Where is the profiler located in the UI?

What do green vs orange sections mean?

What is execution breakdown?

How do you identify scan-heavy queries?

How do you detect join performance problems?

What is remote spilling?

Why are skewed joins dangerous?

What does “queued” indicate?

B. Best Practices

Why filter on natural pruning columns?

Why avoid unnecessary DISTINCT?

How do window functions affect performance?

When should JSON flattening be applied?

What is a broadcast join?

What is a hash join?

How does join order influence cost?

When is materializing intermediate results helpful?

Why use temporary tables sometimes?

Why avoid massive UNION ALL pipelines?

What is the true cost of UDFs?

Why avoid too many VARIANT fields per row?

🔹 4️⃣ CACHING LAYERS
A. Result Cache

What is result caching?

How long does result cache persist?

Does result cache incur compute cost?

When is result cache invalidated?

Does the active role affect cache reuse?

Does warehouse change affect cache reuse?

B. Metadata Cache

What is metadata caching?

What metadata is stored?

Who controls metadata cache?

Does metadata cache survive restarts?

C. Data Cache

What is the warehouse data cache?

Where is it stored?

When is cached data reused?

Is cache shared across warehouses?

Why doesn’t cache persist long suspensions?

What happens if data changes?

🔹 5️⃣ SCENARIO-BASED THINKING

Query slow only the first time — why?

Query fast in morning but slow later — what changed?

Warehouse scaled up but performance didn’t improve — why?

Many tiny files cause slow ingestion — why?

Analysts constantly using SELECT * — impact?

Filters applied on non-selective columns — impact?

Table constantly updated — partition considerations?

Business requests indexes — what do you explain?

Dashboard refresh slow — what tuning checklist applies?

Storage ballooning after CDC — what should be investigated?

🔹 6️⃣ TRICK / MISCONCEPTIONS

Does Snowflake automatically optimize everything?

Do bigger warehouses always run faster?

Does clustering always improve speed?

Does caching eliminate performance tuning?

Does Snowflake lock tables during execution?

Do materialized views always improve performance?

Does automatic clustering solve pruning forever?

Does VARIANT always mean slow?

Does more partitioning always equal cheaper queries?

Does running the same query always hit cache?