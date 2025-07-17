# **Snowflake Dynamic Tables: Real-Time Data Management**

## **What are Snowflake Dynamic Tables?**
Snowflake **Dynamic Tables** are a **declarative materialized view** that **automatically refreshes** at a defined interval, ensuring near real-time data updates. They simplify ETL/ELT workloads by **automating transformations** and **reducing manual data movement**.

Unlike traditional tables, **Dynamic Tables maintain themselves** using a `TARGET_LAG` setting, meaning Snowflake continuously updates the table within a specified lag time.

---

## **How Snowflake Dynamic Tables Work**
1. Define a **Dynamic Table** using `CREATE DYNAMIC TABLE`.
2. Specify **data transformation logic** in a `SELECT` statement.
3. Set a **TARGET_LAG** to control how frequently the table updates.
4. Snowflake **automatically refreshes** the table based on upstream changes.

---

## **Use Cases for Snowflake Dynamic Tables**

### **1. Simplified ETL/ELT Pipelines**
- **Traditional ETL** requires manual scheduling of transformations.
- With **Dynamic Tables**, **data transformation is automatic** after ingestion.
- **Example:** Transform raw JSON logs into structured tables without scheduled jobs.

### **2. Real-Time Analytics & Reporting**
- Businesses require **low-latency updates** for dashboards.
- Dynamic Tables ensure **data freshness** within a predefined lag time.
- **Example:** A BI tool querying sales data can access **near real-time updates**.

### **3. CDC (Change Data Capture)**
- Dynamic Tables **automatically refresh** when new data arrives.
- **Example:** Track incremental customer transactions from an event stream.

### **4. Data Warehousing and Data Lakes**
- Acts as an **auto-updating layer** on top of raw data lakes.
- **Example:** Convert semi-structured JSON/Parquet into structured tables.

---

## **Creating a Dynamic Table in Snowflake**
Here's how to create a **Dynamic Table** for a transformed customer dataset:

```sql
CREATE OR REPLACE DYNAMIC TABLE customer_summary
TARGET_LAG = '5 minutes'
WAREHOUSE = my_warehouse
AS 
SELECT 
    customer_id, 
    COUNT(order_id) AS total_orders, 
    SUM(order_amount) AS total_spent
FROM raw_orders
GROUP BY customer_id;
