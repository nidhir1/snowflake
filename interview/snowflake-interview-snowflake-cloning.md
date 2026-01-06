# Snowflake — Cloning & Zero-Copy Cloning  
## Question Bank (Questions Only)

---

🔹 1️⃣ CLONING OPERATIONS — BASICS

What is cloning in Snowflake?

How is cloning different from copying data?

What does “zero-copy cloning” mean?

Why is cloning extremely fast?

Which objects can be cloned in Snowflake?

Can entire databases be cloned?

Can schemas be cloned?

Can tables be cloned?

What privileges are needed to clone?

Can we rename a clone?

How do you identify if an object is a clone?

Does cloning require a warehouse?

Can you query a clone immediately after creation?

---

🔹 2️⃣ ZERO COPY — HOW IT ACTUALLY WORKS

What does “zero copy” really refer to?

Does Snowflake duplicate physical storage when cloning?

What happens at the metadata layer during cloning?

Why does cloning initially consume almost no extra storage?

When does a clone start consuming its own storage?

Does modifying cloned data affect the source?

Does modifying source affect the clone?

How does Snowflake track changes between clones?

How does Time Travel relate to cloning?

Are historical records shared between clone and source?

---

🔹 3️⃣ SCHEMA & DATABASE LEVEL CLONING

What is schema-level cloning?

Which objects inside a schema are cloned automatically?

Do tasks, streams, views, and stages clone too?

What happens to grants during schema cloning?

What is database-level cloning?

When would you prefer DB cloning instead of schema cloning?

Are cross-account or cross-region clones possible?

Can you clone objects across warehouses?

Are temporary objects cloned?

Are transient objects cloned?

---

🔹 4️⃣ REAL-TIME USES OF CLONING

Creating dev environment copies.

UAT refresh without downtime.

Testing destructive changes safely.

Point-in-time backup using clone.

Sharing data between teams without duplication.

Rapid sandbox creation for analysts.

Reproduce bugs in production environment.

Safe rollback path during migrations.

Reducing storage cost while testing.

Creating masked copies for secure testing.

---

🔹 5️⃣ STORAGE & METADATA LAYER BEHAVIOR

Where is Snowflake data physically stored?

What role does metadata play in cloning?

How does metadata reference original micro-partitions?

What happens when both clone and source delete data?

What happens if the source is dropped?

Does clone keep independent Time Travel history?

Does Fail-safe apply to clones?

How do retention settings affect clone behavior?

Does cloning change Fail-safe windows?

Can you clone historical state using Time Travel?

What are the performance implications of cloning?

---

🔹 6️⃣ SCENARIO-BASED QUESTIONS — REAL PROJECT THINKING

Developer needs production copy — safest solution?

ETL accidentally deletes records — clone or Time Travel?

Business wants testing copy without doubling storage — approach?

Migration rollback needed — how do you prepare?

Need region-to-region sandbox — cloning or replication?

Someone dropped the source table — can clone still survive?

Analysts want to change structure on copy — allowed?

Team wants to refresh every day — clone or load again?

Sandbox queries impacting production — which approach fixes that?

Data scientist needs experiments on large dataset — best option?

---

🔹 7️⃣ TRICK / MISCONCEPTION QUESTIONS

Does cloning instantly duplicate entire dataset?

Does dropping source delete the clone?

Does dropping clone delete the source?

Does clone always remain dependent on source forever?

Are clones free forever?

Does cloning bypass security policies?

Is cloning the same as replication?

Is cloning a backup strategy by itself?

Can a clone be converted back into the source?

Does cloning work outside retention windows?

Are compute costs eliminated when using clones?

Is cloning slower for bigger tables?

Does Time Travel stop working with clones?

Are views and materialized views cloned the same way?
