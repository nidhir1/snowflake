# ❄️ Snowflake --- Procedures & Views



------------------------------------------------------------------------

# 🔹 1️⃣ STORED PROCEDURES --- BASICS

## Ques: What is a stored procedure in Snowflake?

Ans: A **stored procedure** is a reusable program stored inside
Snowflake that executes one or more SQL statements as a single logical
unit.

Think of it as a **function or script stored in the database** that can
perform complex workflows such as:

-   Running multiple SQL statements
-   Applying business logic
-   Performing ETL operations
-   Automating repetitive tasks

Instead of writing many SQL statements repeatedly, you encapsulate them
in a procedure and simply call it.

Example:

``` sql
CREATE OR REPLACE PROCEDURE refresh_sales()
RETURNS STRING
LANGUAGE SQL
AS
$$
BEGIN
    INSERT INTO sales_summary
    SELECT * FROM sales_raw;
    RETURN 'Sales refreshed';
END;
$$;
```

------------------------------------------------------------------------

## Ques: Why do we use stored procedures?

Ans: Stored procedures are used when queries require **complex logic
that cannot easily be written as a single SQL statement**.

Common reasons to use procedures:

-   Multi-step ETL pipelines
-   Conditional logic
-   Looping operations
-   Dynamic SQL execution
-   Data validation workflows

Benefits:

-   Reusable logic
-   Reduced duplication
-   Better governance
-   Centralized business rules

------------------------------------------------------------------------

## Ques: What kind of logic is best suited for procedures?

Ans: Procedures are best for **control flow and orchestration logic**,
such as:

-   If/Else decisions
-   Loops
-   Multi-step processing
-   Calling multiple SQL statements
-   Error handling

Example workflow inside a procedure:

1.  Load raw data
2.  Validate records
3.  Transform data
4.  Insert into final tables

------------------------------------------------------------------------

## Ques: Where do stored procedures execute?

Ans: Stored procedures execute **inside Snowflake's compute layer
(virtual warehouses)**.

The warehouse assigned to the session provides the compute resources
required to run the procedure's SQL statements.

------------------------------------------------------------------------

## Ques: What languages can procedures be written in?

Ans: Snowflake supports several languages for stored procedures:

-   SQL (Snowflake Scripting)
-   JavaScript
-   Python (via Snowpark)
-   Java / Scala (Snowpark)

Most traditional Snowflake procedures are written using **SQL scripting
or JavaScript**.

------------------------------------------------------------------------

## Ques: Difference between SQL procedures and JavaScript procedures?

Ans:

SQL Procedures: - Use Snowflake SQL scripting - Easier for SQL
developers - Better for straightforward data workflows

JavaScript Procedures: - Use JavaScript runtime - Provide more advanced
programming features - Useful for complex logic and dynamic operations

------------------------------------------------------------------------

## Ques: What is a handler function?

Ans: A **handler function** is the entry point that Snowflake executes
when the procedure is called.

In JavaScript procedures, the handler function contains the main logic
of the procedure.

Example:

    handler = 'runProcedure'

Snowflake calls this function when the procedure executes.

------------------------------------------------------------------------

## Ques: What is return type of procedure?

Ans: Stored procedures return a defined data type.

Common return types:

-   STRING
-   INTEGER
-   VARIANT
-   OBJECT

Example:

    RETURNS STRING

Procedures typically return status messages or result objects.

------------------------------------------------------------------------

## Ques: What privileges are required to create procedures?

Ans: To create a procedure, the role must have:

-   CREATE PROCEDURE privilege on schema
-   USAGE privilege on database and schema

Roles with sufficient privileges can define procedures.

------------------------------------------------------------------------

## Ques: Can stored procedures modify data?

Ans: Yes.

Stored procedures can execute:

-   INSERT
-   UPDATE
-   DELETE
-   MERGE

This allows them to perform full ETL and data transformation tasks.

------------------------------------------------------------------------

# 🔹 2️⃣ CREATING & USING PROCEDURES

## Ques: What are the main components of a procedure definition?

Ans: A procedure definition typically includes:

-   Procedure name
-   Input parameters
-   Return type
-   Language
-   Handler function
-   Procedure body

Example structure:

    CREATE PROCEDURE procedure_name(parameters)
    RETURNS datatype
    LANGUAGE SQL
    AS
    $$
    procedure body
    $$;

------------------------------------------------------------------------

## Ques: What are parameters in stored procedures?

Ans: Parameters allow values to be passed into procedures when they are
executed.

Example:

    CREATE PROCEDURE get_orders(customer_id INT)

This allows dynamic input values.

------------------------------------------------------------------------

## Ques: What is input parameter vs output parameter?

Ans:

Input Parameter: - Value passed into procedure - Used during execution

Output Parameter: - Value returned from procedure

Snowflake procedures typically return values through the **RETURN
statement**.

------------------------------------------------------------------------

## Ques: How do you declare variables in a stored procedure?

Ans: Variables can be declared within the procedure block.

Example:

    DECLARE total_orders INT;

Variables store intermediate results used during logic execution.

------------------------------------------------------------------------

## Ques: How do loops work in procedures?

Ans: Loops allow repeating operations.

Example uses:

-   Process rows one by one
-   Execute repeated SQL statements

Common loop types:

-   FOR loop
-   WHILE loop

------------------------------------------------------------------------

## Ques: How to handle conditional logic in procedures?

Ans: Conditional logic uses **IF / ELSE statements**.

Example:

    IF order_count > 100 THEN
        RETURN 'High volume';
    ELSE
        RETURN 'Normal volume';
    END IF;

This allows procedures to implement decision logic.

------------------------------------------------------------------------

## Ques: Can procedures call other procedures?

Ans: Yes.

Procedures can call other procedures using the CALL command.

This allows modular design and reusable logic.

------------------------------------------------------------------------

## Ques: Can procedures execute dynamic SQL?

Ans: Yes.

Dynamic SQL allows building SQL statements during execution.

Example use cases:

-   Dynamic table names
-   Schema-driven transformations
-   Metadata-driven pipelines

------------------------------------------------------------------------

## Ques: What is EXECUTE AS OWNER vs EXECUTE AS CALLER?

Ans:

EXECUTE AS CALLER: - Procedure runs using privileges of the user calling
it.

EXECUTE AS OWNER: - Procedure runs using privileges of the procedure
owner.

EXECUTE AS OWNER allows controlled privilege escalation.

------------------------------------------------------------------------

## Ques: How are transactions handled inside procedures?

Ans: Snowflake procedures support transaction control.

Common statements:

-   BEGIN
-   COMMIT
-   ROLLBACK

This allows grouping multiple SQL statements into a single transaction.

------------------------------------------------------------------------

# 🔹 3️⃣ USING CALL COMMAND

## Ques: What is CALL command used for?

Ans: The **CALL command executes a stored procedure**.

Example:

    CALL refresh_sales();

This triggers execution of the stored procedure logic.

------------------------------------------------------------------------

## Ques: What is the syntax of CALL?

Ans:

    CALL procedure_name(arguments);

Example:

    CALL get_orders(1001);

------------------------------------------------------------------------

## Ques: How are arguments passed to CALL?

Ans: Arguments are passed inside parentheses in the order defined in the
procedure.

Example:

    CALL calculate_discount(500);

------------------------------------------------------------------------

## Ques: What happens if incorrect parameter type is passed?

Ans: Snowflake throws a **type mismatch error** and the procedure fails.

Proper parameter validation is recommended.

------------------------------------------------------------------------

## Ques: Can CALL be executed from UI, SnowSQL, or tools?

Ans: Yes.

CALL statements can be executed from:

-   Snowflake Web UI
-   SnowSQL CLI
-   Python connectors
-   BI tools
-   Data pipelines

------------------------------------------------------------------------

## Ques: What happens when procedure throws error?

Ans: If an error occurs:

-   Execution stops
-   Transaction may rollback
-   Error message is returned to the caller

Proper error handling helps prevent pipeline failures.

------------------------------------------------------------------------

## Ques: How can we capture results from CALL?

