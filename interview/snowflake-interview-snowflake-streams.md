🔹 1️⃣ WORKING WITH SNOWFLAKE STREAMS
A. BASICS — WHAT IS A STREAM?

What is a Snowflake Stream?

Why do we need Streams?

What problem do Streams solve in ETL pipelines?

Are Streams physical data copies?

How do Streams track table changes?

What is CDC (Change Data Capture)?

What operations can Streams capture?

Do Streams modify underlying tables?

How often can a Stream be queried?

What privileges are needed to create streams?

B. STREAM OBJECT & DML AUDITING

What metadata does a Stream store?

What are the metadata columns (METADATA$)?

What does METADATA$ACTION represent?

What does METADATA$ISUPDATE show?

What is METADATA$ROW_ID?

How do Streams help with DML auditing?

Can Streams detect TRUNCATE operations?

Do Streams track historical changes forever?

What happens when data is consumed from a Stream?

Why is consumption marker important?

C. STREAM TYPES
⭐ Standard Stream

What is a Standard Stream?

What operations does it capture?

When should we use Standard Streams?

⭐ Append-Only Stream

What is an Append-Only Stream?

Does it capture deletes or updates?

When is Append-Only useful?

⭐ Insert-Only Stream

What is Insert-Only Stream?

How is it different from Append-Only?

When should Insert-Only be used?

Why are lighter-weight Streams sometimes better?

D. DATA FLOW WITH STREAMS

What is source table?

What is target table?

How do Streams interact with tasks?

What is a pipeline using stream + task?

What happens when ETL reads from a stream?

What is offset checkpoint?

Can one stream feed multiple consumers?

Can one table have multiple streams?

What happens if consumer crashes mid-run?

How do you reset or recreate a stream?

🔹 2️⃣ TIME TRAVEL WITH STREAMS
A. USING STREAM TABLES

What role does Time Travel play with Streams?

How do Streams internally rely on Time Travel?

What happens if retention window expires?

Can Streams function without Time Travel?

Can you query stream history directly?

What is stale stream error?

What happens if source table is dropped?

Can Streams be cloned with tables?

Do Streams participate in Fail-safe?

Best practices for managing streams lifecycle.

B. AUDITING INSERT / UPDATE / DELETE

How can Streams audit inserts?

How can Streams audit updates?

How can Streams audit deletes?

What column indicates before vs after image?

How to reconstruct change history over time?

When should streams be used for auditing vs ETL?

Can Streams detect who made the change?

How to combine Streams with QUERY_HISTORY?

How to troubleshoot missing rows in Stream?

What happens when stream is consumed twice?

🔹 3️⃣ SCENARIO-BASED QUESTIONS

Need incremental load from source to target — which feature helps?

Need real-time CDC — task + stream or full refresh?

Accidentally read entire stream twice — what now?

Source table truncated — can stream help recover?

Business asks “what changed since yesterday?” — how do you answer?

Data warehouse requires audit logs — which Snowflake feature helps?

ETL pipeline missed two days — how to catch up?

Need insert-only audit for logs — which stream type?

Want light-weight change tracking — append-only or standard?

Retention expired and stream failed — what options exist?

🔹 4️⃣ TRICK / MISCONCEPTION QUESTIONS

Do Streams store real copies of changed rows?

Do Streams keep data forever?

If a row is updated twice, do we see two events?

Can Streams be queried without warehouse?

Does querying a Stream remove data immediately?

Can Streams rewind backwards?

Does Time Travel replace Streams?

Are Streams good for full reload jobs?

Do Streams audit user identity automatically?

Does deleting a stream delete historical changes permanently?

🎯 TEACHING SUMMARY — WHAT STUDENTS MUST UNDERSTAND

✔ Streams = change tracking, not storage
✔ They rely on metadata + Time Travel
✔ Different stream types fit different workloads
✔ Essential for CDC pipelines and auditing
✔ Need careful lifecycle + retention management
✔ Not designed for permanent history storage