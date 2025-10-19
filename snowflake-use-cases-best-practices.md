# Data Governance and Compliance

## 1. Data Governance in Snowflake

### Setting Up Data Governance Policies
Snowflake provides several built-in capabilities to support **data governance** and ensure controlled access to enterprise data assets.

**Key Components:**
1. **Role-Based Access Control (RBAC):**
   - Use roles and privileges to segregate duties and manage user permissions.
   ```sql
   CREATE ROLE data_steward;
   GRANT SELECT ON DATABASE sales_db TO ROLE data_steward;
   GRANT ROLE data_steward TO USER yogesh;
   ```

2. **Tag-Based Governance:**
   - Use **object tags** to classify and track sensitive data (e.g., PII, financial).  
   ```sql
   CREATE TAG pii_tag COMMENT = 'Personally Identifiable Information';
   ALTER TABLE customers MODIFY COLUMN email SET TAG pii_tag = 'true';
   ```

3. **Data Masking Policies:**
   - Apply dynamic masking for sensitive columns.
   ```sql
   CREATE MASKING POLICY mask_ssn AS (val STRING) RETURNS STRING ->
   CASE
     WHEN CURRENT_ROLE() IN ('DATA_ADMIN') THEN val
     ELSE 'XXX-XX-XXXX'
   END;
   ALTER TABLE employee MODIFY COLUMN ssn SET MASKING POLICY mask_ssn;
   ```

4. **Row Access Policies:**
   - Control visibility at the row level for different user roles.
   ```sql
   CREATE ROW ACCESS POLICY region_filter AS (region STRING) RETURNS BOOLEAN ->
     region = CURRENT_REGION();
   ALTER TABLE orders ADD ROW ACCESS POLICY region_filter ON (region);
   ```

### Ensuring Data Quality and Consistency
- Implement **data validation layers** using Snowflake Streams and Tasks.  
- Integrate **data catalog tools** like Collibra or Alation.  
- Establish **data lineage** and **metadata tracking** via Snowflake’s `INFORMATION_SCHEMA` and `ACCOUNT_USAGE` views.

**Example Quality Check Task:**
```sql
CREATE OR REPLACE TASK dq_check_task
  WAREHOUSE = etl_wh
  SCHEDULE = '1 HOUR'
AS
  INSERT INTO dq_audit_log
  SELECT COUNT(*) AS invalid_records, CURRENT_TIMESTAMP()
  FROM customer_data
  WHERE email NOT LIKE '%@%';
```

---

## 2. Compliance and Security

### GDPR, HIPAA, and Other Regulatory Compliance
Snowflake provides compliance certifications such as:  
- **GDPR (General Data Protection Regulation)**  
- **HIPAA (Health Insurance Portability and Accountability Act)**  
- **PCI DSS**, **SOC 1/2/3**, **FedRAMP**, **ISO 27001**

**Best Practices for Compliance:**
- Classify sensitive data using **object tags**.  
- Use **time travel and fail-safe** for auditability.  
- Enable **encryption at rest and in transit** (AES-256 + TLS).  
- Restrict data export via **network policies** and **resource monitors**.

### Implementing Data Security Best Practices
| Security Feature | Description |
|------------------|-------------|
| **Multi-Factor Authentication (MFA)** | Adds second-level authentication to access Snowflake. |
| **Network Policies** | Restrict login IP ranges and enforce secure network access. |
| **Key Pair Authentication** | Use RSA key pairs for non-interactive access. |
| **Access History View** | Audit user activity across databases. |

**Example: Network Policy**
```sql
CREATE NETWORK POLICY secure_access
ALLOWED_IP_LIST = ('10.10.0.0/16', '192.168.1.0/24')
BLOCKED_IP_LIST = ('0.0.0.0/0');
ALTER ACCOUNT SET NETWORK_POLICY = secure_access;
```

---

# Real-World Use Cases and Best Practices

## 1. Real-World Use Cases

### Case Studies of Snowflake Implementations
1. **Financial Services (Citi, JPMorgan):**
   - Built **enterprise data lake** with real-time ingestion (Kafka → Snowflake).  
   - Used **Snowpark Python** for predictive risk modeling.

2. **Healthcare (Regeneron, IQVIA):**
   - Used **Snowflake Streams + Tasks** for continuous CDC ingestion.  
   - Ensured HIPAA compliance via row-level security and masking policies.

3. **Retail (Walmart, Target):**
   - Implemented **real-time inventory analytics** with Snowpipe + Tableau.  
   - Leveraged **data sharing** for supplier analytics.

### Industry-Specific Applications of Snowflake
| Industry | Use Case | Benefit |
|-----------|-----------|----------|
| **Banking** | Trade Finance & Loan Reporting | Single source of truth for regulatory reporting |
| **Healthcare** | Clinical Trials Data Integration | Faster insights from unified patient data |
| **Retail** | Demand Forecasting | Real-time sales and trend analysis |
| **Pharma** | Drug Efficacy Analytics | Secure data sharing with partners |

---

## 2. Best Practices for Snowflake

### Effective Data Management Strategies
- **Separate environments** for Dev, QA, and Prod using different Snowflake databases.  
- **Define governance roles**: Data Owners, Stewards, and Custodians.  
- **Leverage Snowflake Tasks and Streams** for incremental data refresh.  
- Regularly **monitor resource usage** with `ACCOUNT_USAGE` views.

### Tips for Optimizing Snowflake Usage
| Category | Recommendation |
|-----------|----------------|
| **Storage** | Use compression and clustering keys effectively |
| **Compute** | Use multi-cluster warehouses for concurrency |
| **Performance** | Enable auto-suspend/auto-resume for cost savings |
| **Security** | Apply masking, tagging, and RBAC consistently |
| **Data Sharing** | Use Secure Data Sharing instead of manual exports |

**Example: Cost Optimization**
```sql
ALTER WAREHOUSE etl_wh SET AUTO_SUSPEND = 300;
ALTER WAREHOUSE etl_wh SET AUTO_RESUME = TRUE;
```

---

## Summary

| Concept | Description |
|----------|--------------|
| **Governance** | Policies for security, access control, and lineage |
| **Compliance** | GDPR, HIPAA, SOC, PCI standards supported |
| **Use Cases** | Real implementations across industries |
| **Best Practices** | Cost, performance, and governance optimization |

---

**References:**
- [Snowflake Data Governance](https://docs.snowflake.com/en/user-guide/security-access-control-overview)
- [Snowflake Security Guide](https://docs.snowflake.com/en/user-guide/security-intro)
- [Compliance Certifications](https://www.snowflake.com/compliance/)
- [Snowflake Best Practices](https://docs.snowflake.com/en/user-guide/performance-best-practices)
