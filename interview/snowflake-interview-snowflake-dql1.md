# ❄️ MASTER DQL — FULL QUESTIONS & DETAILED ANSWERS (Snowflake)

> One **single** file covering DQL: SELECT, filters, joins, functions, CTEs, scenarios & trick questions.

---

## 1️⃣ DQL DATA SETUP

### ❓ Why do we need sample data before practicing queries?
You can’t learn real SQL just by reading syntax. You need tables with **actual rows** so you can see how joins behave, how NULLs break logic, and how filters interact. Sample data lets you make mistakes safely and then debug your thinking.

### ❓ What kind of datasets are most useful for learning?
The best practice datasets look like real business data, for example:
- `customers`, `orders`, `order_items`
- `accounts`, `transactions`
- `products`, `inventory`, `stores`  

They should include:
- multiple relationships (1–many, many–many)
- time-based columns
- some dirty data (NULLs, duplicates, weird codes)

### ❓ Why realistic test data matters?
If your test data is “too clean”, most bugs never appear:
- joins silently duplicate or lose rows in production
- filters fail when NULLs or unexpected values appear
- date/time logic breaks when time zones are involved  

Realistic data forces you to think about **edge cases**, which is what differentiates a senior engineer from a junior one.

### ❓ How to create tables for practicing joins?
Create 2–3 tables with shared keys:
- `customers(customer_id, name, region)`
- `orders(order_id, customer_id, order_date, amount)`
- `payments(payment_id, order_id, paid_amount)`  

Then deliberately insert rows that:
- have orders without payments
- payments without corresponding order (bad data)
- customers with zero orders  

These patterns are exactly what LEFT / INNER / FULL JOIN training needs.

### ❓ Why insert messy / incomplete data sometimes?
Because real data:
- is missing fields (NULLs)
- has outdated codes
- has inconsistent text (`'NY'`, `'New York'`, `'Ny'`)  

By practicing on messy data, you learn:
- COALESCE, NVL
- CASE expressions
- how NOT IN + NULL can silently break logic
- why trimming and normalizing strings matters

---

## 2️⃣ CORE DQL — SELECT / WHERE / GROUP BY / HAVING / ORDER / LIMIT

### A. SELECT

#### ❓ What does SELECT actually do?
`SELECT` *does not change data*. It just **projects** and **filters** existing rows to produce a result set. Think of it as taking a picture of the data at a point in time; the underlying tables remain unchanged.

#### ❓ What is projection?
Projection means choosing **which columns** to include in the output.  
Example:
```sql
SELECT first_name, last_name FROM customers;
```
You’re projecting only these two columns out of the full table.

#### ❓ Why avoid `SELECT *`?
Because:
- It scans more data than needed → more cost.
- Schema changes can break downstream tools silently.
- It makes debugging harder (“where did this column come from?”).
- BI dashboards and APIs become slower than necessary.  

Use explicit columns instead. `SELECT *` is acceptable for quick ad‑hoc exploration, but not in production code.

#### ❓ Can you rename columns in SELECT?
Yes, using aliases:
```sql
SELECT first_name AS fname, last_name AS lname
FROM customers;
```
Aliases:
- improve readability
- avoid name conflicts
- help when building views or feeding BI tools

#### ❓ What happens if two columns have the same name?
If you do this:
```sql
SELECT c.id, o.id FROM customers c JOIN orders o ON c.id = o.customer_id;
```
many tools will show both columns labeled `ID`. This causes confusion in BI layers, ORMs, and further SQL. Always use unique aliases in such cases.

#### ❓ Order of logical SQL execution vs written order?
You **write** SQL roughly as:
```sql
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
LIMIT ...
```

But **Snowflake logically evaluates** in this order:
1. FROM (and JOINs)
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT (projection & expressions)
6. ORDER BY
7. LIMIT / FETCH  

Understanding this explains:
- why you cannot use SELECT aliases in WHERE (they don’t exist yet)
- why HAVING sees aggregated values, but WHERE doesn’t

---

### B. WHERE

#### ❓ What is filtered in WHERE?
WHERE operates on **rows**, not aggregate groups. It decides which rows are included *before* grouping or aggregations. It only sees raw column values, not aggregated results.

