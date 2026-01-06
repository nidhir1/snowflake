# ❄️ MASTER DQL QUESTION BANK — SNOWFLAKE (Questions Only)

> One consolidated file. No splitting into multiple docs.

---

## 1️⃣ DQL DATA SETUP

- Why do we need sample data before practicing queries?
- What kind of datasets are most useful for learning?
- Why realistic test data matters?
- How to create tables for practicing joins?
- Why insert messy / incomplete data sometimes?

---

## 2️⃣ CORE DQL — SELECT / WHERE / GROUP BY / HAVING / ORDER / LIMIT

### A. SELECT
- What does SELECT actually do?
- What is projection?
- Why avoid SELECT *?
- Can you rename columns in SELECT?
- What happens if two columns have same name?
- Order of logical SQL execution vs written order.

### B. WHERE
- What is filtered in WHERE?
- NULL behavior in WHERE.
- AND vs OR vs NOT.
- Order of conditions.
- Why functions in WHERE sometimes slow queries?

### C. GROUP BY
- Why group records?
- Rules for columns in GROUP BY.
- Aggregation mistakes beginners make.
- GROUP BY with expressions.

### D. HAVING
- HAVING vs WHERE.
- Filtering aggregated results.
- Why HAVING can be expensive.

### E. ORDER BY
- Sorting rules.
- Sorting by alias.
- Sorting numbers stored as text.
- Sorting NULL values.

### F. LIMIT
- LIMIT vs FETCH vs SAMPLE.
- LIMIT without ORDER BY — danger.
- Pagination concepts.

---

## 3️⃣ BASIC OPERATORS

- = operator rules.
- <> vs !=.
- IN with value lists.
- IN with subqueries.
- NOT IN with NULL — why dangerous?
- LIKE vs ILIKE.
- Pattern matching tricks.
- BETWEEN boundaries.
- Numeric comparisons.
- Date comparisons.
- EXISTS vs NOT EXISTS.
- EXISTS vs IN performance.

---

## 4️⃣ SET OPERATIONS

- UNION vs UNION ALL.
- INTERSECT.
- EXCEPT.
- Column number + datatype rule.
- Sorting + set operations.
- Removing duplicates efficiently.

---

## 5️⃣ JOINS

- Purpose of joins.
- Primary vs foreign key role.
- INNER JOIN.
- LEFT JOIN.
- RIGHT JOIN.
- FULL OUTER JOIN.
- CROSS JOIN.
- JOIN with additional filters.
- Multiple join order.
- Duplicate rows caused by joins.
- Nulls in joins.
- Self joins.
- Join performance tips.

---

## 6️⃣ STRING FUNCTIONS

- LENGTH.
- SUBSTR.
- CONCAT and ||.
- LOWER / UPPER.
- TRIM / LTRIM / RTRIM.
- POSITION.
- LEFT / RIGHT.
- REVERSE.
- REPLACE.
- CASE expressions.
- NVL / IFNULL / COALESCE.
- SPLIT.
- INITCAP.

Key thinking topics:
- handling NULLs,
- whitespace issues,
- Unicode / collation,
- performance impact.

---

## 7️⃣ DATE & TIME FUNCTIONS

- CURRENT_TIMESTAMP.
- DATE_PART / EXTRACT.
- DATEDIFF.
- DATEADD / DATESUB.
- DATE_TRUNC.
- TO_CHAR formatting.
- TO_DATE / TO_TIMESTAMP.
- Convert timestamp to string.
- Convert string to timestamp.
- Working with time zones.
- UTC vs system time.
- Offset handling.
- Timezone conversions.
- DST edge cases.
- Truncation mistakes.

---

## 8️⃣ AGGREGATE FUNCTIONS

- COUNT(*).
- COUNT(column).
- COUNT(DISTINCT).
- SUM / AVG / MIN / MAX.
- Aggregates with NULLs.
- Aggregates with GROUP BY.
- Combining aggregates with CASE.

---

## 9️⃣ ANALYTICAL (WINDOW) FUNCTIONS

- What is a window function?
- PARTITION BY vs GROUP BY.
- ORDER BY inside window.
- ROW_NUMBER.
- RANK vs DENSE_RANK.
- NTILE.
- LAG / LEAD.
- FIRST_VALUE / LAST_VALUE.
- Frame clause (ROWS BETWEEN etc.).
- Why window queries can be expensive.

---

## 🔟 COMMON TABLE EXPRESSIONS (CTEs)

- What is a CTE?
- Why use CTEs?
- Recursive CTE basics.
- CTE vs subquery.
- Multiple chained CTEs.
- Readability vs performance.
- Common mistakes.

---

## 🎯 SCENARIO QUESTIONS (REAL THINKING)

- Query returns duplicates — what to check?
- NOT IN returns zero rows — why?
- LEFT JOIN unexpectedly reduces rows — why?
- GROUP BY fails — missing column error.
- Dates shift by hours — timezone issue.
- UNION reduced record count unexpectedly.
- ORDER BY not respected — because no LIMIT.
- Window function produced strange ranking.
- LIKE search case mismatch.
- CROSS JOIN exploded row count massively.

---

## ❗ TRICK QUESTIONS STUDENTS ALWAYS MISS

- Does WHERE filter before SELECT?
- Does COUNT ignore NULLs?
- Does NULL = NULL return TRUE?
- Does LIMIT guarantee consistent results?
- Does GROUP BY sort results automatically?
- Do joins require primary keys?
- Does EXISTS return rows?
- Does DATEADD change timezone?
- Do CTEs always improve performance?
- Does BETWEEN include both bounds?
