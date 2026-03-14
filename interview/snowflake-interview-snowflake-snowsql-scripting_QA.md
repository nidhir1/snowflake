# ❄️ SnowSQL --- Concepts, Installation, Automation & Scripting

## Teaching Guide (Ques / Ans Format)

------------------------------------------------------------------------

# 🔹 1️⃣ SNOWSQL CONCEPTS & CLIENT INSTALLATION

## Ques: What is SnowSQL?

**Ans:**\
SnowSQL is a **command-line client** provided by Snowflake that allows
users to connect to Snowflake and execute SQL queries from the terminal.

It works similarly to database CLI tools such as:

-   psql (PostgreSQL)
-   mysql CLI
-   sqlcmd (SQL Server)

With SnowSQL, engineers can:

-   run SQL queries
-   automate database deployments
-   execute scripts
-   export query results
-   integrate Snowflake with automation pipelines

------------------------------------------------------------------------

## Ques: How is SnowSQL different from the Snowflake Web UI?

**Ans:**

Snowflake Web UI: - graphical interface - used for manual query
execution - useful for analysts and exploration

SnowSQL: - command line interface - used for automation and scripting -
preferred by engineers and DevOps teams

Example:

UI → interactive queries\
SnowSQL → automated scripts and deployments

------------------------------------------------------------------------

## Ques: Why do engineers prefer SnowSQL for automation?

**Ans:**

SnowSQL is preferred because it supports:

-   script execution
-   parameterized variables
-   batch processing
-   CI/CD pipeline integration

Example automation workflow:

    CI/CD Pipeline
         ↓
    SnowSQL script executes
         ↓
    Schemas / tables deployed

This allows infrastructure and database changes to be
version-controlled.

------------------------------------------------------------------------

## Ques: What platforms support SnowSQL?

**Ans:**

SnowSQL can run on:

-   Windows
-   macOS
-   Linux

This allows developers to use it in local machines, servers, or cloud
pipelines.

------------------------------------------------------------------------

## Ques: What are typical use cases for SnowSQL?

**Ans:**

Common use cases include:

-   automated data loading scripts
-   database object deployment
-   CI/CD data pipeline automation
-   scheduled batch processing
-   exporting query results

------------------------------------------------------------------------

## Ques: Does SnowSQL require installation?

**Ans:**

Yes.

SnowSQL must be installed locally or on a server before it can be used.

Installation typically involves:

1.  downloading the SnowSQL package
2.  running the installer
3.  configuring connection settings

------------------------------------------------------------------------

## Ques: Does SnowSQL replace ETL tools?

**Ans:**

No.

SnowSQL is primarily a **command-line interface for running SQL
scripts**.

ETL tools such as:

-   Airflow
-   Informatica
-   dbt

provide orchestration, scheduling, and complex pipeline management.

SnowSQL is often used **inside those tools** to execute Snowflake
commands.

------------------------------------------------------------------------

# 🔹 2️⃣ CONFIGURATION OPTIONS

## Ques: What is a SnowSQL configuration file?

**Ans:**

The SnowSQL configuration file stores connection settings such as:

-   account identifier
-   username
-   default role
-   warehouse
-   database
-   schema

This allows users to connect without typing full credentials each time.

------------------------------------------------------------------------

## Ques: Where is the configuration file stored?

**Ans:**

Typical locations:

Linux / Mac:

    ~/.snowsql/config

Windows:

    %USERPROFILE%\.snowsql\config

This file stores connection profiles.

------------------------------------------------------------------------

## Ques: What is a connection profile?

**Ans:**

A connection profile stores a set of connection parameters.

Example profile:

    [dev]
    account = company.us-east-1
    user = data_engineer
    warehouse = dev_wh
    database = sales_db
    schema = staging
    role = developer_role

This allows connecting quickly using:

    snowsql -c dev

------------------------------------------------------------------------

## Ques: What is the ACCOUNT parameter used for?

**Ans:**

The ACCOUNT parameter identifies the Snowflake account.

Example:

    account = company.us-east-1

It tells SnowSQL which Snowflake environment to connect to.

------------------------------------------------------------------------

## Ques: What is the USER parameter?

**Ans:**

USER specifies the Snowflake user account used for authentication.

Example:

    user = data_engineer

------------------------------------------------------------------------

## Ques: What is the ROLE parameter?

**Ans:**

ROLE defines the default role activated during connection.

Example:

    role = analyst_role

This determines what privileges are available in the session.

------------------------------------------------------------------------

## Ques: What is the WAREHOUSE parameter?

**Ans:**

WAREHOUSE specifies the compute cluster used to execute queries.

Example:

    warehouse = analytics_wh

Without a warehouse, queries cannot run.

------------------------------------------------------------------------

## Ques: What are DATABASE and SCHEMA settings?

