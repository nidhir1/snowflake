# Snowflake staging — end-to-end playbook

**Goal:** reliably land raw source data (files/rows/messages) in a Snowflake staging area (either an external stage or an internal Snowflake stage / RAW schema) so downstream ELT can cleanse/transform it.

---

## 1) Quick glossary
- **Landing / Landing zone:** cloud object storage (S3 / GCS / Blob) where raw files first arrive.
- **Stage (Snowflake):** a Snowflake stage object that references internal or external storage; also shorthand for the Snowflake schema that holds raw tables.
- **RAW / STAGE / CURATED:** common layered schemas — RAW = mirror of source, STAGE = standardized, CURATED = business-ready.
- **VARIANT:** Snowflake column type for JSON/Avro/Parquet/other semi-structured payloads.
  - See Snowflake documentation for VARIANT and semi-structured best practices.

---

## 2) Discovery & prep checklist 
- Identify source(s), owner, criticality, sensitivity (PII/PHI/PCI), retention/compliance rules.
- Volume & latency requirements (MB/day vs TB/day; batch vs near-real-time).
- Formats produced (CSV / JSON / NDJSON / Parquet / Avro / ORC / XML / Excel / images / PDFs).
- Transactional consistency needs (full snapshot vs CDC with ordering).
- Network topology & cloud provider (same cloud as Snowflake or cross-cloud; this affects egress/cost).
- SLA for reprocessing/backfills (how often you must re-run ingestion).

Document these and pick an ingestion pattern based on latency, volume, and schema stability.

---

## 3) Ingestion patterns — decision matrix

| Source type | Typical pattern | Recommended Snowflake feature / tool | Why / notes |
|---|---:|---|---|
| Batch files on S3/GCS/Blob | Bulk load | `COPY INTO` from external stage; create `FILE FORMAT` | Simple, fast for large nightly loads. Use 100–250 MB compressed file sizes for throughput. |
| Real-time app logs / events | Streaming ingestion | Snowpipe (auto-ingest) or Snowpipe Streaming; use event notifications (SQS/SNS/GCS Pub/Sub). | Near-real-time, serverless, continuous. Auto ingest uses cloud messaging to trigger loads. |
| Kafka / event bus | Streaming sink | Snowflake Kafka Connector (or Kafka → S3 → Snowpipe) | Connects topics to Snowflake tables; supports batching and staging. |
| Databases (OLTP) | Bulk snapshot OR CDC | Bulk export → stage → COPY OR CDC pipeline (Debezium, DMS, 3rd-party CDC) → Kafka/S3 → Snowpipe | For transactional integrity use a log-based CDC tool; for less stringent use full extracts. |
| SaaS (Salesforce, Stripe, etc.) | Managed connector | Fivetran / Stitch / Airbyte / Matillion → Snowflake | Easiest for SaaS; connectors handle schema mapping & incremental loads. |
| Files on SFTP/FTP | Agent/worker | Ingest agent → upload to cloud storage → COPY / Snowpipe | Agents (custom or third-party) typically buffer + upload to S3 then trigger Snowpipe. |
| Large Parquet data lake | Query-in-place | External tables or stage + COPY for hot data | External tables let Snowflake query objects in S3/GCS directly (lakehouse pattern). |
| Ad-hoc user uploads (Excel, CSV) | Manual | PUT to internal stage + `COPY INTO` | For small one-offs; convert Excel to CSV or parse upstream. |

---

## 4) Landing options: external stage vs internal stage vs direct INSERT
- **External stage** (recommended for most production pipelines): external S3/GCS/Blob referenced by a Snowflake `STORAGE_INTEGRATION` (secure, IAM). Great for immutable raw files and reprocessing.
- **Internal stage:** smaller, quick staging (PUT from local). Not ideal for large-scale production.
- **Direct INSERT / JDBC batch:** only for tiny volumes or transactional inserts; avoid for bulk loads — `COPY` is far faster.

---

## 5) File formats and semi-structured handling
- Use named **FILE FORMAT** objects so `COPY` / `PIPE` commands stay tidy. Supported types: `CSV`, `JSON`, `AVRO`, `ORC`, `PARQUET`, `XML`, etc.
- **Semi-structured:** store the whole payload in a `VARIANT` column in RAW. Delay parsing until STAGE/CURATED — this handles schema drift and speeds ingestion.
- **XML:** Snowflake supports XML file loads; parsed into `VARIANT` / `OBJECT` / `ARRAY` structures (use XML functions).
- **Excel:** convert to CSV/Parquet first (Snowflake doesn’t natively ingest `.xlsx` without conversion).
- **Recommended file sizing:** aim for ~**100–250 MB compressed** files to maximize parallel load throughput; avoid thousands of tiny files or single gigantic files.

