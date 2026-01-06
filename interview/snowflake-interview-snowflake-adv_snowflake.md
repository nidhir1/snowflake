❄️ ADVANCED SNOWFLAKE FEATURES

(for: snowflake-advanced-features.md)

Covering:

1️⃣ External storage & Azure
2️⃣ Snowpipe & incremental loads
3️⃣ Integrations
4️⃣ Secure data sharing
5️⃣ Data marketplace

🔹 1️⃣ SNOWFLAKE ON AZURE & EXTERNAL STORAGE
A. WORKING WITH AZURE STORAGE

What external storage options does Snowflake support?

What is Azure Blob Storage?

What is Azure Data Lake Storage (ADLS)?

How does Snowflake connect to Azure storage?

What is external stage authentication?

What is SAS token?

What is storage integration?

Why prefer storage integration over SAS keys?

What permissions are required on storage?

What security risks exist if keys are exposed?

B. CREATING & USING EXTERNAL STAGES

What is an external stage?

How is it different from an internal stage?

Can external stages store credentials?

How do we reference files in external stage?

Can Snowflake query files without loading them?

What is external table?

When should external tables be used?

What happens when files change in storage?

How do partitions apply to external tables?

How do we ingest from external stage into table?

🔹 2️⃣ SNOWPIPE & INCREMENTAL LOADS
A. SNOWPIPE BASICS

What is Snowpipe?

Difference between Snowpipe and Tasks.

What does “serverless ingestion” mean?

Does Snowpipe need a warehouse?

How does Snowpipe auto-ingest new files?

Which cloud notifications does Snowpipe use?

How often does Snowpipe poll?

Where can we monitor Snowpipe?

What happens when Snowpipe fails?

B. INCREMENTAL LOAD CONCEPTS

What is incremental loading?

Why not reload full dataset every time?

How do Streams + Tasks + Snowpipe work together?

How to avoid duplicate loads?

How does metadata help in incremental logic?

Why is idempotency important?

C. DATA UNLOADING

What is data unloading?

Why would organizations unload data out of Snowflake?

What command is used for unloading?

What formats support unloading?

How to secure exported files?

Can unloading integrate with other clouds?

D. EXTERNAL DATA STAGES

What is an external data stage used for?

What are security considerations for external stages?

How do lifecycle policies affect file availability?

What happens if external stage key expires?

How to rotate keys securely?

Can multiple warehouses load from one external stage?

🔹 3️⃣ SNOWFLAKE INTEGRATION
A. AZURE DATA FACTORY (ADF)

What is Azure Data Factory?

Why use ADF with Snowflake?

How does ADF connect to Snowflake?

What authentication methods can be used?

What is linked service?

What scenarios are suited for ADF + Snowflake?

When NOT to use ADF?

B. GOVERNANCE & MANAGEMENT

What is data governance?

What governance challenges exist in cloud DW?

What governance features does Snowflake offer?

What is masking policy?

What is row access policy?

How does tagging support governance?

Why metadata lineage matters?

How does governance integrate with BI tools?

🔹 4️⃣ SECURE DATA SHARING
A. DATA SHARING CONCEPTS

What is secure data sharing?

How does Snowflake share data without copying?

What are provider and consumer accounts?

What is a reader account?

Difference between sharing and replication.

Who controls access to shared data?

Does consumer pay storage?

Can shared data be modified?

B. MANAGING SHARED DATA

How do you revoke shared data?

How do you audit shared data usage?

What governance risks exist?

Can shared data be cloned?

Can shared data be joined with local data?

Can different consumers receive different slices of data?

What is data clean room concept?

🔹 5️⃣ SNOWFLAKE DATA MARKETPLACE
A. USING MARKETPLACE

What is Snowflake Data Marketplace?

What types of data are available?

How does marketplace reduce integration cost?

Do consumers pay for marketplace storage?

Are marketplace datasets real-time?

What legal/compliance issues exist?

B. PUBLISHING TO MARKETPLACE

Who can publish datasets?

What requirements are needed?

How to manage subscriptions?

How pricing works for providers?

How to ensure privacy when sharing data?

What governance tools support data marketplace sharing?

What happens when provider updates dataset?

🔹 6️⃣ SCENARIO-BASED QUESTIONS

Need near real-time ingestion from ADLS — which feature?

Large enterprise wants multi-account data sharing — approach?

Project requires audit-safe data masking — which governance tools?

Partner company wants read-only analytics — sharing or replication?

Data team wants CDC with minimal cost — pipeline design?

Need to expose curated dataset publicly — marketplace or sharing?

Need automated ingestion whenever file lands — Snowpipe or Tasks?

Need to unload data to S3 for ML — how?

External account must access your data securely — approach?

Legal requests revoke partner access — what steps?

🔹 7️⃣ TRICK / MISCONCEPTION QUESTIONS

Does Snowpipe replace ETL tools completely?

Does Snowpipe guarantee zero latency?

Does secure sharing duplicate storage?

Can shared consumers modify provider data?

Is external table same as normal table?

Does marketplace data always free?

Is data sharing same as exporting CSV?

Do tasks automatically process new files like Snowpipe?

Are governance policies optional?

Does integration always reduce cost?

🎯 TEACHING SUMMARY — KEY STUDENT INSIGHTS

✔ External stages enable hybrid storage
✔ Snowpipe + streams enable continuous ingestion
✔ Integrations make Snowflake part of enterprise architecture
✔ Secure data sharing avoids duplication
✔ Marketplace enables commercial and public data exchange
✔ Governance remains essential everywhere