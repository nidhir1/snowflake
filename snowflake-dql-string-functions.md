![Snowflake Tinitiate Image](snowflake_tinitiate.png)

# Snowflake
&copy; TINITIATE.COM

##### [Back To Context](./README.md)

# DQL - String Functions 
- String functions in Snowflake are used to manipulate text data.
- They provide a variety of operations to perform tasks such as searching for substrings, modifying case, trimming whitespace, and extracting parts of strings.

## String functions in Snowflake: 
### Length Function (LENGTH): 
- Returns the number of characters in a string.
```sql
-- Retrieve the length of the 'ename' column in the employees table
SELECT LENGTH(ename) FROM employees.emp;
```

### Substring Function (SUBSTR or SUBSTRING): 
- Extracts a substring from a string based on a specified start position and optional length.
```sql
-- Extract the first three characters from
-- the 'ename' column in the employees table
SELECT SUBSTR(ename, 1, 3) FROM employees.emp;
```
### Concatenation Operator (||): 
- Concatenates two or more strings together.
```sql
-- Concatenate the 'ename' and 'job' columns in the employees table
SELECT ename || ' - ' || job FROM employees.emp;
```
### Lower Function (LOWER): 
- Converts a string to lowercase.
```sql
-- Convert the 'dname' column in the departments table to lowercase
SELECT LOWER(dname) FROM employees.dept;
```
### Upper Function (UPPER): 
- Converts a string to uppercase.
```sql
-- Convert the 'job' column in the employees table to uppercase
SELECT UPPER(job) FROM employees.emp;
```
### Trim Function (TRIM): 
- Removes leading and trailing whitespace from a string.
```sql
-- Remove leading and trailing whitespace from the
-- 'loc' column in the departments table
SELECT TRIM(loc) FROM employees.dept;

-- It removes the whitespaces from front and back of a string text
SELECT TRIM('    tinitiate.com     ') AS trimmed_string;
```
### Ltrim Function (LTRIM): 
- Removes leading whitespace from a string.
```sql
-- Remove leading whitespace from the 'loc' column in the departments table
SELECT LTRIM(loc) FROM employees.dept;
-- It removes the whitespaces from the front of a string text
SELECT LTRIM('    tinitiate.com     ') AS trimmed_lstring;
```
### Rtrim Function (RTRIM): 
- Removes trailing whitespace from a string.
```sql
-- Remove trailing whitespace from the 'loc' column in the departments table
SELECT RTRIM(loc) FROM employees.dept;
-- It removes the whitespaces from the back of a string text
SELECT RTRIM('    tinitiate.com     ') AS trimmed_rstring;
```
### Position Function (POSITION): 
- Returns the position of a substring within a string.
```sql
-- Find the position of 'e' in the 'ename' column of the employees table
SELECT POSITION('e' IN ename) FROM employees.emp;
```
### Left Function (LEFT): 
- Extracts a specified number of characters from the left side of a string.
```sql
-- Extract the first three characters from the
-- 'dname' column in the departments table
SELECT LEFT(dname, 3) FROM employees.dept;
```
### Right Function (RIGHT): 
- Extracts a specified number of characters from the right side of a string.
```sql
-- Extract the last three characters from the
-- 'loc' column in the departments table
SELECT RIGHT(loc, 3) FROM employees.dept;
```
### Reverse Function (REVERSE): 
- Reverses the characters in a string.
```sql
-- Reverse the 'ename' column in the employees table
SELECT REVERSE(ename) FROM employees.emp;
```

### Replace Function (REPLACE): 
- Replaces occurrences of a substring within a string with another substring.
```sql
-- Replace 'a' with 'X' in the 'dname' column of the departments table
SELECT REPLACE(dname, 'a', 'X') FROM employees.dept;
```
### Case Statement (CASE): 
- Evaluates conditions and returns a value when the first condition is met.
```sql
-- Decode the value 'clerk' to 'Clerk', 'salesman' to 'Salesman' in the 'job' column
SELECT CASE job
           WHEN 'clerk' THEN 'Clerk'
           WHEN 'salesman' THEN 'Salesman'
           ELSE 'Unknown'
       END AS job_title
FROM employees.emp;
```
### NVL Function (NVL): 
- Replaces null values with a specified replacement value.
```sql
-- Replace nulls in the 'commission' column with 0
SELECT ename, NVL(commission, 0) AS comm
FROM employees.emp;
```
### Coalesce Function (COALESCE): 
- Replaces NULL with a specified value or returns the first non-NULL value in a list of arguments.
```sql
-- Return the first non-null value among 'comm', 'bonus', and 0
SELECT ename, COALESCE(commission, mgr, 0) AS compensation
FROM employees.emp;
```
### SPLIT Function (SPLIT):
- The `SPLIT` function is used to split a string into an array of substrings based on a specified delimiter.
```sql
-- Split a string by a comma
SELECT SPLIT('apple,banana,cherry', ',') AS fruits;
-- Result: ['apple', 'banana', 'cherry']

-- Split a string by a space
SELECT SPLIT('this is a test', ' ') AS words;
-- Result: ['this', 'is', 'a', 'test']

-- Split a string by a dash
SELECT SPLIT('2024-07-30', '-') AS date_parts;
-- Result: ['2024', '07', '30']
```
### INITCAP Function (INITCAP) 
- The `INITCAP` function capitalizes the first letter of each word in a string, while converting all other letters to lowercase.
```sql
-- Capitalize the first letter of each word
SELECT INITCAP('snowflake database') AS capitalized;
-- Result: 'Snowflake Database'

-- Capitalize a single word
SELECT INITCAP('hello') AS capitalized;
-- Result: 'Hello'

-- Mixed case input
SELECT INITCAP('hELLo WoRLd') AS capitalized;
-- Result: 'Hello World'
```

##### [Back To Context](./README.md)
***
| &copy; TINITIATE.COM |
|----------------------|