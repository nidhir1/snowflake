![Snowflake Tinitiate Image](snowflake_tinitiate.png)

# Snowflake
&copy; TINITIATE.COM

##### [Back To Context](./README.md)

# DQL - Date Functions 
- Date functions in Snowflake are used to perform various operations on date and timestamp data stored in the database.
- They allow for manipulation, extraction, formatting, and calculation of dates and times.

## Date functions in Snowflake: 
### Current Date and Time (CURRENT_TIMESTAMP): 
- Returns the current date and time.
```sql
-- Retrieve the current date and time
SELECT CURRENT_TIMESTAMP();
```

### Date Part Function (DATE_PART): 
- Returns a specific component (such as year, month, day) from a date or timestamp.
```sql
-- Retrieve the day of the month from the 'hiredate' column
SELECT DATE_PART('DAY', hiredate) FROM employees.emp;
```
### Extract Function (EXTRACT):
- Extracts a specific component (such as year, month, day) from a date or timestamp.
- In Snowflake, both DATE_PART and EXTRACT functions are used to extract specific parts from date, time, or timestamp values. They are essentially synonyms and can be used interchangeably. 
```sql
-- Extract the year from a date
SELECT EXTRACT(year FROM '2024-05-15'::DATE) AS year;
-- Result: 2024

-- Extract the month from a timestamp
SELECT EXTRACT(month FROM '2024-05-15 10:30:45'::TIMESTAMP) AS month;
-- Result: 5

-- Extract the day of the week from the current date
SELECT EXTRACT(dayofweek FROM CURRENT_DATE()) AS day_of_week;
-- Result: (depending on the current date, 1 for Sunday, 2 for Monday, etc.)

-- Extract the hour from the current timestamp
SELECT EXTRACT(hour FROM CURRENT_TIMESTAMP()) AS hour;
-- Result: (the current hour)
```
### Date Difference Function (DATEDIFF): 
- Calculates the difference between two dates or timestamps.
```sql
-- Calculate the age of each employee based on their 'hiredate'
SELECT DATEDIFF(year, hiredate, CURRENT_TIMESTAMP()) FROM employees.emp;
```
### Date Addition/Subtraction (DATEADD): 
- Adds or subtracts a specified interval (such as days, months) from a date or timestamp.
```sql
-- Add a specific number of days to a date
SELECT DATEADD(day, 7, hiredate) FROM employees.emp;

-- Subtract 6 months from the 'hiredate' column in the employees table
SELECT DATEADD(month, -6, hiredate) FROM employees.emp;
```
### Date Truncation (DATE_TRUNC):
- The DATE_TRUNC function in Snowflake truncates a date, time, or timestamp to a specified precision. This is useful for aggregating data based on different time intervals.
```sql
-- Truncate date to the start of the month
SELECT DATE_TRUNC('month', '2024-05-15'::DATE) AS truncated_date;
-- Result: 2024-05-01

-- Truncate timestamp to the start of the hour
SELECT DATE_TRUNC('hour', '2024-05-15 10:30:45'::TIMESTAMP) AS truncated_timestamp;
-- Result: 2024-05-15 10:00:00

-- Truncate timestamp to the start of the quarter
SELECT DATE_TRUNC('quarter', '2024-07-01 15:45:30'::TIMESTAMP) AS truncated_quarter;
-- Result: 2024-07-01 00:00:00
```
### Date Formatting (TO_CHAR): 
- Formats a date or timestamp according to a specified format.
```sql
-- Format the 'hiredate' column in a specific date format
SELECT TO_CHAR(hiredate, 'YYYY-MM-DD') FROM employees.emp;
```
### Weekday Function (DAYOFWEEK): 
- Returns the day of the week (0 for Sunday, 1 for Monday, etc.) from a date or timestamp.
```sql
-- Retrieve the day of the week (0 for Sunday, 1 for Monday, etc.) from
-- the 'hiredate' column
SELECT DAYOFWEEK(hiredate) FROM employees.emp;
```
### Date to String (TO_VARCHAR): 
- Converts a date to a string in various formats.
```sql
-- YYYY-MM-DD:
SELECT empno, ename, TO_VARCHAR(hiredate, 'YYYY-MM-DD') AS hiredate
FROM employees.emp;

-- MM/DD/YYYY:
SELECT empno, ename, TO_VARCHAR(hiredate, 'MM/DD/YYYY') AS hiredate
FROM employees.emp;

-- DD/MM/YYYY:
SELECT empno, ename, TO_VARCHAR(hiredate, 'DD/MM/YYYY') AS hiredate
FROM employees.emp;

-- Mon DD, YYYY:
SELECT empno, ename, TO_VARCHAR(hiredate, 'Mon DD, YYYY') AS hiredate
FROM employees.emp;

-- YYYYMMDD:
SELECT empno, ename, TO_VARCHAR(hiredate, 'YYYYMMDD') AS hiredate
FROM employees.emp;

-- DD-MM-YYYY:
SELECT empno, ename, TO_VARCHAR(hiredate, 'DD-MM-YYYY') AS hiredate
FROM employees.emp;

-- YYYY/MM/DD:
SELECT empno, ename, TO_VARCHAR(hiredate, 'YYYY/MM/DD') AS hiredate
FROM employees.emp;

-- DD MMM YYYY:
SELECT empno, ename, TO_VARCHAR(hiredate, 'DD MMM YYYY') AS hiredate
FROM employees.emp;
```
### DateTime to String (TO_VARCHAR): 
- Converts a datetime to a string in various formats.
```sql
-- YYYY-MM-DD HH:MI:SS:
SELECT empno, ename, TO_VARCHAR(hiredate, 'YYYY-MM-DD HH24:MI:SS') AS hiredate
FROM employees.emp;
-- hiredate doesn't have time inserts, so time won't show up
-- Using CURRENT_TIMESTAMP()
SELECT TO_VARCHAR(CURRENT_TIMESTAMP(), 'YYYY-MM-DD HH24:MI:SS') AS datetimetostring;

-- YYYY-MM-DD HH:MI:SS:MMM:
SELECT TO_VARCHAR(CURRENT_TIMESTAMP(), 'YYYY-MM-DD HH24:MI:SS:FF3') AS datetimetostring;

-- DD Mon YYYY HH:MI:SS:MMM:
SELECT TO_VARCHAR(CURRENT_TIMESTAMP(), 'DD Mon YYYY HH24:MI:SS:FF3') AS datetimetostring;

-- Mon DD YYYY HH:MI:SS:MMMAM (or PM):
SELECT TO_VARCHAR(CURRENT_TIMESTAMP(), 'Mon DD YYYY HH12:MI:SS:FF3AM') AS datetimetostring;
```
### String to Date (TO_DATE): 
- Converts a string in various formats to a date.
```sql
-- YYYY-MM-DD:
SELECT TO_DATE('2023-04-15', 'YYYY-MM-DD') AS date;

-- MM/DD/YYYY:
SELECT TO_DATE('04/15/2023', 'MM/DD/YYYY') AS date;

-- DD.MM.YYYY:
SELECT TO_DATE('15.04.2023', 'DD.MM.YYYY') AS date;

-- Mon DD, YYYY:
SELECT TO_DATE('Apr 15, 2023', 'Mon DD, YYYY') AS date;

-- YYYYMMDD:
SELECT TO_DATE('20230415', 'YYYYMMDD') AS date;
```
### String to DateTime (TO_TIMESTAMP): 
- Converts a string in various formats to a datetime.
```sql
-- YYYY-MM-DD HH:MI:SS:
SELECT TO_TIMESTAMP('2023-04-15 13:30:45', 'YYYY-MM-DD HH24:MI:SS') AS datetime;

-- MM/DD/YYYY HH:MI:SS:
SELECT TO_TIMESTAMP('04/15/2023 13:30:45', 'MM/DD/YYYY HH24:MI:SS') AS datetime;

-- DD.MM.YYYY HH:MI:SS:
SELECT TO_TIMESTAMP('15.04.2023 13:30:45', 'DD.MM.YYYY HH24:MI:SS') AS datetime;

-- Mon DD YYYY HH:MI:SS:MMM:
SELECT TO_TIMESTAMP('Apr 15 2023 01:30:45:375', 'Mon DD YYYY HH12:MI:SS:FF3') AS datetime;

-- YYYYMMDD HH:MI:SS:
SELECT TO_TIMESTAMP('20230415 13:30:45', 'YYYYMMDD HH24:MI:SS') AS datetime;
```
### DateTime and TimeZone: 
#### Date, Timezones, UTC, and Offsets: 
- **Date and Time** : In computing, dates and times are represented using various data types. A common approach is to use a datetime data type that includes both date and time information.

