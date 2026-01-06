# INTRODUCTION TO DATABASES

## 🔹 BASIC

### What is a database?
A structured place to store data so it can be searched, updated, shared, and protected efficiently.

### What is data vs information?
- Data = raw facts (100, “John”)
- Information = processed data that has meaning (“John paid $100”)

### What is DBMS?
Software that stores, manages, secures, and recovers databases (MySQL, Oracle, SQL Server).

### What is RDBMS?
A DBMS that stores data in tables and supports relationships using keys (MySQL, PostgreSQL, Oracle).

### What is a table?
A structure made of rows and columns used to store related data.

### What is a row (record)?
One instance/object — e.g., one student, one order.

### What is a column (field)?
One attribute — e.g., name, age, email — with a defined data type.

### What is a primary key?
A column (or combination) that uniquely identifies each record.

### Why do we need a primary key?
Prevents duplicates, enables reliable relationships and updates.

### What happens if there is no primary key?
Duplicates, inconsistent records, broken joins, confusion.

### What is a foreign key?
A column that references another table’s primary key to form relationships.

### What is a relationship?
A logical link between tables so data is not duplicated everywhere.

### Types of relationships?
- One-to-One
- One-to-Many
- Many-to-Many (requires junction table)

### What is a schema?
The blueprint of the database — tables, columns, datatypes, rules.

### What is metadata?
“Data about data” (column types, indexes, creation details).

### What is SQL?
Language used to query and manage relational databases.

### Difference between SQL and database?
SQL = language  
Database = storage system

### What is normalization?
Organizing data to reduce redundancy and anomalies.

### Why do we normalize?
To avoid duplicates, inconsistencies, and difficult updates.

### What is denormalization?
Intentionally duplicating data to improve read performance.

### What is a transaction?
A unit of work that must complete fully or roll back completely.

### What is ACID?
Reliability guarantees: Atomicity, Consistency, Isolation, Durability.

### What is atomicity?
All operations succeed or none do.

### What is consistency?
Data must obey rules before and after a transaction.

### What is isolation?
Parallel transactions shouldn’t affect each other’s results.

### What is durability?
Committed data remains safe even after crashes.

### What is OLTP?
Systems optimized for frequent transactions (banking, apps).

### What is OLAP?
Systems optimized for analytics and reporting on large datasets.

---

## 🔹 INTERMEDIATE

### Why aren’t spreadsheets the same as databases?
Spreadsheets lack integrity rules, multi-user safety, scalability, and transactions. Databases enforce all of these.

### When would you use an RDBMS?
When accuracy, relationships, ACID transactions, and structured data matter.

### Difference between RDBMS and NoSQL.
RDBMS — structured, ACID, relationships  
NoSQL — flexible schema, scalable, good for large or semi-structured data

### What is index? Why is it used?
A structure that speeds lookups — like a book index — reducing table scans.

### What problems does indexing solve?
Slow searches, expensive joins, and heavy table scans.

### What happens if you over-index?
Slower inserts/updates, more storage, more maintenance.

### What are constraints?
Rules that ensure valid data (PK, FK, UNIQUE, NOT NULL, CHECK).

### What is NOT NULL?
Prevents missing values.

### What is UNIQUE?
Ensures no duplicate values in a column.

### What is a composite key?
Primary key made from multiple columns.

### What is a surrogate key?
System-generated key (like auto-increment ID) used instead of natural business keys.

### Why surrogate instead of natural keys?
Natural keys change; surrogate keys stay stable and simple.

### What is referential integrity?
Ensures relationships remain valid — no orphan records.

### What is cascading delete?
Deleting a parent automatically deletes related child rows (use carefully).

### How does a database store data internally?
On disk as pages/blocks with indexes, logs, and metadata for fast access.

### Why do databases enforce rules instead of applications?
Central enforcement ensures all apps stay consistent.

---

## 🔹 ADVANCED THINKING

### Can we have a table without any key?
Yes — but it leads to duplicates and tracking problems. Bad design except for staging.

### Why might denormalization sometimes be better?
Reduces joins, speeds reporting, improves analytics queries.

### When is duplication acceptable?
Caching, analytics, data warehousing, history tracking.

### Why do OLTP systems avoid complex joins?
They slow inserts/updates and increase locking.

### Why do OLAP systems allow heavy analytics?
Data is read-only and optimized for aggregations.

### How does concurrency affect transactions?
Users may conflict — databases use locking and isolation levels.

### Why are isolation levels required?
To balance data correctness vs performance.

### What happens during deadlock?
Two transactions wait forever — DB cancels one.

### Why can normalization slow down reads?
More joins required to reconstruct data.

### How would you choose between SQL and NoSQL?
Relationships + ACID → SQL  
Scale + flexible schema → NoSQL

### Why are logs critical in databases?
Support recovery, rollback, replication, and auditing.

---

## 🔹 SCENARIO QUESTIONS

### A table is receiving duplicate data — how do you fix it?
Identify duplicates → remove → add UNIQUE/PK → fix source logic.

### A user deleted records accidentally — what can be done?
Restore from backup, transaction logs; consider soft deletes.

### A report query is slow — where do you start?
Check indexes, joins, execution plan, and filters.

### Team wants fast inserts and updates — what type of DB?
OLTP RDBMS with minimal indexing.

### You need historical changes for customers — design idea?
Use audit/history tables or Slowly Changing Dimensions (Type 2).

### How would you prevent unauthorized access?
Roles, least privilege, encryption, auditing.

### How do you migrate from Excel to DB?
Clean → model tables → import → validate.

---

## 🔹 TRICK / MISCONCEPTION QUESTIONS

### Does primary key automatically create an index?
Yes — in most RDBMS.

### Can a primary key be NULL?
No.

### Can a table have more than one primary key?
No — but it can have a composite primary key.

### Can foreign keys be NULL?
Yes — if relationship is optional.

### Does normalization always improve performance?
Improves integrity — may slow reads.

### Are database backups and logs the same?
No — backups store full data, logs record changes.

### Does deleting data always reduce storage immediately?
Often no — storage frees after cleanup processes.

### Are SQL databases only for structured data?
Primarily structured, but many support JSON/XML.
