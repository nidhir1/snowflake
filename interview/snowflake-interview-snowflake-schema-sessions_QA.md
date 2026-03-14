# ❄️ Snowflake --- Schemas, Session Context and Data Loading


# 1️⃣ SCHEMAS IN SNOWFLAKE

## Ques: What is a schema in Snowflake?

**Ans:**\
A **schema** is a logical container inside a Snowflake database used to
organize database objects.

Think of a schema like a **folder inside a database**.

Example hierarchy:

Account\
→ Database\
→ Schema\
→ Tables / Views / Procedures

Example:

Database: SALES_DB\
Schema: ANALYTICS\
Tables: ORDERS, CUSTOMERS

Schemas help organize objects and manage permissions efficiently.

------------------------------------------------------------------------

## Ques: How is a schema different from a database?

**Ans:**

Database: - Top-level container for data objects. - Can contain multiple
schemas.

Schema: - A subdivision of a database. - Contains tables, views, stages,
and procedures.

Example:

SALES_DB\
├── RAW_SCHEMA\
├── STAGING_SCHEMA\
└── ANALYTICS_SCHEMA

------------------------------------------------------------------------

## Ques: Why do we use schemas?

**Ans:**

Schemas help organize and control access to data.

Benefits:

1.  Organize objects logically
2.  Separate development environments
3.  Apply access control policies
4.  Simplify data governance

Example structure:

RAW schema → raw ingestion data\
STAGING schema → cleaned intermediate data\
CURATED schema → analytics-ready tables

------------------------------------------------------------------------

## Ques: Can a database have multiple schemas?

**Ans:**

Yes.

A database can contain **many schemas**.

Example:

    SALES_DB
       ├ RAW
       ├ STAGING
       ├ ANALYTICS
       └ ML_FEATURES

Each schema can contain its own tables and views.

------------------------------------------------------------------------

## Ques: What kinds of objects can exist inside schemas?

**Ans:**

Schemas can contain many object types including:

-   Tables
-   Views
-   Materialized views
-   Stages
-   File formats
-   Stored procedures
-   Functions
-   Sequences
-   Tasks

Schemas act as the **organizational layer for all database objects**.

------------------------------------------------------------------------

## Ques: What is the default schema when none is selected?

**Ans:**

If a schema is not explicitly selected, Snowflake uses the schema
defined in the **session context**.

Typically this is:

PUBLIC schema

Example:

    DATABASE.PUBLIC.TABLE_NAME

------------------------------------------------------------------------

## Ques: Can two schemas have tables with the same name?

**Ans:**

Yes.

Two tables can have the same name as long as they are in different
schemas.

Example:

    RAW.orders
    ANALYTICS.orders

Both tables are valid because they belong to different schemas.

------------------------------------------------------------------------

## Ques: What is schema namespace?

**Ans:**

Namespace refers to the **unique path that identifies an object**.

Object path format:

    DATABASE.SCHEMA.OBJECT

Example:

    SALES_DB.ANALYTICS.ORDERS

This full path uniquely identifies the table.

------------------------------------------------------------------------

## Ques: Who can create schemas?

**Ans:**

Users with the appropriate privileges can create schemas.

Typically:

-   ACCOUNTADMIN
-   SYSADMIN
-   Roles with CREATE SCHEMA privilege

------------------------------------------------------------------------

## Ques: What privilege is required to create a schema?

**Ans:**

The role must have:

    CREATE SCHEMA

privilege on the database.

Example:

    GRANT CREATE SCHEMA ON DATABASE SALES_DB TO ROLE data_engineer;

------------------------------------------------------------------------

# 2️⃣ SESSION CONTEXT

## Ques: What is a Snowflake session?

**Ans:**

A **session** represents an active connection between a client and
Snowflake.

Sessions are created when:

-   a user logs in through UI
-   a script connects via driver
-   an application connects through API

The session stores context such as:

-   active role
-   active warehouse
-   active database
-   active schema

------------------------------------------------------------------------

## Ques: How is a session created?

**Ans:**

A session is created whenever a connection is established.

Examples:

-   Logging into Snowflake Web UI
-   Running SnowSQL
-   Connecting using Python connector
-   BI tool connecting via JDBC/ODBC

Each connection creates a separate session.

------------------------------------------------------------------------

## Ques: What happens when a session closes?

**Ans:**

When a session ends:

-   session variables are cleared
-   temporary tables are dropped
-   context information is lost

A new session must be created for future queries.

------------------------------------------------------------------------

