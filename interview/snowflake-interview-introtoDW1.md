# INTRODUCTION TO CLOUD PLATFORMS — FULL QUESTION BANK (DETAILED ANSWERS)

---

## 🔹 1. BASIC — FOUNDATIONAL QUESTIONS

### What is cloud computing?
Cloud computing is the delivery of IT resources (servers, storage, networks, databases, AI, analytics, etc.) over the internet, on demand.  
Instead of buying hardware, you rent what you need from a provider — and scale up or down whenever required.

---

### Why do companies move to the cloud?
Cloud helps organizations:
- avoid big upfront hardware costs
- deploy faster
- scale instantly during demand spikes
- reduce maintenance effort
- innovate quicker using managed services (AI, analytics, databases)

It shifts focus from managing infrastructure to building business solutions.

---

### What are on-premises systems?
On-prem systems are installed in the organization’s own building or data center.  
The company is responsible for buying hardware, installing software, security, power, cooling, backups, and upgrades.

---

### Difference between on-premises and cloud
| On-Premises | Cloud |
|---|---|
| You buy hardware | You rent resources |
| High upfront cost | Pay only for usage |
| Slow to scale | Scale instantly |
| You maintain everything | Provider manages infrastructure |
| Limited reach | Global availability |

---

### What does “pay-as-you-go” mean?
You pay only for what you actually use — like electricity.  
If you stop using a service, the cost stops as well.

---

### What is scalability?
Scalability is the ability to increase capacity when workload grows.  
Example: adding more servers when millions of users join.

---

### What is elasticity?
Elasticity automatically scales resources **up AND down** based on demand.  
Busy hour → scale up  
Quiet time → scale down to save cost

---

### What is availability?
Availability measures how often a system is accessible and operational (e.g., 99.99%).  
High availability designs reduce downtime.

---

### What is reliability?
Reliability means the system consistently works as expected over time — without frequent failures, crashes, or lost data.

---

### What is fault tolerance?
Fault tolerance keeps systems running even when components fail.  
If one server fails, another one automatically takes over.

---

### What is redundancy?
Redundancy means having backup copies of systems/components — multiple disks, servers, or regions — so a single failure doesn’t impact users.

---

### What are cloud service models?
1️⃣ **IaaS** – Infrastructure as a Service  
   You manage OS + apps; provider manages servers/network.

2️⃣ **PaaS** – Platform as a Service  
   Provider manages OS + runtime; you deploy code.

3️⃣ **SaaS** – Software as a Service  
   Complete applications delivered over the browser.

---

### What are cloud deployment models?
- **Public cloud** — shared infrastructure, provider-managed  
- **Private cloud** — dedicated infrastructure for one organization  
- **Hybrid cloud** — mix of cloud + on-prem  
- **Multi-cloud** — more than one cloud provider

---

### What is virtualization?
Virtualization allows multiple virtual computers to run on a single physical server using a hypervisor.  
It improves hardware utilization and isolation.

---

### What is a virtual machine?
A VM behaves like a full computer with its own OS and apps — but shares physical hardware with others.

---

### What is a container?
A container packages an application plus its dependencies.  
It starts fast, uses fewer resources than VMs, and runs consistently everywhere (e.g., Docker, Kubernetes).

---

### Examples of major cloud providers
AWS, Microsoft Azure, Google Cloud, Oracle Cloud, IBM Cloud, Alibaba Cloud.

---

### What is vendor lock-in?
Vendor lock-in occurs when systems rely heavily on one cloud provider’s proprietary services — making migration difficult or expensive.

---

### What is multi-cloud?
Using more than one cloud provider at the same time — for flexibility, cost negotiation, or resilience.

---

### What is hybrid cloud?
Combining on-prem systems with cloud environments so workloads can move or integrate across both.

---

## 🔹 2. INTERMEDIATE — HOW CLOUD REALLY WORKS

### Difference between private, public, and hybrid cloud
- **Public** — owned/managed by provider, shared infrastructure  
- **Private** — dedicated to one organization, more control  
- **Hybrid** — bridges both, used for gradual migration or compliance needs

---

### What are data centers?
Highly secured buildings full of servers, storage, networking, cooling, power redundancy, and monitoring — operated by the cloud provider.

---

### What is a region?
A specific geographic location where cloud resources are hosted (e.g., `us-east-1`, `eu-west-1`).

---

### What is an availability zone?
Multiple isolated data centers **inside a region**.  
If one fails, others continue working — improving resilience.

---

### Why are multiple regions used?
- disaster recovery  
- better performance for local users  
- data residency/compliance requirements  
- business continuity