**Ans:**

These define the default objects used in the session.

Example:

    database = sales_db
    schema = analytics

Then referencing a table like:

    SELECT * FROM orders;

means:

    sales_db.analytics.orders

------------------------------------------------------------------------

## Ques: What is the difference between password authentication and key-pair authentication?

**Ans:**

Password authentication:

-   uses username and password
-   simple but less secure

Key-pair authentication:

-   uses private/public keys
-   no password stored
-   stronger security for automation

------------------------------------------------------------------------

## Ques: What is the benefit of key-pair authentication?

**Ans:**

Key-pair authentication improves security because:

-   credentials are not stored in plain text
-   automated pipelines can authenticate securely
-   passwords do not expire

It is commonly used in CI/CD systems.

------------------------------------------------------------------------

# 🔹 3️⃣ WORKING WITH SNOWSQL

## Ques: How do you open a SnowSQL session?

**Ans:**

You start SnowSQL from the terminal:

    snowsql -a account_name -u username

Or using a connection profile:

    snowsql -c dev

This opens an interactive Snowflake session.

------------------------------------------------------------------------

## Ques: How do you run a single SQL statement?

**Ans:**

After opening SnowSQL, simply type the SQL query and end with a
semicolon.

Example:

    SELECT CURRENT_VERSION();

------------------------------------------------------------------------

## Ques: How do you execute multiple statements?

**Ans:**

Multiple SQL statements can be written in a script file or executed
sequentially.

Example:

    CREATE TABLE test (id INT);
    INSERT INTO test VALUES (1);
    SELECT * FROM test;

Each statement must end with a semicolon.

------------------------------------------------------------------------

## Ques: How do you change roles from SnowSQL?

**Ans:**

Use the SQL command:

    USE ROLE analyst_role;

This switches the active role in the session.

------------------------------------------------------------------------

## Ques: How do you switch warehouses?

**Ans:**

Use:

    USE WAREHOUSE analytics_wh;

This sets the warehouse used for query execution.

------------------------------------------------------------------------

## Ques: How do you export query results to a file?

**Ans:**

SnowSQL supports exporting results to files.

Example:

    snowsql -q "SELECT * FROM sales" -o output_format=csv > results.csv

This writes query results to a CSV file.

------------------------------------------------------------------------

# 🔹 VARIABLES & SCRIPTING

## Ques: What are SnowSQL variables?

**Ans:**

Variables allow dynamic values to be used inside SQL scripts.

They are useful for:

-   environment configuration
-   parameterized scripts
-   automation pipelines

------------------------------------------------------------------------

## Ques: How do you define a variable?

**Ans:**

Example:

    !define schema_name = analytics

Then used as:

    SELECT * FROM &schema_name.orders;

------------------------------------------------------------------------

## Ques: Why use variables in scripts?

**Ans:**

Variables make scripts reusable across environments.

Example:

DEV:

    schema = dev_schema

PROD:

    schema = prod_schema

The same script works for both environments.

------------------------------------------------------------------------

# 🔹 BATCH PROCESSING

## Ques: What is batch execution?

**Ans:**

Batch execution means running **multiple SQL commands from a script
file**.

Example:

    snowsql -f deploy_schema.sql

This executes all statements in the file.

------------------------------------------------------------------------

## Ques: Why is batching useful in deployments?

**Ans:**

Batch scripts allow teams to:

-   deploy schemas
-   create tables
-   apply security grants

as part of automated deployment pipelines.

------------------------------------------------------------------------

# 🔹 SCENARIO QUESTIONS

## Ques: You need nightly table loads --- SnowSQL or UI?

**Ans:**

SnowSQL.

It allows scheduled automation using scripts executed by schedulers like
cron or Airflow.

------------------------------------------------------------------------

## Ques: Script fails halfway --- how do you recover?

**Ans:**

Check:

-   query history
-   error logs
-   transaction settings

Then restart the script from the failed step.

------------------------------------------------------------------------

# 🔹 TRICK QUESTIONS

## Ques: Does SnowSQL come preinstalled with Snowflake?

**Ans:**

No.

SnowSQL must be downloaded and installed separately.

------------------------------------------------------------------------

## Ques: Does SnowSQL bypass RBAC security?

**Ans:**

No.

SnowSQL still respects all Snowflake security controls including:

-   RBAC roles
-   privileges
-   authentication policies

------------------------------------------------------------------------

## Ques: Do SELECT queries need a warehouse?

**Ans:**

Yes.

All queries that read data require a warehouse to provide compute
resources.

------------------------------------------------------------------------

## Ques: Is SnowSQL the same as Snowpark?

**Ans:**

No.

SnowSQL is a command-line SQL client.

Snowpark is a developer framework for writing data processing code in
languages like Python and Java.
