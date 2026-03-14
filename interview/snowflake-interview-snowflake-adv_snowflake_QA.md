❄️ Advanced Snowflake Features 



🔹 1️⃣ SNOWFLAKE ON AZURE & EXTERNAL STORAGE
A. WORKING WITH AZURE STORAGE
✅ What external storage options does Snowflake support?

Snowflake can integrate with:

Azure Blob Storage

Azure Data Lake Storage (ADLS Gen2)

AWS S3

Google Cloud Storage

Snowflake never hosts these files — it simply reads them.

✅ What is Azure Blob Storage?

A general-purpose cloud object store used for:

raw files

logs

CSV / JSON / Parquet datasets

Blob is ideal when simple object storage is sufficient.

✅ What is Azure Data Lake Storage (ADLS)?

ADLS Gen2 is optimized for analytics workloads, providing:

hierarchical folders

fine-grained security

scalable low-latency file access

Preferred when building data lakes.

✅ How does Snowflake connect to Azure storage?

Connection happens through:

➡ external stages
➡ external tables
➡ COPY / UNLOAD commands

Snowflake does not pull files blindly — it uses secure credentials.

✅ What is external stage authentication?

The mechanism Snowflake uses to prove identity when reading/writing external files.

Two main options:

1️⃣ SAS tokens / access keys
2️⃣ Storage integrations (recommended)

✅ What is a SAS token?

A temporary signed key granting access to a specific container or path.

✔ easy
❌ insecure if leaked
❌ needs rotation manually

✅ What is storage integration?

A secure trust relationship between Snowflake and Azure.

Snowflake authenticates using its cloud identity, not passwords.

Benefits:

✔ no secrets stored in SQL
✔ centrally controlled
✔ auditable
✔ rotates automatically

✅ Why prefer storage integration over SAS keys?

Because SAS keys behave like passwords.

If exposed:

❌ attackers can read data
❌ access is hard to track
❌ manual revocation required

Storage integrations eliminate that risk.

✅ What permissions are required on storage?

Minimum required access such as:

read on blobs for ingestion

write when unloading

list for file discovery

Always follow least privilege.

✅ What security risks exist if keys are exposed?

Someone could:

⚠ copy your data externally
⚠ delete files
⚠ plant malicious files

This is why integrations are best practice.

B. CREATING & USING EXTERNAL STAGES
✅ What is an external stage?

A pointer in Snowflake to cloud storage.

It does not store files — it stores location + credential mapping.

✅ How is it different from an internal stage?

Internal stage lives inside Snowflake storage.

External stage lives in:

Blob

ADLS

S3

GCS

✅ Can external stages store credentials?

Yes — but only via storage integrations (best) or SAS (discouraged).

✅ How do we reference files?

Simply query using stage path.

✅ Can Snowflake query files without loading them?

Yes — using external tables.

✅ What is an external table?

A metadata wrapper that allows querying data in place without copying into Snowflake.

Best for:

data lakes

log analytics

staging exploration

✅ What happens when files change?

New files become visible automatically.
Deleted files disappear.

✅ How do partitions apply?

You define partitioning columns (often dates).
This improves pruning when scanning.

✅ How do we ingest into internal tables?

Use COPY INTO … FROM stage or external table.

🔹 2️⃣ SNOWPIPE & INCREMENTAL LOADS
A. SNOWPIPE BASICS
✅ What is Snowpipe?

Snowflake’s continuous ingestion service.

As soon as a file lands → Snowpipe loads it.

✅ Difference vs Tasks?

Snowpipe = reacts to new files
Tasks = scheduled logic

✅ What does “serverless ingestion” mean?

Snowflake runs compute automatically — you do not manage warehouses.

✅ Does Snowpipe need a warehouse?

No — ingestion compute is managed automatically.

✅ How does Snowpipe auto-ingest?

Uses cloud notifications:

Azure Event Grid

AWS SNS / SQS

GCS Pub/Sub

Files trigger ingestion instantly.

✅ Where do we monitor Snowpipe?

In UI or metadata views.

✅ What happens when Snowpipe fails?

File is retried and errors are logged for review.

B. INCREMENTAL LOAD CONCEPTS
✅ What is incremental loading?

Loading only new or changed data, instead of everything.

Benefits:

✔ faster
✔ cheaper
✔ safer

✅ Why not reload everything?

Full reloads:

waste compute

increase error risk

take longer as data grows

✅ How do Streams + Tasks + Snowpipe work together?

📥 Snowpipe loads raw files
🔍 Streams track changes
⚙ Tasks process CDC and merge into target

This is the modern ingestion pattern.

✅ How to avoid duplicates?

Use:

metadata columns

deduplication keys

MERGE logic

✅ Why idempotency matters?

Running the same pipeline twice should not duplicate results.

C. DATA UNLOADING
✅ What is unloading?

Exporting Snowflake data back to external storage.

Common use cases:

ML pipelines

archive

sharing with external systems

✅ How do we secure exported files?

Encrypt + restrict external IAM access.

D. EXTERNAL DATA STAGES — SECURITY

Covers:

lifecycle retention

expiring keys

rotation

shared access

Key rule:

Snowflake never “owns” the external data — governance remains with storage.

🔹 3️⃣ SNOWFLAKE INTEGRATION
A. AZURE DATA FACTORY (ADF)

ADF orchestrates flows across systems.

Snowflake benefits:

✔ scheduling
✔ retries
✔ multi-system pipelines

