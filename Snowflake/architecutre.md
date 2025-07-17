# Snowflake Architecture 

## Snowflake’s architecture is a hybrid of shared-disk and shared-nothing architectures, designed for cloud-based data warehousing. It consists of three main layers:

1. **Storage Layer**
Snowflake automatically manages structured and semi-structured data in an optimized columnar format.
Data is stored in cloud storage (AWS S3, Azure Blob Storage, or Google Cloud Storage).
It compresses, encrypts, and partitions the data.
Users do not directly manage storage but interact with it through SQL queries.
2. **Compute Layer (Virtual Warehouses)**
This layer consists of virtual warehouses (compute clusters) that process queries.
Virtual warehouses are independent of storage, allowing for automatic scaling and concurrent processing.
Each warehouse can be resized or paused independently, optimizing cost and performance.
Queries run in parallel without competing for resources.
3. **Cloud Services Layer**
This layer handles authentication, query optimization, metadata management, and security.
It eliminates manual database tuning by optimizing query execution automatically.
Metadata is stored separately from the compute layer, enabling instant access to data catalog and query history.

## Key Features of Snowflake Architecture
* Separation of Compute & Storage: Unlike traditional databases, storage and compute are decoupled, allowing independent scaling.
* Multi-Cluster Warehouses: Supports multiple compute clusters to handle high concurrency.
* Zero-Copy Cloning: Allows instant duplication of databases without extra storage cost.
*- Time Travel & Fail-Safe: Enables historical data recovery for a specific period.
* Support for Semi-Structured Data: JSON, Avro, Parquet, and ORC are natively supported.