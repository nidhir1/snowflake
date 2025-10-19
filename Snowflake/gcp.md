# Google Cloud Platform (GCP) Topics and Keywords

## 1. Compute
- **Compute Engine**  
  - VM Instances, Preemptible VMs, Custom Machine Types, Autoscaling, Managed Instance Groups  
  - *Use case:* Run scalable virtual machines for custom workloads.
- **Google Kubernetes Engine (GKE)**  
  - Managed Kubernetes, Clusters, Node Pools, Autopilot, Networking, Security  
  - *Use case:* Container orchestration and microservices deployment.
- **App Engine**  
  - Standard Environment, Flexible Environment, Autoscaling  
  - *Use case:* Deploy web apps without managing infrastructure.
- **Cloud Functions**  
  - Event-driven serverless compute  
  - *Use case:* Trigger functions on events like file uploads or Pub/Sub messages.
- **Cloud Run**  
  - Fully managed containerized apps, serverless  
  - *Use case:* Run stateless containers on demand.

## 2. Storage
- **Cloud Storage**  
  - Buckets, Storage Classes (Standard, Nearline, Coldline, Archive), Object Lifecycle Management  
  - *Use case:* Store and serve large amounts of unstructured data.
- **Persistent Disks & Local SSD**  
  - Block storage for VMs, SSD or HDD backed  
  - *Use case:* High-performance disks attached to VMs.
- **Filestore**  
  - Managed NFS file storage  
  - *Use case:* Shared file storage for applications like CMS or HPC workloads.
- **Databases**  
  - Cloud SQL (Managed relational: MySQL, PostgreSQL, SQL Server)  
  - Cloud Spanner (Global relational DB)  
  - Bigtable (NoSQL wide-column)  
  - Firestore (NoSQL document DB)  
  - Memorystore (Redis, Memcached)  
  - *Use case:* Choose database based on workload needs — transactional, scalable, caching.

## 3. Networking
- **Virtual Private Cloud (VPC)**  
  - Subnets, Routes, Firewalls, Private Access  
  - *Use case:* Isolate and secure cloud resources with custom networks.
- **Load Balancing**  
  - HTTP(S), TCP/UDP, Internal Load Balancers  
  - *Use case:* Distribute incoming traffic efficiently and ensure high availability.
- **Cloud CDN**  
  - Content Delivery Network to cache and deliver content globally  
  - *Use case:* Speed up delivery of websites and APIs worldwide.
- **Cloud DNS**  
  - Managed DNS service  
  - *Use case:* Reliable, scalable domain name resolution.
- **Cloud Armor**  
  - Web Application Firewall (WAF), DDoS protection  
  - *Use case:* Protect applications from attacks.
- **Cloud VPN & Interconnect**  
  - Secure connections between on-premises and cloud  
  - *Use case:* Hybrid cloud connectivity.

## 4. Big Data & Analytics
- **BigQuery**  
  - Serverless data warehouse, SQL queries, ML integration  
  - *Use case:* Analyze petabytes of data with fast SQL queries.
- **Dataflow**  
  - Stream & batch data processing with Apache Beam  
  - *Use case:* Real-time data pipelines and ETL jobs.
- **Dataproc**  
  - Managed Hadoop/Spark clusters  
  - *Use case:* Run existing big data workloads with minimal management.
- **Pub/Sub**  
  - Messaging service for event ingestion and delivery  
  - *Use case:* Decouple event producers and consumers.
- **Data Fusion**  
  - Managed data integration (ETL) service  
  - *Use case:* Build scalable data pipelines with low code.
- **Cloud Composer**  
  - Managed Apache Airflow workflows  
  - *Use case:* Orchestrate complex data workflows.

## 5. Machine Learning & AI
- **AI Platform / Vertex AI**  
  - Model training, deployment, pipelines  
  - *Use case:* Build, deploy, and manage ML models at scale.
- **AutoML**  
  - Custom model building with minimal coding (Vision, Language, Tables, Translation)  
  - *Use case:* Quickly create tailored ML models without deep ML expertise.
- **Prebuilt APIs**  
  - Vision API, Natural Language API, Translation API, Speech-to-Text, Text-to-Speech, Video Intelligence  
  - *Use case:* Add AI capabilities like image recognition, language understanding.
- **Dialogflow**  
  - Conversational AI platform  
  - *Use case:* Build chatbots and virtual assistants.

## 6. Security
- **Cloud IAM**  
  - Fine-grained access control with roles and policies  
  - *Use case:* Secure resource access management.
- **Cloud Identity**  
  - User identity management  
  - *Use case:* Manage users and groups across GCP.
- **Cloud KMS**  
  - Key management and encryption  
  - *Use case:* Encrypt sensitive data with managed keys.
- **Security Command Center**  
  - Centralized security and risk management  
  - *Use case:* Monitor and remediate security issues.
- **DLP API**  
  - Data loss prevention, sensitive data discovery and masking  
  - *Use case:* Protect sensitive info like PII and PCI data.
- **Shielded VMs & Binary Authorization**  
  - Secure VM boot and image verification  
  - *Use case:* Ensure VM integrity and trusted deployments.

## 7. Management & Monitoring
- **Cloud Console & Cloud Shell**  
  - Web UI and command line for resource management
- **Deployment Manager**  
  - Infrastructure as Code for resource provisioning
- **Cloud Monitoring & Logging**  
  - Metrics collection, dashboards, alerts, log aggregation  
  - *Use case:* Monitor app and infra health and troubleshoot issues.
- **Cloud Trace & Profiler**  
  - Distributed tracing and performance profiling  
  - *Use case:* Optimize app latency and resource use.

## 8. Developer Tools
- **Cloud Build**  
  - Continuous Integration/Continuous Delivery (CI/CD)  
  - *Use case:* Automate builds, tests, and deployments.
- **Cloud Source Repositories**  
  - Private Git repositories
- **Artifact Registry**  
  - Container and artifact storage
- **Cloud Scheduler**  
  - Cron jobs and scheduled tasks
- **Cloud Tasks**  
  - Asynchronous task execution
- **Cloud Code**  
  - IDE plugins for Kubernetes & Cloud development

## 9. Serverless
- **Cloud Functions**
- **Cloud Run**
- **App Engine**

*(Serverless compute options to build scalable apps without managing servers)*

## 10. Migration & Transfer
- **Transfer Service for Cloud**
- **Migrate for Compute Engine**
- **Database Migration Service**
- **Transfer Appliance**

*Use case:* Move data, VMs, and databases to GCP with minimal downtime.

## 11. API Management & Integration
- **API Gateway**
- **Apigee**
- **Cloud Endpoints**
- **Eventarc**
- **Pub/Sub**

*Use case:* Manage, secure, and monitor APIs and event-driven integrations.

## 12. Hybrid & Multi-cloud
- **Anthos**
- **Migrate for Anthos**
- **Traffic Director**
- **Service Mesh (Istio)**

*Use case:* Deploy and manage workloads across on-premises and multiple clouds seamlessly.

## 13. Cost Management
- **Cloud Billing**
- **Budgets & Alerts**
- **Cost Table & Recommender**
- **Pricing Calculator**

*Use case:* Track, optimize, and forecast cloud spending.

## 14. Compliance & Governance
- **Resource Hierarchy** (Organization, Folders, Projects)
- **Policy Intelligence**
- **Cloud Asset Inventory**
- **Compliance Certifications** (HIPAA, GDPR, FedRAMP)

*Use case:* Enforce governance and meet regulatory requirements.
