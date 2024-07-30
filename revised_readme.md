![Snowflake Tinitiate Image](snowflake_tinitiate.png)
# Snowflake
&copy; TINITIATE.COM

## CONTEXT
* ### [Introduction to Databases](introduction-to-databases.md)
* ### [Introduction to Cloud Platforms](introduction-to-cloud-platforms.md)
* ### [Infrastructure As A Service (IaaS)](infrastructure-as-a-service.md)
* ### [Platform As A Service (PaaS)](platform-as-a-service.md)
* ### [Software As A Service (SaaS)](software-as-a-service.md)
* ### [Introduction To Snowflake](introduction-to-snowflake.md)
* ### [Snowflake SaaS Cloud Platform](snowflake-saas-cloud-platform.md)
* ### [Traditional RDBMS vs Snowflake Platform](traditional-rdbms-vs-snowflake-platform.md)
* ### [Snowflake Versions, Architecture, and Pricing](snowflake-versions-architecture-and-pricing.md)
* ### [Working With Snowflake Components](working-with-snowflake-components.md)
* ### [Database, Schema, Role, and User](snowflake-db-sch-role-user.md)
* ### [Datatypes](snowflake-datatypes.md)
* ### [DDL](snowflake-ddl.md)
    * Create
    * Alter
    * Drop
* ### [DDL - Constraints](snowflake-ddl-constraints.md)
    * Not Null
    * Unique
    * Check
    * Primary Key
    * Foreign Key
* ### [DML](snowflake-dml.md)
    * Insert
    * Update
    * Delete
* ### [DQL Data Setup](snowflake-dql-data-setup.md)
* ### [DQL](snowflake-dql.md)
    * Select
    * Where
    * Group By
    * Having
    * Order By
    * Limit
* ### [DQL - Basic Operators](snowflake-dql-basic-operators.md)
    * Equality Operator (=)
    * Inequality Operator (<>)
    * IN Operator
    * NOT IN Operator
    * LIKE Operator
    * NOT LIKE Operator
    * BETWEEN Operator
    * Greater Than (>)
    * Greater Than or Equal To (>=)
    * Less Than (<)
    * Less Than or Equal To (<=)
    * EXISTS Operator
    * NOT EXISTS Operator
* ### [DQL - Set Operations](snowflake-dql-set-operations.md)
    * Union
    * Intersect
    * Except
* ### [DQL - Joins](snowflake-dql-joins.md)
    * Inner Join
    * Left Join (or Left Outer Join)
    * Right Join (or Right Outer Join)
    * Full Join (or Full Outer Join)
    * Cross Join
* ### [DQL - String Functions](snowflake-dql-string-functions.md)
    * Length Function (LENGTH)
    * Substring Function (SUBSTR or SUBSTRING)
    * Concatenation Operator (||)
    * Lower Function (LOWER)
    * Upper Function (UPPER)
    * Trim Function (TRIM)
    * Ltrim Function (LTRIM)
    * Rtrim Function (RTRIM)
    * Position Function (POSITION)
    * Left Function (LEFT)
    * Right Function (RIGHT)
    * Reverse Function (REVERSE)
    * Replace Function (REPLACE)
    * Case Statement (CASE)
    * NVL Function (NVL)
    * Coalesce Function (COALESCE)
    * SPLIT Function (SPLIT)
    * INITCAP Function (INITCAP)
* ### [DQL - Date Functions](snowflake-dql-date-functions.md)
    * Current Date and Time (CURRENT_TIMESTAMP)
    * Date Part Function (DATE_PART)
    * EXTRACT Function (EXTRACT)
    * Date Difference Function (DATEDIFF)
    * Date Addition/Subtraction (DATEADD, DATESUB)
    * Date Truncation (DATE_TRUNC)
    * Date Formatting (TO_CHAR)
    * Weekday Function (DAYOFWEEK)
    * Date to String (Various formats)
    * DateTime to String (Various formats)
    * String to Date (TO_DATE)
    * String to DateTime (TO_TIMESTAMP)
    * DateTime and TimeZone
        * Date, Timezones, UTC, and Offsets
            * Date and Time
            * Timezones
            * UTC
            * Offsets
            * Applying an Offset to a Date Datatype Column
        * Cast a DateTime to DateTime with Timezone (in UTC, EST, and IST TimeZones)
        * Cast a DateTime Timezone to another TimeZone