#### ❓ How does NULL behave in WHERE?
Any comparison with NULL returns **UNKNOWN**, which behaves like FALSE. So:

```sql
WHERE col = 5  -- rows with NULL in col are excluded
```

To include NULLs explicitly:
```sql
WHERE col IS NULL
```

#### ❓ AND vs OR vs NOT — what can go wrong?
The classic trap:
```sql
WHERE status = 'A' OR status = 'B' AND is_active = TRUE
```
`AND` has higher precedence, so this is read as:
```sql
WHERE status = 'A' OR (status = 'B' AND is_active = TRUE)
```
Use parentheses to make logic crystal clear.

#### ❓ Order of conditions — does it matter?
Logically no — SQL is a **declarative** language. But the optimizer may rearrange conditions for performance. You still should write them in a readable way for humans first.

#### ❓ Why do functions in WHERE sometimes slow queries?
If you wrap a column in a function, Snowflake often cannot use metadata (min/max) for partition pruning:

```sql
WHERE TO_DATE(order_date) = '2025-01-01'
```
Better:
```sql
WHERE order_date >= '2025-01-01'
  AND order_date <  '2025-01-02'
```

---

### C. GROUP BY

#### ❓ Why group records?
To collapse multiple rows into summary metrics:
- total sales per region
- count of orders per customer
- average balance per account type

#### ❓ Rules for columns in GROUP BY
Every column in SELECT must either:
- be in the GROUP BY, or
- be wrapped in an aggregate function (COUNT, SUM, MAX, etc.)

Otherwise SQL wouldn’t know **which value** to display for that column when multiple rows are grouped.

#### ❓ Common aggregation mistakes
- Selecting non-grouped, non-aggregated columns (invalid).
- Assuming aggregate ignores filters (remember WHERE vs HAVING).
- Using COUNT(col) expecting to count rows, but NULLs get excluded.