## Ques: What is session context in Snowflake?

**Ans:**

Session context defines **which objects and resources Snowflake should
use by default**.

Context includes:

-   role
-   warehouse
-   database
-   schema

This determines how queries are executed.

------------------------------------------------------------------------

## Ques: What is current role?

**Ans:**

The **current role** determines which privileges are active.

Example:

    USE ROLE analyst_role;

Now queries run with privileges granted to that role.

------------------------------------------------------------------------

## Ques: What is current warehouse?

**Ans:**

The **current warehouse** provides compute resources for queries.

Example:

    USE WAREHOUSE analytics_wh;

All queries run using this warehouse.

------------------------------------------------------------------------

## Ques: What is current database and schema?

**Ans:**

These determine where Snowflake looks for objects when names are not
fully qualified.

Example:

    USE DATABASE sales_db;
    USE SCHEMA analytics;

Now referencing `orders` means:

    sales_db.analytics.orders

------------------------------------------------------------------------

## Ques: What happens if required context isn't set?

**Ans:**

Snowflake may return errors such as:

    Object does not exist

because it cannot locate the object in the current schema.

------------------------------------------------------------------------

## Ques: What are fully qualified names?

**Ans:**

Fully qualified names specify the complete object path.

Format:

    DATABASE.SCHEMA.OBJECT

Example:

    sales_db.analytics.orders

Using full names avoids ambiguity.

------------------------------------------------------------------------

# 3️⃣ DATA LOADING

## Ques: What are the main ways to load data into Snowflake?

**Ans:**

Common data loading methods include:

1.  Web UI (GUI upload)
2.  COPY INTO command
3.  SnowSQL CLI
4.  External data pipelines
5.  Snowpipe for streaming ingestion

Each method ultimately uses **Snowflake stages and COPY commands**.

------------------------------------------------------------------------

## Ques: What is loading via the Web UI?

**Ans:**

The Snowflake Web UI allows users to upload files directly from their
local machine.

Steps:

1.  Choose table
2.  Select "Load Data"
3.  Upload file
4.  Configure file format
5.  Execute load

Snowflake internally performs:

-   file staging
-   COPY INTO operation

------------------------------------------------------------------------

## Ques: What is loading using COPY INTO?

**Ans:**

COPY INTO is Snowflake's primary command for bulk loading data.

Example:

    COPY INTO sales_table
    FROM @sales_stage
    FILE_FORMAT = (TYPE = CSV);

This command loads files from a stage into a table.

------------------------------------------------------------------------

## Ques: Why should we avoid inserting data row by row?

**Ans:**

Row-by-row inserts are inefficient because Snowflake is optimized for
**bulk data processing**.

Bulk loading:

-   improves performance
-   reduces compute cost
-   allows parallel ingestion

------------------------------------------------------------------------

## Ques: What file formats are commonly loaded into Snowflake?

**Ans:**

Snowflake supports many file formats including:

-   CSV
-   JSON
-   PARQUET
-   AVRO
-   ORC
-   XML

Structured and semi-structured data can both be loaded.

------------------------------------------------------------------------

# 4️⃣ SCENARIO-BASED QUESTIONS

## Ques: You created a table but cannot find it --- what should you check?

**Ans:**

Check:

1.  Current database
2.  Current schema
3.  Role privileges
4.  Query history

Often the table exists in a different schema.

------------------------------------------------------------------------

## Ques: Two tables exist with the same name --- how is that possible?

**Ans:**

They are likely located in different schemas.

Example:

    raw.orders
    analytics.orders

------------------------------------------------------------------------

## Ques: A file didn't load --- how do you check what happened?

**Ans:**

Check:

-   query history
-   load history
-   error messages from COPY INTO command

These reveal why the load failed.

------------------------------------------------------------------------

# 5️⃣ TRICK / MISCONCEPTION QUESTIONS

## Ques: Does schema mean the same as database?

**Ans:**

No.

A schema is a **subdivision inside a database**.

------------------------------------------------------------------------

## Ques: Does transient schema mean temporary?

**Ans:**

No.

Transient schemas are permanent objects but have **reduced data
protection features**.

------------------------------------------------------------------------

## Ques: Does GUI loading avoid stages?

**Ans:**

No.

Even GUI loading uses a stage internally before running COPY INTO.

------------------------------------------------------------------------

## Ques: Does Time Travel work for every schema type?

**Ans:**

No.

Transient schemas have reduced Time Travel retention and no Fail-safe
protection.