---

### What is latency?
Latency is the time data takes to travel from user → server → back.  
Long distance = higher latency.

---

### Why is latency important in cloud?
High latency causes slow apps, laggy websites, delayed responses — hurting user experience and revenue.

---

### What is auto-scaling?
Automatically adds or removes compute resources based on real-time load — without manual intervention.

---

### What is load balancing?
Distributes traffic across multiple servers so no single server becomes overloaded or fails users.

---

### What is the shared responsibility model?
Security is shared:
- **Provider** protects hardware, network, data centers
- **Customer** secures applications, accounts, data, permissions

---

### Who is responsible for security in cloud?
Both sides — responsibility changes depending on whether it’s IaaS, PaaS, or SaaS.

---

### What is cloud storage?
Highly durable, scalable storage systems accessed via the internet — with built-in redundancy and backups.

---

### What is object storage?
Stores files as objects with metadata — ideal for backups, logs, images, videos, and archives.

---

### What is block storage?
Acts like a physical disk attached to a VM — used for databases and fast transactional workloads.

---

### What is file storage?
Shared file system accessible by multiple servers simultaneously — similar to traditional network drives.

---

### What is a managed service?
Provider handles infrastructure, scaling, patching, and backups — you just use the service.  
Example: managed databases, message queues, Kubernetes services.

---

### What is serverless computing?
You write code — cloud runs it automatically, scales instantly, and charges only for execution time.  
No servers to manage (even though servers still exist underneath).

---

### What is Infrastructure as Code (IaC)?
Create and manage infrastructure using code templates instead of manual clicks.  
Ensures repeatability, version control, and automation.

---

### What is API-driven infrastructure?
Cloud resources can be controlled programmatically through APIs — enabling automation, DevOps, and orchestration.

---

### Difference between cost optimization and cost cutting
Cost optimization = right-size, automate shutdowns, choose correct services.  
Cost cutting = removing resources blindly, often causing outages or performance issues.

---

## 🔹 3. ADVANCED — STRATEGY & ARCHITECTURE THINKING

### Why do enterprises choose cloud over on-prem?
Faster innovation, fewer capital expenses, instant scaling, global reach, automation, and access to advanced services (AI, analytics, IoT).

---

### What challenges exist in cloud migrations?
Legacy systems, data movement, downtime risk, redesign requirements, cost surprises, skill gaps, and cultural resistance.

---

### What is TCO (Total Cost of Ownership)?
Full lifetime cost including hardware, networking, staff, power, licenses, migration, security, monitoring, and operations.

---

### Difference between CAPEX and OPEX
- **CAPEX** — upfront hardware purchases  
- **OPEX** — ongoing pay-as-you-use operating expenses

Cloud primarily shifts IT spend to OPEX.

---

### What is high availability architecture?
Designing systems with redundancy across zones or regions so failure does not cause downtime.

---

### What is disaster recovery (DR)?
Plans and technology used to restore systems and data after catastrophic failure (fire, outage, ransomware, region failure).

---

### What is RTO and RPO?
- **RTO** — how fast service must be restored  
- **RPO** — how much data loss is acceptable (time window)

---

### What is cloud automation?
Automating deployments, scaling, patching, backups, and monitoring — reducing human error and effort.

---

### How do organizations secure data in cloud?
Encryption, IAM policies, network isolation, firewalls, monitoring, zero-trust principles, backups, and governance.

---

### What is data encryption at rest?
Protecting stored data using encryption keys so stolen disks remain unreadable.

---

### What is data encryption in transit?
Encrypting data while moving between systems using HTTPS/TLS to prevent interception.

---

### What is Identity and Access Management (IAM)?
Central system controlling who can access what — users, roles, permissions, MFA, and policies.

---

### Why are roles and policies important?
They prevent excessive access, protect sensitive data, and enforce least-privilege security best practices.

---

### What are compliance frameworks?
Rules such as GDPR, HIPAA, PCI-DSS that define how organizations must handle privacy, data protection, and auditing.

---

### What is logging and monitoring?
Capturing system events, metrics, and errors to detect failures, investigate incidents, and maintain reliability.

---

### What is observability?
Going beyond monitoring — using metrics, logs, traces, and context to deeply understand system behavior and troubleshoot faster.

---

### What is cloud governance?
Policies and controls for:
- cost limits
- security enforcement
- account structure
- naming standards
- approvals and auditing

Keeps the environment organized and safe.

---

### How does cloud help DevOps?
Automated provisioning, CI/CD, infrastructure as code, faster releases, and continuous delivery pipelines.

