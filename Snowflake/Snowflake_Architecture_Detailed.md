
# Snowflake Architecture - Detailed Overview

## 1. Introduction
Snowflake is a cloud-native data warehouse built for scalability, elasticity, and high performance. 
It uses a **multi-cluster shared data architecture** and separates **storage, compute, and services** layers.

---

## 2. Core Architectural Layers

### **A. Cloud Services Layer**
- Coordinates all Snowflake activities.
- Manages authentication, metadata, optimization, and query parsing.
- Key Responsibilities:
  - **Authentication & Access Control** (SSO, MFA, OAuth, Key-Pair Authentication)
  - **Metadata Management** (tables, columns, statistics, micro-partitioning)
  - **Query Parsing & Optimization** (cost-based optimizer)
  - **Transaction Management** (ACID compliance)
  - **Infrastructure Management** (auto-suspend, auto-resume)

---

### **B. Compute Layer (Virtual Warehouses)**
- Independent compute clusters used to execute queries.
- Can scale **horizontally** (multi-cluster) or **vertically** (larger warehouses).
- Key Features:
  - **Elastic Scaling** (increase/decrease nodes instantly)
  - **Concurrency Scaling** (parallel query execution)
  - **Isolation** (separate warehouses for ETL, BI, ML workloads)

---

### **C. Storage Layer**
- Stores all structured and semi-structured data.
- Data is stored in **micro-partitions** (~50MB–500MB each).
- Columnar format for efficient compression and retrieval.
- Key Features:
  - **Automatic Compression**
  - **Time Travel** (1–90 days data recovery)
  - **Zero-Copy Cloning** (instant, cost-free duplication)
  - **Support for Multiple Formats** (CSV, JSON, Avro, ORC, Parquet)

---

## 3. Deployment on Public Clouds
Snowflake is available on:
- **AWS** → Uses S3 for storage, EC2 for compute
- **Azure** → Uses Blob Storage, Virtual Machines for compute
- **Google Cloud (GCP)** → Uses Google Cloud Storage, Compute Engine

---

## 4. Data Flow in Snowflake

### Step 1: Data Ingestion
- **Batch Loads**: COPY INTO from cloud storage (S3, Azure Blob, GCS)
- **Streaming Loads**: Snowpipe for near-real-time ingestion
- **External Tables**: Query without importing into Snowflake

### Step 2: Storage
- Data is automatically partitioned into micro-partitions.
- Metadata stored in Cloud Services Layer.

### Step 3: Compute
- Virtual Warehouses read from storage, process, and write results back.

### Step 4: Output
- Results served to BI tools, APIs, or stored for later queries.

---

## 5. Advantages of Snowflake Architecture
- **Separation of Storage & Compute** → Pay only for what you use
- **Multi-Cloud Availability**
- **High Concurrency Support**
- **Native Semi-Structured Data Handling**
- **Zero Maintenance**

---

## 6. High-Level Architecture Diagram

```plaintext
                +------------------------+
                |   Cloud Services Layer |
                | (Auth, Metadata, SQL   |
                |  Parsing, Optimization)|
                +-----------+------------+
                            |
         +--------------------------------------------+
         |                 Compute Layer              |
         | (Virtual Warehouses - Multiple Clusters)   |
         +--------------------+-----------------------+
                              |
                +-------------------------------+
                |         Storage Layer          |
                | (Cloud Storage: S3, Blob, GCS) |
                +-------------------------------+
```

---

## 7. Summary
Snowflake's architecture separates services, compute, and storage, allowing unmatched scalability and flexibility. 
Its design supports diverse workloads — from ETL pipelines to BI dashboards and ML model training — while keeping operational overhead minimal.
