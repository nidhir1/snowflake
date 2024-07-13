![Snowflake Tinitiate Image](snowflake_tinitiate.png)

# Snowflake
&copy; TINITIATE.COM

##### [Back To Context](./README.md)

# Database, Schema, Role and User
* In Snowflake, a database serves as a structured collection of data.
* A schema acts as a logical container for organizing and grouping database objects, such as tables, views, and stored procedures.
* Lastly, a user in Snowflake is an entity that holds access rights to interact with the Snowflake system and its databases.
* To make it even short:
    * Snowflake software is called the account
    * In an account, we can create multiple databases
    * Each database has logical grouping of database objects called Schema
    * The schema within a database can be accessed by users, which are the individual user logins.

## Database:
* In Snowflake, a database is a structured collection of data that is organized and managed for easy access and retrieval.
* It stores tables, views, stored procedures, and other objects that define the schema and structure of the data.
* Within a Snowflake account, you can create multiple databases to organize your data.
```sql
-- Create database tinitiate
CREATE DATABASE tinitiate;

-- Use the database tinitiate
USE DATABASE tinitiate;
```
## Role and User:
* In Snowflake, users are individual logins that can access the database objects within a schema.
* Roles are created to manage the permissions granted to users.
* These users and roles are configured by the database administrator.
```sql
-- Create a role named 'tirole'
CREATE ROLE tirole;

-- Create a role named 'developer_role'
CREATE ROLE developer_role;

-- Create a user named 'tiuser' with the password 'Tinitiate!23'
CREATE USER tiuser PASSWORD = 'Tinitiate!23';

-- Create a user named 'developer' with the password 'Tinitiate!23'
CREATE USER developer PASSWORD = 'Tinitiate!23';

-- Grant the role to the user
GRANT ROLE tirole TO USER tiuser;
GRANT ROLE developer_role TO USER developer;
```
## Schema:
* In Snowflake, each database contains a logical grouping of database objects called schemas.
* Schemas provide a way to organize and namespace database objects within a database.
```sql
-- Use the database where you want to create the schema
USE DATABASE tinitiate;

-- Create the schema
CREATE SCHEMA employees;

-- Grant ownership of the schema to the role
GRANT OWNERSHIP ON SCHEMA employees TO ROLE tirole;
```

##### [Back To Context](./README.md)
***
| &copy; TINITIATE.COM |
|----------------------|