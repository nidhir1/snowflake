❄️ SnowSQL — Concepts, Installation, Automation & Scripting

(Complete Q&A — Medium-Detail, Teaching Style)

🔹 1️⃣ SNOWSQL CONCEPTS & CLIENT INSTALLATION
A. BASICS — WHAT IS SNOWSQL?
✅ What is SnowSQL?

SnowSQL is Snowflake’s command-line client.
It allows engineers to connect to Snowflake and run SQL and scripts through the terminal.

Think of it as:

“Snowflake Web UI — but fully scriptable and automation-friendly.”

✅ How is SnowSQL different from the Web UI?
Web UI	SnowSQL
Browser-based	Command-line
Click + explore	Script + automate
Good for learning	Ideal for engineering work
Manual workflows	CI/CD pipelines

SnowSQL is preferred when work must be repeatable, auditable, and automated.

✅ Why do engineers prefer SnowSQL for automation?

Because it supports:

reusable scripts

parameterization

batch execution

integration with DevOps tools

scheduled pipelines

Anything you automate → SnowSQL fits.

✅ What platforms support SnowSQL?

SnowSQL runs on:

Windows

macOS

Linux

✅ What are typical SnowSQL use cases?

✔ Data loading and batch jobs
✔ CI/CD deployments
✔ Schema & object changes
✔ ETL workflows
✔ Admin + audit work

✅ Does SnowSQL require installation?

Yes — it is installed separately from the Web UI.

You download it from Snowflake and configure it locally.

❌ Does SnowSQL replace ETL tools?

No.

It runs SQL.
ETL tools orchestrate workflows across systems.

SnowSQL is usually part of the pipeline — not the pipeline itself.

B. CONFIGURATION OPTIONS
✅ What is a SnowSQL configuration file?

A config file stores:

credentials

connection profiles

default role/warehouse/database

So you don’t type them every time.

✅ Where is the config file stored?

Common locations:

Windows
C:\Users\<user>\.snowsql\config

Linux/macOS
~/.snowsql/config

✅ What is a connection profile?

A named shortcut for connecting.

Example:

[prod]
account_name=abc123
user=JOHN
role=SYSADMIN
warehouse=WH_XL


Then connect using:

snowsql -c prod

✅ ACCOUNT parameter?

Identifies which Snowflake account you connect to.

✅ USER parameter?

The Snowflake username.

✅ ROLE parameter?

Determines what permissions apply when you connect.

✅ WAREHOUSE parameter?

Defines the compute engine used for execution.

✅ DATABASE & SCHEMA settings?

Default objects SnowSQL connects to.

✅ How do we configure multiple environments?

Create profiles like:

dev

test

prod

Then switch using profile names instead of rewriting credentials.

✅ Password vs Key-pair authentication?

Key-pair authentication uses:

private key on your machine

public key stored in Snowflake

More secure than passwords.

🎯 Benefit of key-pair authentication

✔ No password exposure
✔ Safer automation
✔ Works great with CI/CD tools

C. ACCOUNT AUTHORIZATION
✅ What information is required to connect?

You need:

account name

username

authentication method

region (if needed)

role (optional but recommended)

✅ What is account locator?

Unique identifier for Snowflake account.

✅ What is region?

Specifies where your Snowflake deployment runs geographically.

✅ Organization name?

Used in some enterprise setups. Optional.

✅ Can SnowSQL work with MFA?

Yes — MFA works depending on authentication type.

❌ What happens if credentials are wrong?

Connection fails with authentication error.

✅ How to verify successful connection?

You’ll see the SnowSQL prompt:

0n> 


And you can run:

select current_account(), current_user();

🔹 2️⃣ WORKING WITH SNOWSQL
A. RUNNING COMMANDS
✅ Opening a SnowSQL session
snowsql -c dev

✅ Running single SQL statement
select current_role();

✅ Multiple statements

Separate using semicolons.

✅ What is auto-commit?

SnowSQL commits every statement automatically — unless disabled.

✅ Changing roles
use role SYSADMIN;

✅ Switching warehouses
use warehouse WH_MEDIUM;

✅ Listing objects

Examples:

show tables;
show schemas;
show roles;
show users;

✅ Output formats

SnowSQL supports:

table

CSV

JSON

✅ Export results to file
!output file='result.csv'
select * from customers;
!output off

✅ View query history
select * from table(information_schema.query_history());

B. DDL / DML / SELECT

Yes — SnowSQL supports full SQL:

CREATE / ALTER / DROP

INSERT / UPDATE / DELETE

SELECT

Transactions

If query fails mid-script → only committed changes persist.

C. VARIABLES & SCRIPTING
✅ What are SnowSQL variables?

Values stored temporarily and reused inside scripts.

Define variable
!set var_date='2025-01-01'


Use in query:

select * from orders where order_date = $var_date;

Why variables?

They enable:

parameterized deployments

reusable scripts

environment switching

D. BATCH PROCESSING
✅ What is batch execution?

Executing SQL files instead of typing manually.

snowsql -f migration.sql

Stop on error vs continue

Controlled using configuration flags.

Useful when running deployment pipelines.

E. SQL SYNTAX FORMAT

✔ Semicolon ends statements
✔ Case insensitive
✔ Comments supported:

-- comment
/* comment */


Readable formatting improves maintenance.

🔹 3️⃣ SCENARIOS — PRACTICAL THINKING
⭐ Nightly load script?

Use SnowSQL, not UI.

⭐ CI/CD deployment?

SnowSQL runs migration scripts safely.

⭐ Switch dev/prod quickly?

Use profiles, NOT manual edits.

⭐ Script failed halfway — recovery?

Check logs + rerun only failed parts.

⭐ Need audit logs?

Enable output logging and query history.

🔹 4️⃣ MISCONCEPTIONS — FIXED

❌ SnowSQL comes preinstalled
✔ Must be downloaded

❌ SnowSQL runs faster
✔ Performance same — compute determines speed

❌ SnowSQL bypasses security
✔ RBAC always applies

❌ Passwords stored plain text
✔ Use environment variables or key-pairs

❌ Errors auto-rollback
✔ Only inside explicit transaction

❌ Variables persist forever
✔ Session only

❌ SnowSQL edits stored procedures directly
✔ It executes ALTER statements, not IDE

❌ Can access multiple accounts at once
✔ Only one per session

❌ SELECT needs no warehouse
✔ Queries still require compute

❌ SnowSQL = Snowpark
✔ Snowpark = programming API, different purpose