---

## 6) Concrete ingestion mechanisms & commands

### A — Bulk batch (recommended for nightly / large loads)
**Create a file format**
```sql
CREATE OR REPLACE FILE FORMAT my_csv_format
  TYPE = 'CSV'
  FIELD_DELIMITER = ','
  SKIP_HEADER = 1
  NULL_IF = ('','NULL');
```

**Create external stage (S3 example)**
```sql
CREATE OR REPLACE STAGE my_s3_stage
  URL='s3://my-bucket/path/'
  STORAGE_INTEGRATION = my_storage_integration
  FILE_FORMAT = my_csv_format;
```

**COPY INTO table**
```sql
COPY INTO raw_schema.orders
  FROM @my_s3_stage
  FILE_FORMAT = (FORMAT_NAME = 'my_csv_format')
  ON_ERROR = 'CONTINUE';
```

`COPY INTO` is Snowflake’s bulk loader — it's the workhorse for files.

---

### B — Continuous / near-real-time: Snowpipe (auto-ingest)
- Configure S3/GCS bucket notifications to Snowflake via SQS/SNS or Cloud Pub/Sub.
- Create a Snowpipe that runs a COPY statement; set `AUTO_INGEST = TRUE` so file arrival triggers a load.

**Example (simplified)**
```sql
CREATE OR REPLACE PIPE my_pipe AUTO_INGEST = TRUE AS
COPY INTO raw_schema.events (raw_payload, filename, loaded_at)
FROM (SELECT $1, METADATA$FILENAME, CURRENT_TIMESTAMP()
      FROM @my_s3_stage)
FILE_FORMAT = (TYPE = 'JSON' STRIP_OUTER_ARRAY = TRUE)
ON_ERROR = 'CONTINUE';
```

Snowpipe auto-ingest is the canonical pattern for streaming files from cloud storage.

---

### C — Kafka & event bus
- Use Snowflake Connector for Kafka to sink topics directly to Snowflake (it stages messages as files and loads them). Connector can also tie into Snowpipe Streaming.
- Alternative: Kafka → Kinesis/Firehose → S3 → Snowpipe.

**High-level flow**
```
Producer → Kafka topic → Snowflake Kafka Connector
   → (internal stage files) → Snowpipe / COPY → target table
```

---

### D — Database replication & CDC
- **Snapshot / bulk export:** dump data to files and `COPY`.
- **Log-based CDC:** Debezium / AWS DMS / vendor CDC → Kafka (or S3) → Snowpipe or Kafka connector → Snowflake. Log-based CDC preserves ordering and minimizes lag.

---

### E — External tables (query the lake without loading)
- Use **external tables** to query Parquet/ORC/CSV files in S3/GCS directly as if they were tables. Good for very large cold data in a lakehouse pattern or for infrequent queries. External tables read files into a `VARIANT`-style column.

---

## 7) Metadata, schema evolution & auditing (practical tips)
- Add ingestion metadata columns on load: `metadata$filename`, `metadata$file_row_number`, `metadata$file_last_modified`, `loaded_at`, `source_system` — you can capture these in `COPY` using `INCLUDE_METADATA` or `SELECT` transformations. This makes lineage and reprocessing much easier.
- For files with changing schemas (Parquet/JSON), use `INFER_SCHEMA`, `MATCH_BY_COLUMN_NAME`, and `ENABLE_SCHEMA_EVOLUTION` where appropriate — they let you evolve table schemas automatically when using `COPY`.
- Store raw files in cloud storage unchanged (immutable), and load copies into Snowflake. That raw archive is your rollback in reprocessing incidents.

---

## 8) Validation, error handling & idempotency
- Use `ON_ERROR` in `COPY` (`ABORT_STATEMENT`, `SKIP_FILE`, `CONTINUE`) depending on tolerance. Track `COPY_HISTORY` and `LOAD_HISTORY` for failures.
- For streaming, design idempotent loads (use dedup keys or dedup logic in STAGE) — Snowpipe can load files once, but producers may retry uploads.
- Use checksum/row counts in the landing zone to verify file completeness before running `COPY`.
- Implement automated alerts for failed loads and slow-downs (monitor `LOAD_HISTORY`, Snowflake usage metrics).

