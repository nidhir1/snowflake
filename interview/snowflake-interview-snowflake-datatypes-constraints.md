# Snowflake — Constraints & Data Validation  
## Question Bank (Questions Only)

### 1️⃣ CONSTRAINTS & DATA VALIDATION — BASICS

What are constraints in Snowflake?
Why do databases use constraints?
What is data validation?
Why is data validation important in data warehouses?
What happens if invalid data enters analytics systems?
Does Snowflake strictly enforce constraints?
What does “informational constraints” mean in Snowflake?
Why did Snowflake choose not to enforce constraints physically?
When should we still define constraints?

### 2️⃣ NULL & NOT NULL PROPERTIES

What is NULL?
Difference between NULL and empty string.
Can a NOT NULL column accept NULL?
What happens if we insert NULL into NOT NULL?
How does Snowflake treat blank strings?
Can numeric fields be NULL?
Can date fields be NULL?
Why is handling NULL important in analytics?
What are common mistakes developers make with NULL?
How to test for NULL in Snowflake (IS NULL / IS NOT NULL)?

### 3️⃣ UNIQUE, PRIMARY & FOREIGN KEYS

What is a UNIQUE constraint?
Can a UNIQUE column contain NULLs?
What is a PRIMARY KEY?
Difference between PRIMARY KEY and UNIQUE.
Can a table have multiple UNIQUE constraints?
Can a table have multiple PRIMARY KEYS?
What is a FOREIGN KEY?
What is referential integrity?
Does Snowflake enforce referential integrity?
Why define PRIMARY/FOREIGN keys if not enforced?
Do BI tools benefit from defined constraints?
Should developers rely on Snowflake constraints for validation?

### 4️⃣ NUMERIC DATA TYPES

What numeric types exist in Snowflake?
What is NUMBER / DECIMAL / NUMERIC?
What are precision and scale?
What happens if a number exceeds precision?
What is INTEGER / BIGINT / SMALLINT?
What is FLOAT / DOUBLE?
When should FLOAT be avoided?
When is NUMBER preferred?
How is rounding handled?
Are numeric comparisons different for floats vs decimals?

### 5️⃣ STRING & BINARY TYPES

What is VARCHAR?
What are limits on string length?
What is CHARACTER vs CHAR?
What is TEXT?
What is BINARY data type?
When is BINARY useful?
Can binary store images or files?
How does Snowflake compress string data?
What happens if text exceeds defined length?

### 6️⃣ BOOLEAN TYPE

What is BOOLEAN?
What values does BOOLEAN accept?
How does Snowflake treat 1/0 or ‘true/false’ text?
What happens if string is inserted into BOOLEAN?
How BOOLEAN helps in filtering and flags?

### 7️⃣ DATE & TIME TYPES

What is DATE type?
What is TIME type?
What is TIMESTAMP?
Difference between TIMESTAMP_LTZ, NTZ, TZ.
Why do time zones matter?
How does Snowflake store timezone internally?
What is DATE_TRUNC?
What is DATEADD / DATEDIFF?
What happens when timezone conversion is wrong?
Why do ETL pipelines often fail because of timestamps?

### 8️⃣ SEMI-STRUCTURED DATA TYPES

What is semi-structured data?
What are common semi-structured formats?
What is VARIANT?
What is OBJECT?
What is ARRAY?
Why doesn’t VARIANT need a fixed schema?
How to access fields inside VARIANT?
What is LATERAL FLATTEN?
Why JSON fits well in Snowflake?
Performance tradeoffs using VARIANT.

### 9️⃣ GEOSPATIAL & SPECIAL TYPES

What is GEOGRAPHY data type?
What formats can GEOGRAPHY store (WKT, GeoJSON)?
What is ST_DISTANCE?
What use cases require geospatial analytics?
Can VARIANT store geospatial data?
Why use native GEO instead of plain strings?

### 🔟 SCENARIO-BASED QUESTIONS

Customer emails should never be NULL — how ensure?
Orders table must always link to customer — what constraint concept applies?
Numbers round incorrectly — which type should we use?
Financial calculations showing precision issues — FLOAT or NUMBER?
JSON logs contain dynamic fields — how to store?
ETL inserts empty strings instead of NULL — impact?
Need to analyze delivery routes — which data type?
Team wants constraints enforced — where should they enforce instead?
Analysts need to understand data relationships — how do constraints help?

### 1️⃣1️⃣ TRICK / MISCONCEPTION QUESTIONS

Does PRIMARY KEY physically enforce uniqueness in Snowflake?
Does Snowflake reject invalid foreign key relationships?
Does defining NOT NULL guarantee clean data always?
Does VARIANT mean “schema-less forever”?
Does VARIANT automatically optimize storage?
Can you store entire files inside VARCHAR?
Are all NULLs treated the same across systems?
Does GEOGRAPHY replace specialized GIS systems?
Does using JSON mean we don't need modeling?
Are constraints unnecessary in data warehouses?
