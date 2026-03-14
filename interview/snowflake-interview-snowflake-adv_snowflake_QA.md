# ❄️ Advanced Snowflake Features --- Detailed Q&A

------------------------------------------------------------------------

## 1️⃣ SNOWFLAKE ON AZURE & EXTERNAL STORAGE

### Ques: What external storage options does Snowflake support?

Ans: Snowflake supports external cloud storage services including Amazon
S3, Azure Blob Storage, Azure Data Lake Storage (ADLS), and Google Cloud
Storage. These allow Snowflake to load, query, and unload data stored
outside the Snowflake platform.

### Ques: What is Azure Blob Storage?

Ans: Azure Blob Storage is Microsoft's object storage service designed
for storing massive amounts of unstructured data such as logs, backups,
media files, and analytics datasets.

### Ques: What is Azure Data Lake Storage (ADLS)?

Ans: Azure Data Lake Storage is built on top of Azure Blob Storage and
optimized for analytics workloads. It supports hierarchical file
structures and is commonly used for big data processing.

### Ques: How does Snowflake connect to Azure storage?

Ans: Snowflake connects through external stages configured with
authentication credentials such as SAS tokens or through secure storage
integrations.

### Ques: What is external stage authentication?

Ans: External stage authentication is the method Snowflake uses to
securely access external storage using credentials or integrations.

### Ques: What is SAS token?

Ans: A Shared Access Signature (SAS) token is a temporary credential
that grants limited access permissions to Azure storage resources.

### Ques: What is storage integration?

Ans: Storage integration is a Snowflake security object that enables
secure access to cloud storage using identity-based authentication
without exposing credentials.

### Ques: Why prefer storage integration over SAS keys?

Ans: Storage integration improves security by avoiding hard-coded
credentials and allowing centralized permission management.

### Ques: What permissions are required on storage?

Ans: Typically READ and LIST permissions are required for loading data,
while WRITE permissions are required when unloading data.

### Ques: What security risks exist if keys are exposed?

Ans: Exposed credentials could allow unauthorized users to access, copy,
modify, or delete data stored in cloud storage.

------------------------------------------------------------------------

## 2️⃣ SNOWPIPE & INCREMENTAL LOADS

### Ques: What is Snowpipe?

Ans: Snowpipe is Snowflake's automated ingestion service that
continuously loads data from cloud storage into Snowflake tables as
files arrive.

### Ques: Difference between Snowpipe and Tasks.

Ans: Snowpipe automatically ingests files, while Tasks schedule and run
SQL operations such as transformations or pipeline orchestration.

### Ques: What does "serverless ingestion" mean?

Ans: Serverless ingestion means Snowflake manages the compute resources
needed for ingestion, eliminating the need for user-managed warehouses.

### Ques: Does Snowpipe need a warehouse?

Ans: No, Snowpipe uses Snowflake-managed compute resources.

### Ques: How does Snowpipe auto-ingest new files?

Ans: It integrates with cloud event notification systems like Azure
Event Grid or AWS SQS.

### Ques: Which cloud notifications does Snowpipe use?

Ans: Snowpipe uses services such as Azure Event Grid, AWS SNS/SQS, or
Google Pub/Sub depending on the cloud platform.

### Ques: How often does Snowpipe poll?

Ans: When event notifications are used, ingestion typically occurs
within seconds to minutes.

### Ques: Where can we monitor Snowpipe?

Ans: Monitoring can be done via the Snowflake UI, INFORMATION_SCHEMA
views, ACCOUNT_USAGE views, or system functions like
SYSTEM\$PIPE_STATUS.

### Ques: What happens when Snowpipe fails?

Ans: Errors are logged and files can be retried after resolving the
issue.

------------------------------------------------------------------------

## 3️⃣ SNOWFLAKE INTEGRATION

### Ques: What is Azure Data Factory?

Ans: Azure Data Factory (ADF) is a cloud ETL orchestration tool used to
build data pipelines that move and transform data across systems.

### Ques: Why use ADF with Snowflake?