Ans: The result returned by the procedure can be captured as the output
of the CALL command.

Example:

    CALL refresh_sales();

Result message appears in the result panel.

------------------------------------------------------------------------

## Ques: Can we log messages inside procedures?

Ans: Yes.

Procedures can log messages using RETURN statements or writing to
logging tables.

This helps debugging pipelines.

------------------------------------------------------------------------

## Ques: How do we debug failing CALL executions?

Ans: Debugging methods include:

-   Reviewing error messages
-   Checking query history
-   Logging intermediate results
-   Testing procedure steps individually

------------------------------------------------------------------------

# 🔹 4️⃣ VIEWS --- BASICS

## Ques: What is a view?

Ans: A **view is a virtual table defined by a SQL query**.

Instead of storing data physically, a view stores the query definition.

When users query the view, Snowflake executes the underlying query
dynamically.

------------------------------------------------------------------------

## Ques: Why are views used instead of tables?

Ans: Views provide:

-   Abstraction layer
-   Simplified queries
-   Data security
-   Controlled access

Example use case:

Expose curated data to analysts without exposing raw tables.

------------------------------------------------------------------------

## Ques: What is logical vs physical storage?

Ans:

Logical storage: - Query definition only - No data stored

Physical storage: - Actual table data stored

Views are logical objects.

------------------------------------------------------------------------

## Ques: What is a regular (non-materialized) view?

Ans: A regular view simply stores the SQL query definition.

Each time it is queried, Snowflake executes the underlying SQL.

------------------------------------------------------------------------

## Ques: Do views store data physically?

Ans: No.

Views store only the **query definition**, not the actual data.

------------------------------------------------------------------------

## Ques: What happens when the base table changes?

Ans: Views automatically reflect the latest data because they execute
the base query dynamically.

------------------------------------------------------------------------

## Ques: Can views be updated?

Ans: In some cases yes, but usually views are used for **read-only
access**.

Updates depend on complexity of the view query.

------------------------------------------------------------------------

## Ques: What is a secure view?

Ans: A secure view hides the underlying query definition and prevents
data leakage.

It is commonly used for:

-   Data sharing
-   Protecting sensitive logic
-   Enforcing governance

------------------------------------------------------------------------

## Ques: Why are secure views important for data sharing?

Ans: Secure views allow organizations to share curated datasets without
exposing underlying table structures or logic.

------------------------------------------------------------------------

## Ques: What permissions are required for views?

Ans: Users need:

-   USAGE on database and schema
-   SELECT privilege on the view

------------------------------------------------------------------------

# 🔹 6️⃣ SYSTEM PREDEFINED VIEWS

## Ques: What are system views in Snowflake?

Ans: System views provide metadata and monitoring information about
Snowflake objects and activity.

They allow administrators to monitor:

-   query history
-   warehouse usage
-   storage usage

------------------------------------------------------------------------

## Ques: What are INFORMATION_SCHEMA views?

Ans: INFORMATION_SCHEMA contains metadata about objects in the current
database.

Examples:

-   tables
-   views
-   columns
-   procedures

------------------------------------------------------------------------

## Ques: What are ACCOUNT_USAGE views?

Ans: ACCOUNT_USAGE views provide account-wide monitoring data such as:

-   query history
-   warehouse activity
-   login history

These are commonly used for governance and monitoring.

------------------------------------------------------------------------

# 🔹 8️⃣ TRICK / MISCONCEPTIONS

## Ques: Are stored procedures required to run SQL in Snowflake?

Ans: No.

Users can run SQL queries directly without procedures.

Procedures are used only when complex logic or automation is required.

------------------------------------------------------------------------

## Ques: Does a view physically store query results?

Ans: No.

Views store only the SQL definition.

------------------------------------------------------------------------

## Ques: Do views automatically improve performance?

Ans: Not necessarily.

Views simplify queries but performance depends on the underlying SQL
execution.

------------------------------------------------------------------------

## Ques: If you drop a table, does the view data still exist?

Ans: No.

Views depend on the base table.

If the table is dropped, queries on the view will fail.

------------------------------------------------------------------------