- **Timezones** : Timezones are regions of the Earth that have the same standard time. Each timezone is usually offset from Coordinated Universal Time (UTC) by a certain number of hours and minutes. For example, Eastern Standard Time (EST) is UTC-5, meaning it is 5 hours behind UTC.
- **UTC** : Coordinated Universal Time (UTC) is the primary time standard by which the world regulates clocks and time. It is not adjusted for daylight saving time. UTC is often used as a reference point for converting between different timezones.
- **Offsets** : An offset is the difference in time between a specific timezone and UTC. It is usually expressed as a positive or negative number of hours and minutes. For example, UTC+2 means the timezone is 2 hours ahead of UTC, while UTC-8 means the timezone is 8 hours behind UTC.
- **Applying an Offset to a Date Datatype Column** : To apply an offset to a date datatype column in Snowflake, you can use the `TIMESTAMPADD` function to add or subtract a specific number of hours to/from the column value. For example, to add 5 hours to a datetime column named `my_datetime_column`, you can use the following query:
```sql
-- This query will return the value of my_datetime_column adjusted by 5 hours. 
SELECT TIMESTAMPADD(HOUR, 5, hiredate) AS adjusted_datetime
FROM employees.emp;
```

#### Cast a DateTime to DateTime with Timezone (in UTC, EST and IST TimeZones): 
- Create a datetime variable and cast it as a datetime with timezone data type
- Here we cast it at different timezones (UTC, EST and IST)
```sql
SELECT 
    '2024-04-17 15:30:00' AS OriginalDateTime,
    CONVERT_TIMEZONE('UTC', '2024-04-17 15:30:00') AS UTCDateTime,
    CONVERT_TIMEZONE('UTC', 'America/New_York', '2024-04-17 15:30:00') AS ESTDateTime,
    CONVERT_TIMEZONE('UTC', 'Asia/Kolkata', '2024-04-17 15:30:00') AS ISTDateTime;
```
#### Cast a DateTime Timezone to another TimeZone: 
```sql
SELECT 
    'UTCDateTime: ' || TO_CHAR('2024-04-17 15:30:00'::timestamp, 'YYYY-MM-DD HH24:MI:SS') AS UTCDateTime,
    'ESTDateTime: ' || TO_CHAR(CONVERT_TIMEZONE('UTC', 'America/New_York', '2024-04-17 15:30:00'::timestamp), 'YYYY-MM-DD HH24:MI:SS') AS ESTDateTime;
```

##### [Back To Context](./README.md)
***
| &copy; TINITIATE.COM |
|----------------------|