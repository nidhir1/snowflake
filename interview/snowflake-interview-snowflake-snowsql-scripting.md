❄️ SnowSQL — Questions Only (Concepts, Installation, Automation & Scripting)
🔹 1️⃣ SNOWSQL CONCEPTS & CLIENT INSTALLATION
A. BASICS — WHAT IS SNOWSQL?

What is SnowSQL?

How is SnowSQL different from the Snowflake Web UI?

Why do engineers prefer SnowSQL for automation?

What platforms support SnowSQL (Windows / Mac / Linux)?

What are typical use cases for SnowSQL?

Does SnowSQL require installation?

Does SnowSQL replace ETL tools?

B. CONFIGURATION OPTIONS

What is a SnowSQL configuration file?

Where is the configuration file stored?

What is a connection profile?

What is the ACCOUNT parameter used for?

What is the USER parameter?

What is the ROLE parameter?

What is the WAREHOUSE parameter?

What are DATABASE and SCHEMA settings?

How can multiple environments (dev / prod) be configured?

What is the difference between password auth and key-pair authentication?

What is the benefit of key-pair authentication?

C. ACCOUNT AUTHORIZATION

What information is required to connect to Snowflake?

What is an account locator?

What is a region?

What is an organization name (and when is it used)?

Can SnowSQL work with MFA?

What happens if credentials are wrong?

How do you verify successful connection?

🔹 2️⃣ WORKING WITH SNOWSQL
A. RUNNING COMMANDS

How do you open a SnowSQL session?

How do you run a single SQL statement?

How do you execute multiple statements?

What is auto-commit mode in SnowSQL?

How do you change roles from SnowSQL?

How do you switch warehouses?

How do you list objects (tables, schemas, roles, users)?

How do you change output format (table, CSV, JSON)?

How do you export query results to a file?

How do you check query history in SnowSQL?

B. DDL / DML / SELECT

Can SnowSQL run DDL commands?

Can SnowSQL run INSERT / UPDATE / DELETE?

How do you run SELECT queries?

Can SnowSQL run transactions (BEGIN / COMMIT)?

What permissions are required for running commands?

What happens if a query fails mid-script?

How do you safely re-run commands?

C. VARIABLES & SCRIPTING

What are SnowSQL variables?

How do you define a variable?

What is the difference between local variables and substitution variables?

How do you reference variable values inside SQL?

Why use variables in scripts?

How do you pass variables from the command line?

How do you parameterize environment-specific values?

Can variables be used in file paths and object names?

D. BATCH PROCESSING

What is batch execution?

How do you run SQL from an external file?

How do you call multiple scripts sequentially?

What happens on first failure in a batch?

What setting controls stop-on-error vs continue?

Why is batching useful in deployments?

How do you log batch outputs?

E. SQL QUERY SYNTAX FORMAT

What is the semicolon used for in scripts?

Does Snowflake require uppercase SQL?

Can multiple statements be written on the same line?

What is the comment syntax in SnowSQL?

What are best practices for script formatting?

🔹 3️⃣ SCENARIO-BASED QUESTIONS

You need nightly table loads — SnowSQL or UI?

The team needs automated deployment — how does SnowSQL help?

You must switch quickly between dev and prod — how?

Script fails halfway — how do you recover?

You need audit logs for script execution — what do you use?

You need dynamic date in script — how do you handle it?

You want query results exported to CSV automatically — how?

Query works in UI but fails in SnowSQL — where to troubleshoot?

🔹 4️⃣ TRICK / MISCONCEPTION QUESTIONS

Does SnowSQL come preinstalled with Snowflake?

Does SnowSQL run queries faster than UI?

Does SnowSQL bypass RBAC security?

Are passwords stored in plain text?

Do scripts automatically roll back on errors?

Do variables persist across sessions?

Can SnowSQL edit stored procedures directly?

Can SnowSQL access multiple accounts at once?

Do SELECT queries need a warehouse?

Is SnowSQL the same as Snowpark?