Ans: ADF helps orchestrate complex pipelines that integrate Snowflake
with other systems and automate workflows.

### Ques: How does ADF connect to Snowflake?

Ans: ADF connects through Snowflake connectors configured as linked
services.

### Ques: What authentication methods can be used?

Ans: Authentication methods include username/password, key pair
authentication, OAuth, and managed identity.

### Ques: What is linked service?

Ans: A linked service is a connection configuration in ADF that defines
how to connect to an external system such as Snowflake.

### Ques: What scenarios are suited for ADF + Snowflake?

Ans: ADF is commonly used for pipeline orchestration, cross-platform
data integration, and scheduled ETL workflows.

### Ques: When NOT to use ADF?

Ans: When Snowflake native capabilities like Snowpipe, Streams, and
Tasks can already handle ingestion and transformation.

------------------------------------------------------------------------

## 4️⃣ SECURE DATA SHARING

### Ques: What is secure data sharing?

Ans: Secure data sharing allows Snowflake accounts to share data with
other accounts without copying the underlying dataset.

### Ques: How does Snowflake share data without copying?

Ans: Snowflake shares metadata references to the stored data instead of
duplicating the data itself.

### Ques: What are provider and consumer accounts?

Ans: The provider owns the data and shares it, while the consumer
account receives read-only access.

### Ques: What is a reader account?

Ans: A reader account allows organizations to share data with external
users who do not have their own Snowflake account.

### Ques: Difference between sharing and replication.

Ans: Sharing provides live access without copying data, while
replication creates a physical copy of the data.

### Ques: Who controls access to shared data?

Ans: The provider account controls which objects are shared and who can
access them.

### Ques: Does consumer pay storage?

Ans: No, the provider pays storage costs while the consumer pays compute
costs for queries.

### Ques: Can shared data be modified?

Ans: No, shared data is read-only for consumers.

------------------------------------------------------------------------

## 5️⃣ SNOWFLAKE DATA MARKETPLACE

### Ques: What is Snowflake Data Marketplace?

Ans: The Snowflake Data Marketplace is a platform where organizations
can publish and subscribe to live datasets directly inside Snowflake.

### Ques: What types of data are available?

Ans: Financial data, weather data, demographic datasets, market
insights, and many other curated datasets.

### Ques: How does marketplace reduce integration cost?

Ans: It eliminates the need to build pipelines or manage dataset
synchronization.

### Ques: Are marketplace datasets real-time?

Ans: Many datasets are continuously updated depending on the provider.

### Ques: What legal/compliance issues exist?

Ans: Organizations must ensure compliance with licensing agreements,
privacy regulations, and governance policies.

------------------------------------------------------------------------

## 6️⃣ SCENARIO-BASED QUESTIONS

### Ques: Need near real-time ingestion from ADLS --- which feature?

Ans: Snowpipe combined with Azure Event Grid notifications.

### Ques: Large enterprise wants multi-account data sharing --- approach?

Ans: Secure Data Sharing with proper RBAC governance.

### Ques: Project requires audit-safe data masking --- which governance tools?

Ans: Dynamic Data Masking and Row Access Policies.

### Ques: Partner company wants read-only analytics --- sharing or replication?

Ans: Secure Data Sharing.

### Ques: Need automated ingestion whenever file lands --- Snowpipe or Tasks?

Ans: Snowpipe.

------------------------------------------------------------------------

## 7️⃣ TRICK / MISCONCEPTION QUESTIONS

### Ques: Does Snowpipe replace ETL tools completely?

Ans: No, Snowpipe handles ingestion but full ETL orchestration may still
require external tools.

### Ques: Does Snowpipe guarantee zero latency?

Ans: No, ingestion typically occurs within seconds or minutes.

### Ques: Does secure sharing duplicate storage?

Ans: No, data is shared using metadata references.

### Ques: Is external table same as normal table?

Ans: No, external tables reference files stored outside Snowflake.

### Ques: Is data sharing same as exporting CSV?

Ans: No, secure sharing provides live access without copying data.
