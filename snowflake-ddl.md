![Snowflake Tinitiate Image](snowflake_tinitiate.png)

# Snowflake
&copy; TINITIATE.COM

##### [Back To Context](./README.md)

# DDL - Data Definition Language   (Create/Alter/Drop)

* In Snowflake, DDL (Data Definition Language) encompasses a group of SQL commands that are used to create, modify, and remove the structure of database objects. These objects include tables, indexes, views, schemas, sequences, and more.
* DDL statements are crucial for establishing the database schema, defining relationships between tables, and ensuring data integrity. They serve as the foundation for organizing and managing data within Snowflake databases.

## Primary DDL commands in Snowflake:
### CREATE:
* Used to create new database objects such as tables, indexes, views, schemas, sequences, and other objects.
```sql
-- Use Database
USE DATABASE tinitiate;

-- Schema DDL
CREATE SCHEMA emp;
CREATE USER ti PASSWORD = 'Tinitiate!23';
GRANT ROLE tirole TO USER ti;

-- Set the schema where you want to create the DB objects
USE SCHEMA emp;

-- DDL Create Command
-- Create dept table
CREATE TABLE dept (
    deptid  int,
    dname   varchar(100)
);

-- Create emp table
CREATE TABLE emp (
    empid    int,
    ename    varchar(100),
    sal      numeric(7,2),
    deptid   int
);

-- Create projects table
CREATE TABLE projects (
    ProjectID      int,
    ProjectName    varchar(100),
    ProjectBudget  numeric(12,2)
);

-- Create empprojects table
CREATE TABLE EmpProjects (
    EP_ID       int,
    EmpID       int,
    ProjectID   int,
    StartDate   date,
    EndDate     date
);
```

### ALTER:
* Modifies the structure of existing database objects, such as adding or dropping columns from a table.
```sql
-- Alter table "emp": Add a new column called "hire_date" of type DATE.
ALTER TABLE emp ADD hire_date DATE;
-- To change back to previous
ALTER TABLE emp DROP COLUMN hire_date;

-- Alter table "projects":
-- Change the data type of the column "ProjectBudget" to DECIMAL(12,2).
ALTER TABLE projects ALTER COLUMN ProjectBudget SET DATA TYPE DECIMAL(12,2);
-- To change back to previous
ALTER TABLE projects ALTER COLUMN ProjectBudget SET DATA TYPE NUMERIC(12,2);

-- Alter table "EmpProjects": Drop the column "EndDate".
ALTER TABLE EmpProjects DROP COLUMN EndDate;
-- To change back to previous
ALTER TABLE EmpProjects ADD COLUMN EndDate DATE;
```

### DROP:
* Deletes existing database objects, such as tables, indexes, or views.
```sql
-- To drop dept table in emp schema
DROP TABLE dept;

-- To again create it
CREATE TABLE dept (
    deptid  INT,
    dname   VARCHAR(100)
);
```

##### [Back To Context](./README.md)
***
| &copy; TINITIATE.COM |
|----------------------|