Use when pipelines involve more than Snowflake.

B. GOVERNANCE

Covers:

masking policies

row access policies

tags

lineage tracking

Governance ensures security, transparency, and compliance.

🔹 4️⃣ SECURE DATA SHARING

Key concept:

Data is shared without copying.

Provider controls access.
Consumer queries instantly.

Reader accounts allow access without Snowflake license.

Governance remains essential.

🔹 5️⃣ DATA MARKETPLACE

Snowflake Marketplace is like an “app store for data”.

Organizations can:

consume curated datasets

publish their own data products

No extracts, no file transfers.
🔹 6️⃣ SCENARIOS — THINK LIKE AN ARCHITECT (DETAILED)

These scenarios train you to choose the right Snowflake feature — not just memorize definitions.

⭐ Scenario 1 — Near real-time ingestion from ADLS

Problem

A team uploads files continuously to Azure Data Lake.
Business wants data visible in dashboards almost immediately.

Wrong thinking

“Let’s schedule tasks every 15 minutes.”

Scheduling creates delay. It also wastes compute.

Correct design

👉 Use Snowpipe + External Stage

Flow:

1️⃣ Files land in ADLS
2️⃣ Event Grid notifies Snowflake
3️⃣ Snowpipe ingests automatically
4️⃣ Data is available within seconds/minutes

Why this is right

✔ Serverless ingestion
✔ Very low latency
✔ No manual scheduling
✔ Automatic retry & logging

⭐ Scenario 2 — Multi-account secure data sharing

Problem

A large enterprise has multiple Snowflake accounts across departments.
They want access to the same dataset without copying terabytes.

Wrong thinking

“Export CSV and send via S3”
This breaks security and creates duplicates.

Correct design

👉 Use Secure Data Sharing

Provider publishes a database.
Consumer attaches to it instantly.

Key behavior

Data remains in provider account

No duplication

Provider controls revocation

Why this matters

✔ Single source of truth
✔ Governance friendly
✔ Near zero storage overhead

⭐ Scenario 3 — Protecting sensitive PII fields

Problem

Analysts need customer data — but must not see SSN, PAN, or phone numbers.

Wrong thinking

“Create duplicate masked tables.”

Leads to drift, mistakes, and cost.

Correct design

👉 Apply Masking Policies + Role-based access

Snowflake masks differently depending on the role.

Examples:

Admin sees full value

Analyst sees partially masked value

External users see nulls

Benefit

✔ One table
✔ Dynamic masking
✔ No extra copies

⭐ Scenario 4 — Partner read-only access to curated data

Problem

A partner company wants to run analytics, but must NOT download or modify data.

Correct design options

Requirement	Choose
Partner only queries	➜ Reader Account
Partner already has Snowflake	➜ Secure Data Sharing
Partner needs physical copy	➜ Replication (rare)

Why reader accounts are great

No Snowflake subscription for partner

You still control compute

Security remains centralized

⭐ Scenario 5 — Continuous CDC pipeline with minimal cost

Goal

Process only changed records — and avoid full reloads.

Correct design

👉 Snowpipe + Streams + Tasks

Pipeline:

1️⃣ Snowpipe ingests files
2️⃣ Stream tracks changes
3️⃣ Task merges into final tables

Why this works

✔ Incremental
✔ Exactly-once
✔ Scales automatically

⭐ Scenario 6 — Public dataset exposure

Goal

Share curated data publicly or monetize it.

Correct design

👉 Publish on Snowflake Data Marketplace

Snowflake manages:

subscriptions

usage metering

secure access

Benefit

✔ No file transfers
✔ Immediate consumer onboarding
✔ Revenue potential

🔹 7️⃣ MISCONCEPTIONS — WITH TEACHER EXPLANATIONS

These are questions interviewers love because many people answer incorrectly.

❌ “Snowpipe replaces ETL tools.”

Reality

Snowpipe only gets data into Snowflake.

You still need:

transformations

quality checks

orchestration logic

Snowpipe = ingestion only.

❌ “Snowpipe has zero latency.”

Reality

There is still processing time:

cloud notification

ingestion queue

transformation step

Usually seconds to minutes — not instant.

❌ “Secure sharing copies data to the consumer.”

Reality

No data is moved.

Consumer queries virtual reference pointers.
Provider storage remains authoritative.

❌ “Consumers can modify shared data.”

Reality

They cannot:

no DML

no structural changes

They may only read — unless they copy it.

❌ “External tables behave like normal tables.”

Reality

They read files in storage on demand.

This means:

slower than internal tables

dependent on storage availability

schema-on-read behavior

Use cautiously for analytics, great for lakes.

❌ “Marketplace data is always free.”

Reality

Some datasets are free, others are subscription-based.

Charges depend on:

provider

license terms

compute used to query

❌ “Data sharing is same as exporting data.”

Reality

Export creates duplicates.
Sharing avoids duplication entirely.

❌ “Tasks automatically detect new files like Snowpipe.”

Reality

Tasks are time-based or dependency-based, not event-driven.

If you want auto-ingest → use Snowpipe.

❌ “Governance features are optional.”

Reality

Without masking, tagging, RBAC, and auditing:

compliance fails

access cannot be explained

risk increases dramatically

Governance is a core Snowflake design principle.

❌ “Integration always reduces cost.”

Reality

More integrations sometimes:

add complexity

introduce latency

require governance overhead

Choose integrations deliberately.