* ### [DQL - Aggregate Functions](snowflake-dql-aggregate-functions.md)
    * Count
    * Sum
    * Avg
    * Max
    * Min
* ### [DQL - Analytical Functions](snowflake-dql-analytical-functions.md)
    * Aggregate Functions
    * ROW_NUMBER()
    * RANK()
    * DENSE_RANK()
    * NTILE(n)
    * LAG()
    * LEAD()
    * FIRST_VALUE()
    * LAST_VALUE()
* ### [DQL - Common Table Expressions (CTEs)](snowflake-dql-cte.md)
    * Creating a CTE
    * Using Multiple CTEs
    * Recursive CTEs
    * Common Use Cases for CTEs
    * Benefits of Using CTEs
* ### [Snowflake Cloning](snowflake-cloning.md)
    * Zero Copy Cloning
    * Schema Level Cloning
    * Real-time Uses
    * Storage and Metadata Layers
* ### [Transient Tables](snowflake-transient-tables.md)
    * Creating Transient Tables
    * Use Cases for Transient Tables
    * Differences Between Permanent and Transient Tables
* ### [Security Management](snowflake-security-management.md)
    * Security Entities
    * Securable Objects, Users & Roles
    * Privileges and Privilege Groups
    * Security Hierarchy
    * Role-Based Access Control (RBAC)
    * Role Hierarchy and Dependency
    * Auditing Users and Password Policy
* ### [Transactions](snowflake-transactions.md)
    * ACID Properties
    * Transaction Types
    * Implicit, Explicit, and Auto Commit
    * DDL Statements and Transactions
    * BEGIN TRANSACTION & COMMIT
    * current_transaction() Usage
    * Failed Transactions with SPs
    * Transactions with Stored Procedures
* ### [Streams & Audits](snowflake-streams-audits.md)
    * Stream Object and DML Auditing
    * Stream Types: Standard, Append Only, Insert Only
    * Data Flow with Streams
    * Time Travel with Streams
    * Auditing INSERT, UPDATE, DELETE
* ### [ETL, Stages & Pipes](snowflake-etl-stages-pipes.md)
    * Tasks and Partitions
    * Directed Acyclic Graph (DAG)
    * Tasks Schedules and RESUME Options
    * Partitions: Micro Partition, Cluster Key, ReClusters
    * Stages: Internal and External
    * User and Table Stages
    * COPY Command, Bulk Data Loads
* ### [Advanced Features](snowflake-advanced-features.md)
    * Snowflake on Azure & External Storage
    * SnowPipes & Incremental Loads
    * Data Unloading Concepts
    * External Data Stages
    * Snowflake Integration with Azure Data Factory
    * Data Governance in Snowflake
    * Secure Data Sharing
    * Snowflake Data Marketplace
* ### [SnowSQL and Scripting](snowsql-scripting.md)
    * SnowSQL Concepts and Client Installation
    * Working with SnowSQL: DDL, DML, SELECT Operations
    * Variables, Batch Processing
    * Snowflake SQL Query Syntax
* ### [Performance Tuning](snowflake-performance-tuning.md)
    * Indexes and Performance Tuning
    * Clustering and Partitioning
    * Query Optimization: Query Profiler, Best Practices
    * Caching Mechanisms: Result, Metadata, Data
* ### [Python and Other Tools](snowflake-python-tools.md)
    * Using Python with Snowflake: Pandas, Snowpark
    * Boto3 for AWS Integration
    * Data Movement between Snowflake and Other RDBMS
    * Building Applications: Data Pipelines, ML Models, Web Applications
    * Integrating Snowflake with BI Tools: Tableau, Power BI
* ### [Data Governance](snowflake-data-governance.md)
    * Setting Up Data Governance Policies
    * Ensuring Data Quality and Consistency
    * Compliance and Security: GDPR, HIPAA
    * Implementing Data Security Best Practices
* ### [Use Cases and Best Practices](snowflake-use-cases-best-practices.md)
    * Real-World Use Cases: Case Studies, Industry-Specific Applications
    * Best Practices for Snowflake: Effective Data Management, Optimization Tips

***
| &copy; TINITIATE.COM |
|----------------------|
