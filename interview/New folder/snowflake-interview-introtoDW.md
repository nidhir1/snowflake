# INTRODUCTION TO DATA WAREHOUSING — FULL QUESTION BANK

---

## 🔹 1. BASIC — FOUNDATIONAL QUESTIONS

1. What is a data warehouse?  
2. Why do organizations need data warehouses?  
3. Difference between operational databases and data warehouses.  
4. What is OLTP?  
5. What is OLAP?  
6. Why are OLTP and OLAP separated?  
7. What is historical data?  
8. Why do we store historical data in warehouses?  
9. What is a data mart?  
10. What is the difference between a data mart and a data warehouse?  
11. What is ETL?  
12. What is ELT?  
13. Examples of BI / reporting tools.  
14. What is structured data?  
15. What is semi-structured data?  
16. What is unstructured data?  
17. Why is consistency important in reporting?  
18. What is batch data processing?  
19. What is real-time / streaming processing?  
20. What is metadata in a data warehouse?  

---

## 🔹 2. INTERMEDIATE — UNDERSTANDING HOW IT WORKS

1. Why don’t we directly report from OLTP systems?  
2. What performance challenges exist in OLTP for reporting?  
3. What are facts?  
4. What are dimensions?  
5. What is a fact table?  
6. What is a dimension table?  
7. What is granularity (grain) in a fact table?  
8. Why is granularity important?  
9. What is star schema?  
10. What is snowflake schema (model, not product)?  
11. Difference between star & snowflake schema.  
12. What is denormalization in data warehousing?  
13. What are surrogate keys?  
14. Why don’t we always use natural keys?  
15. What is Slowly Changing Dimension (SCD)?  
16. What are SCD Types 0, 1, 2, 3?  
17. What is a staging area?  
18. Why is staging required?  
19. What is data cleansing?  
20. What is data transformation?  
21. What is data validation in ETL?  
22. What is data loading strategy?  
23. What is full load?  
24. What is incremental load?  
25. What is change data capture (CDC)?  
26. Why do we need indexes in some systems?  
27. Why is data quality important?  

---

## 🔹 3. ADVANCED — DESIGN & ARCHITECTURE THINKING

1. Explain the components of a data warehouse architecture.  
2. What is a data lake vs data warehouse?  
3. Can both coexist? Why?  
4. What is a lakehouse?  
5. What is enterprise data warehouse (EDW)?  
6. What are conformed dimensions?  
7. Why are conformed dimensions important?  
8. What is a bus architecture?  
9. What is data lineage?  
10. What is master data management (MDM)?  
11. What is data governance?  
12. What is data catalog?  
13. Why do companies need audit trails?  
14. How do we manage slowly changing historical data?  
15. What is partitioning?  
16. Why is partitioning useful?  
17. What are materialized views?  
18. Difference between logical and physical data models?  
19. What is surrogate key vs business key debate?  
20. Why is performance tuning critical in warehouses?  
21. What are common ETL failures?  
22. Why is monitoring important?  
23. What is workload isolation?  

---

## 🔹 4. SCENARIO-BASED QUESTIONS — REAL PROJECT THINKING

1. Business users want trend analysis for last 10 years — how do you design?  
2. Reports became slow after adding more data — what might be wrong?  
3. Sales data is scattered across multiple systems — how do you consolidate?  
4. Analytics team complains of duplicate records — root causes?  
5. Customer records keep changing — how do we preserve history?  
6. ETL process failed in the middle — how do you recover?  
7. User requests real-time dashboards — what would you consider?  
8. Business wants single source of truth — what steps help?  
9. Data from two systems doesn’t match — how do you debug?  
10. Old reports show different totals from new warehouse — what do you check?  
11. Source system changed schema — warehouse impact?  
12. A new country launches operations — how do you onboard into warehouse?  
13. Data volume doubled suddenly — what adjustments are needed?  
14. Sensitive fields appear in reports — what should be implemented?  
15. Executive team wants aggregated KPIs only — modeling considerations?  

---

## 🔹 5. TRICK / MISCONCEPTION QUESTIONS

1. Is a data warehouse only for structured data?  
2. Is a data warehouse the same as a database?  
3. Does normalization always improve performance?  
4. Is ELT always better than ETL in cloud?  
5. Are indexes commonly required like OLTP systems?  
6. Does deleting data instantly reduce warehouse storage?  
7. Is star schema always better than snowflake schema?  
8. Are historical snapshots stored in OLTP systems?  
9. Does every warehouse require a staging area?  
10. Can we directly use spreadsheets instead of a data warehouse?  
11. Do real-time systems remove the need for data warehouses?  
12. Does every organization need SCD?  
13. If business key changes, should we change surrogate key?  
14. Does reporting always read from fact tables?  
15. Are dimensions alwa