#### ❓ GROUP BY with expressions
You can group by expressions, but remember you must use the **same expression**:
```sql
SELECT DATE_TRUNC('month', order_date) AS month, SUM(amount)
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

---

### D. HAVING

#### ❓ HAVING vs WHERE?
- WHERE filters **rows** before grouping.
- HAVING filters **groups** after aggregation.

Example:
```sql
-- Customers who have placed at least 5 orders
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 5;
```

#### ❓ Why can HAVING be expensive?
Because all rows are first grouped, and only then some groups are filtered out. If you can move conditions to WHERE, do so — it reduces the dataset earlier.

---

### E. ORDER BY

#### ❓ Sorting rules
You can ORDER BY:
- column positions (`ORDER BY 1, 2`)
- column names
- expressions
- aliases (but only from SELECT list)

#### ❓ Sorting numbers stored as text
If `amount` is stored as TEXT, `"100"` comes before `"2"` lexicographically.  
Cast it:
```sql
ORDER BY TO_NUMBER(amount)
```

#### ❓ Sorting NULLs
Snowflake by default sorts:
- NULLs first for ASC
- NULLs last for DESC  

You can control explicitly:
```sql
ORDER BY amount ASC NULLS LAST;
```

---

### F. LIMIT

#### ❓ LIMIT vs FETCH vs SAMPLE
- `LIMIT`/`FETCH` → restrict number of rows returned.
- `SAMPLE` → randomly sample rows (probabilistic).

#### ❓ LIMIT without ORDER BY — why dangerous?
Without ORDER BY, Snowflake can return **any arbitrary subset** of rows. It might change between runs; results are not stable. Always combine LIMIT with ORDER BY when you rely on particular rows.

#### ❓ Pagination concepts
Classic pattern:
```sql
SELECT ...
FROM ...
ORDER BY created_at
LIMIT 50 OFFSET 100;
```

Be careful: large OFFSETs can be inefficient; keyset pagination using `WHERE` on last seen value is often better.

---

## 3️⃣ BASIC OPERATORS

### ❓ =, <> and !=
- `=` checks equality, but NULL comparisons always return UNKNOWN.
- `<>` and `!=` are equivalent for “not equal”.

### ❓ IN with value lists and subqueries
- `col IN (1,2,3)` is syntactic sugar for `col = 1 OR col = 2 OR col = 3`.
- `col IN (SELECT id FROM ...)` is very common for filtering by a list from another table.

### ❓ Why is `NOT IN` with NULL dangerous?
If the subquery returns even one NULL, the whole `NOT IN` comparison returns UNKNOWN → no rows match.  
Preferred pattern:
```sql
WHERE NOT EXISTS (
  SELECT 1 FROM other o WHERE o.id = t.id
);
```

### ❓ LIKE vs ILIKE
- `LIKE` is case-sensitive.
- `ILIKE` is case-insensitive in Snowflake.

Use `%` as wildcard, `_` for single character.

### ❓ BETWEEN boundaries
`BETWEEN a AND b` is **inclusive** of both a and b.  
For dates, a safer pattern for ranges is often:
```sql
date >= '2025-01-01' AND date < '2025-02-01'
```

### ❓ EXISTS vs NOT EXISTS vs IN
- EXISTS checks for presence of rows, often faster for complex joins.
- IN is simpler for small lookup lists.
- NOT IN has NULL pitfalls; NOT EXISTS avoids them.

---

## 4️⃣ SET OPERATIONS

### ❓ UNION vs UNION ALL
- `UNION` removes duplicates (extra sort/comparison cost).
- `UNION ALL` keeps all rows (faster, cheaper).

Default to `UNION ALL` unless you truly need deduplication.

### ❓ Column number + datatype rule
Both queries in a set operation must return the **same number of columns** and compatible data types in matching positions.

### ❓ Sorting + set operations
To sort the final combined data, put ORDER BY outside:
```sql
SELECT ... FROM t1
UNION ALL
SELECT ... FROM t2
ORDER BY 1;
```

---

## 5️⃣ JOINS

### ❓ Purpose of joins
Joins combine rows from two (or more) tables based on related columns to answer multi-table questions, like “orders with customer details”.

### ❓ Primary vs foreign key role
- Primary key: uniquely identifies a row in a table.
- Foreign key: references a primary key in another table.

Even though Snowflake doesn’t enforce referential integrity physically, designing PK/FK is still helpful conceptually for joins.

### ❓ Join types in practice
- INNER JOIN → only matching rows on both sides.
- LEFT JOIN → all rows from left, with NULLs when no match on right.
- RIGHT JOIN → symmetric to LEFT; used less commonly.
- FULL OUTER JOIN → rows with matches + non-matching from both sides.
- CROSS JOIN → cartesian product (every combination) — use carefully.

### ❓ JOIN with additional filters
Correct pattern:
```sql
SELECT ...
FROM a
LEFT JOIN b
  ON a.id = b.id
 AND b.flag = 'Y';