---

## 9) Security & governance (must-haves)
- Use **STORAGE INTEGRATION** (Snowflake) + cloud IAM policies for external stages (don’t embed keys in scripts).
- Encrypt data at rest in cloud storage and rely on Snowflake’s encryption in transit.
- Use **Object Tagging** and masking policies for PII/PHI; implement RBAC so only ETL roles can write to RAW tables.
- Keep an immutable copy in landing S3/GCS for audit and legal hold.

---

## 10) Performance & cost guidance
- **File sizing:** target ~**100–250MB compressed** files for efficient parallelism. Too many tiny files → overhead; very large files → poor parallelism.
- **Warehouses:** separate compute warehouses for ingestion (COPY / Snowpipe) vs BI/queries. Use auto-suspend (set low timeout) and right-size warehouses to control cost.
- **Clustering & micro-partitions:** for very large tables, consider clustering keys or materialized views to optimize query performance.
- **Avoid unnecessary copies:** use external tables where appropriate instead of copying massive cold datasets into Snowflake.

---

## 11) Common production patterns & examples
- **Pattern: SaaS → managed connector → Snowflake (daily + incremental)**  
  Use Fivetran / Stitch → push to Snowflake destination (they create RAW-like schemas and incremental tables). Fast to stand up, less maintenance.

- **Pattern: OLTP DB (low-latency analytics) — Debezium CDC**  
  Debezium captures binlog → Kafka topics → Snowflake Kafka connector (or a small process to create files in S3) → Snowpipe → RAW table → STREAM/TASK to apply to dimensional tables. This preserves ordering and allows near-RT analytics.

- **Pattern: IoT telemetry**  
  Devices → ingestion gateway → aggregate & write Parquet to S3 shards (100–250MB) → Snowpipe → VARIANT column in RAW → parsed in STAGE when needed.

---

## 12) Recommended operational checklist (what to automate)
- Automated schema discovery + alerts on schema evolution.
- Pipeline health dashboards (ingest rate, error counts, last success).
- Auto-archival + lifecycle rules for raw files.
- Idempotent loaders and dedup logic.
- Regular cost/usage reviews (warehouses / Snowpipe credits).

---

## 13) Example: JSON ingestion into a RAW VARIANT column (full snippet)
```sql
-- 1) file format for JSON
CREATE OR REPLACE FILE FORMAT ff_json
  TYPE = 'JSON'
  STRIP_OUTER_ARRAY = TRUE;

-- 2) external stage pointing to S3 (assumes STORAGE_INTEGRATION already configured)
CREATE OR REPLACE STAGE stg_raw_json
  URL='s3://my-bucket/raw/json/'
  STORAGE_INTEGRATION = my_integration
  FILE_FORMAT = ff_json;

-- 3) create a raw table (keep payload as VARIANT)
CREATE OR REPLACE TABLE raw.events_raw (
  payload VARIANT,
  source_file STRING,
  ingested_at TIMESTAMP_LTZ DEFAULT CURRENT_TIMESTAMP()
);

-- 4) COPY (bulk) example — loads JSON into the VARIANT payload column and captures file name metadata
COPY INTO raw.events_raw (payload, source_file)
FROM (
  SELECT $1, METADATA$FILENAME
  FROM @stg_raw_json
)
FILE_FORMAT = (FORMAT_NAME = 'ff_json')
ON_ERROR = 'CONTINUE';
```

---

## 14) Useful pages (official docs / connector guides)
- `COPY INTO` (bulk loading) — Snowflake docs  
- Snowpipe auto-ingest (S3 / messaging) — official guide  
- Semi-structured types and best practices (VARIANT, OBJECT, ARRAY)  
- External tables (querying Parquet in the lake)  
- Snowflake Connector for Kafka — install & overview  
- File sizing & load planning (official recommendations)

---

## 15) Common gotchas & war stories
- Lots of tiny files → huge load overhead and cost; aggregate upstream or use batching.
- Schema drift in JSON/Parquet → if you flattened everything at ingest you’ll be rewriting pipelines constantly. Keep raw `VARIANT` for agility.
- Missing authorization on S3/Blob: pipelines fail silently if `STORAGE_INTEGRATION` or cloud permissions are wrong — use least-privilege IAM roles and test with small files first.
- Snowpipe credit cost: Snowpipe has its pricing characteristics — monitor for noisy events and filter bucket notifications.

---


