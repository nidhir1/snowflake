```mermaid
flowchart LR
A[Post-Staging Curated Data] --> B[BI & Reporting]
A --> C[ML & Data Science]
A --> D[Data Sharing & APIs]
A --> E[Monitoring & Governance]
A --> F[Lifecycle Management & Archival]
E --> G[Security & Compliance]
B --> H[User Feedback]
C --> H
D --> H
H --> A
```


```mermaid
flowchart LR
A[Post-Staging Curated Data] --> B[BI & Reporting]
A --> C[ML & Data Science]
A --> D[Data Sharing & APIs]
A --> E[Monitoring & Governance]
A --> F[Lifecycle Management & Archival]
E --> G[Security & Compliance]
B --> H[User Feedback]
C --> H
D --> H
H --> A
```

## Workflow Diagram
```mermaid
flowchart LR
A[Staging: Raw + Cleaned Data] --> B[Transformation & Enrichment]
B --> C[Data Modeling (Fact & Dim Tables)]
C --> D[Incremental Loads via Streams & Tasks]
D --> E[Data Quality Monitoring & Alerts]
E --> F[Serving Data to BI, ML, APIs]
F --> G[Governance & Security]
G --> H[Lifecycle Management & Archival]
H --> B
```


```mermaid
flowchart LR
  A[Staging: Raw + Cleaned Data] --> B[Transformation & Enrichment]
  B --> C[Data Modeling - Fact & Dim Tables]
  C --> D[Incremental Loads via Streams & Tasks]
  D --> E[Data Quality Monitoring & Alerts]
  E --> F[Serving Data to BI, ML, APIs]
  F --> G[Governance & Security]
  G --> H[Lifecycle Management & Archival]
  H --> B
```