---

### What is FinOps?
A discipline focused on controlling, analyzing, forecasting, and optimizing cloud spend while still enabling innovation.

---

### When is cloud NOT the right solution?
Very strict regulatory environments, extremely latency-sensitive systems, highly stable legacy workloads, or when predictable fixed costs are preferred.

---

## 🔹 4. SCENARIO-BASED QUESTIONS

### Company wants to reduce hardware cost — how does cloud help?
Instead of buying servers, they rent capacity. Scaling is on demand and hardware maintenance disappears.

---

### Month-end traffic spikes — what’s the cloud strategy?
Use auto-scaling and elastic compute so capacity grows only during peak periods.

---

### Data center crashed — how would cloud improve resilience?
Deploy across multiple availability zones or regions with automated failover.

---

### Deploy globally — how do regions help?
Place applications closer to users in different continents, lowering latency and improving experience.

---

### Application slow for another country — what can you do?
Use nearest region, CDN caching, replication, or geo-load balancing.

---

### Customer data must stay in a country — what is required?
Choose compliant regions and apply data residency and governance rules.

---

### Monthly bill doubled suddenly — where to look?
Unstopped test environments, over-scaled resources, storage growth, data transfer, misconfiguration, unused services.

---

### System must not go down during updates — design idea?
Blue-green deployments, rolling updates, and duplicate environments.

---

### Need long-term backups — which storage?
Archival object storage (cheap, slower retrieval but durable).

---

### Developers need environments quickly — what helps?
Templates, IaC scripts, managed environments, and self-service provisioning.

---

### Restrict permissions by role — what implement?
IAM roles, RBAC, policies, and least-privilege enforcement.

---

### Startup wants fast prototypes — best model?
PaaS or serverless — minimal setup, rapid iteration, low cost.

---

### Avoid vendor lock-in — strategies?
Containers, Kubernetes, standard APIs, portable architectures, minimal proprietary features.

---

### Regulation requires audit logs — cloud support?
Central logging services, immutable logs, retention policies, alerts, audit dashboards.

---

### ETL workloads explode overnight — how elasticity helps?
The cloud automatically allocates extra compute temporarily, then scales back to avoid permanent cost.

---

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS

### Is cloud always cheaper?
No. Cloud can be cheaper — but only when designed correctly. Poor sizing, always-on resources, over-scaling, and unused services can make cloud more expensive than on-prem.

---

### Does cloud mean “no servers”?
No. Servers still exist — but you do not buy, install, or manage them. The provider owns the hardware. You simply consume resources on demand.

---

### Does provider handle all security?
No. Security is shared. The provider protects the data centers and infrastructure, but you must secure apps, data, identities, access, encryption, and configuration. A misconfigured bucket or database is still your responsibility.

---

### Is data automatically safer in cloud?
Not automatically. Cloud provides strong tools, but wrong permissions, public networks, weak passwords, and no encryption can still expose data.

---

### Does cloud remove the need for engineers?
No. Engineers are still needed — but their work shifts to design, automation, DevOps, security, monitoring, governance, and cost control.

---

### Are backups unnecessary?
Absolutely not. Accidental deletes, corruption, ransomware, and human mistakes still happen. Backups and recovery plans are critical.

---

### Does auto-scaling fix bad design?
No. Auto-scaling just gives more resources — it cannot fix poor architecture, inefficient queries, memory leaks, or bad code.

---

### Is serverless “no servers”?
No. Servers still run your code — but they are provisioned, patched, scaled, and shut down automatically by the provider. You only focus on the logic.

---

### Can cloud remove latency?
No. Data still travels over networks and long distances. Placing workloads closer to users and using CDNs only reduces — not eliminates — latency.

---

### Are all cloud services fully managed?
No. Some services are fully managed, others partially managed, and many still require configuration, tuning, and maintenance by customers.

---

### Is multi-cloud always better?
No. Multi-cloud can reduce lock-in, but adds complexity, cost, duplicated effort, and harder troubleshooting. Use it only when justified.

---

### If data is deleted, is it gone forever?
Not always. Cloud often supports versioning, backups, snapshots, and retention policies — depending on how systems are configured.

---

### Is migration only a technical task?
No. Successful migration requires business planning, budgeting, training, process redesign, governance, and organizational change.

---

### Are on-prem systems obsolete?
No. Many organizations still need on-prem for compliance, legacy apps, strict latency, or control — often using hybrid cloud.

---

### Can every workload move easily?
No. Some legacy, real-time, regulated, or hardware-dependent workloads are