```
Placing conditions in ON vs WHERE changes whether unmatched rows are preserved (for outer joins).

### ❓ Why do joins cause duplicate rows?
If one `customer` has 5 `orders`, after join you see 5 rows. That’s **expected** 1-to-many behavior. If you want only one row per customer, you must aggregate or pick one row explicitly.

### ❓ Nulls in joins
If the join key is NULL, that row will not match using standard equality join. You often need to handle NULLs explicitly or treat them as special cases.

### ❓ Join performance tips
- Join on correct keys with matching types.
- Avoid functions on join columns.
- Reduce data earlier (WHERE) before heavy joins when possible.

---

## 6️⃣ STRING FUNCTIONS (Key Concepts)

Common functions:
- LENGTH, SUBSTR, CONCAT/`||`
- LOWER, UPPER
- TRIM, LTRIM, RTRIM
- POSITION
- LEFT, RIGHT
- REVERSE, REPLACE
- CASE expressions
- NVL / IFNULL / COALESCE
- SPLIT, INITCAP

Important points:
- Always trim IDs coming from flat files (spaces are silent bugs).
- Treat NULL and empty string differently where needed.
- Use COALESCE when combining multiple optional fields.

---

## 7️⃣ DATE & TIME FUNCTIONS

Key functions:
- `CURRENT_TIMESTAMP`
- `DATE_PART` / `EXTRACT`
- `DATEDIFF`, `DATEADD`, `DATE_TRUNC`
- `TO_CHAR` for display formatting
- `TO_DATE`, `TO_TIMESTAMP` for parsing

### ❓ Time zones and UTC
Best practice is to store timestamps in UTC and convert only at the presentation layer. Be careful:
- DST transitions
- mixed local time zones without zone info
- implicit casts that drop timezone

Always be explicit about the timezone whenever it matters.

---

## 8️⃣ AGGREGATE FUNCTIONS

### ❓ COUNT(*), COUNT(column), COUNT(DISTINCT)
- `COUNT(*)` → all rows, including rows where columns are NULL.
- `COUNT(column)` → counts only rows where that column is NOT NULL.
- `COUNT(DISTINCT col)` → unique non-NULL values (more expensive).

NULLs are **ignored** by most aggregates (SUM, AVG).

---

## 9️⃣ ANALYTICAL (WINDOW) FUNCTIONS

### ❓ What is a window function?
A window function computes a value across a set of rows that are related to the current row **without collapsing them** like GROUP BY.

Examples: ranking, running totals, moving averages.

### ❓ PARTITION BY vs GROUP BY
- GROUP BY collapses rows per group.
- PARTITION BY keeps rows but groups them logically for the window.

### ❓ Frame clauses
You can define which rows to include in the window:
```sql
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```
This enables running totals and moving windows.

Window queries can be expensive on very large partitions; use PARTITION BY wisely.

---

## 🔟 COMMON TABLE EXPRESSIONS (CTEs)

### ❓ What is a CTE and why use it?
A CTE is a named subquery defined with `WITH`:
```sql
WITH base AS (
  SELECT ... FROM ...
)
SELECT ... FROM base WHERE ...;
```

Benefits:
- improves readability
- lets you reuse logic
- helps break complex queries into steps

CTEs are not automatically materialized; Snowflake may inline them. They are mostly a **logical** structuring tool.

---

## 🎯 SCENARIO QUESTIONS — HOW TO THINK

### ❓ Query returns duplicates — what to check?
- Are joins creating 1-to-many matches?
- Is UNION vs UNION ALL appropriate?
- Should aggregation or DISTINCT have been applied?

### ❓ NOT IN returns zero rows — why?
Because the subquery returned a NULL and `NOT IN` with NULL yields UNKNOWN. Switch to NOT EXISTS.

### ❓ LEFT JOIN unexpectedly reduces rows
Likely a filter in WHERE wiped out rows with NULL on the right-hand side. Move conditions to the ON clause when needed.

### ❓ UNION reduced record count unexpectedly
UNION removed duplicates. Use UNION ALL if duplicates are logically valid.

### ❓ CROSS JOIN exploded rows
Because every row from table A was joined to every row from table B. Ensure a join condition exists, unless you intentionally need a cartesian product.

---

## ❗ TRICK QUESTIONS — ANSWERED

- **Does WHERE filter before SELECT?**  
  Yes, logically WHERE is applied before SELECT projection.

- **Does COUNT ignore NULLs?**  
  `COUNT(column)` does, `COUNT(*)` does not.

- **Does NULL = NULL return TRUE?**  
  No, it returns UNKNOWN. Use `IS NULL` instead.

- **Does LIMIT guarantee consistent results?**  
  Only when combined with a deterministic ORDER BY.

- **Does GROUP BY sort results automatically?**  
  No. Any ordering must be explicit with ORDER BY.

- **Do joins require primary keys?**  
  No, but well-defined keys improve correctness and performance.

- **Does EXISTS return rows?**  
  EXISTS returns TRUE/FALSE per outer row — it does not output rows from the subquery.

- **Do CTEs always improve performance?**  
  No. They help readability. The optimizer decides the actual plan.

- **Does BETWEEN include both bounds?**  
  Yes, it is inclusive.

---

## 🎓 Final Takeaway

Mastering DQL is less about memorizing functions and more about **thinking in sets**:
- how rows combine
- how filters apply
- how NULLs behave
- how time and text are represented

Once you are fluent here, everything else in Snowflake (clustering, performance tuning, modeling) becomes much easier to reason about.
