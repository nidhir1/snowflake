
# ❄️ Snowflake DML — Questions Only

## 🔹 A. What is DML?
- What is DML?
- How is DML different from DDL?
- What are examples of DML commands?
- Does DML auto‑commit in Snowflake?
- How does Time Travel help with mistakes?
- Do multiple users editing data cause conflicts?
- How does Snowflake ensure consistency?

---

## 🔹 B. INSERT

### ⭐ Basics
- What is the syntax for inserting a single row?
- How do we insert multiple rows?
- How do we insert into specific columns?
- What happens if fewer or more values are provided than columns?

### ⭐ Defaults & NULLS
- What are default column values?
- When is NULL inserted automatically?
- How does NOT NULL interact with inserts?

### ⭐ INSERT…SELECT
- What does INSERT…SELECT do?
- When should we insert from another table?
- What are risks when inserting large volumes?

### ⭐ COPY INTO (Bulk Loads)
- Difference between INSERT and COPY INTO?
- When should COPY be used instead of INSERT?
- Performance and cost considerations?

---

## 🔹 C. UPDATE

### ⭐ Basics
- How do we update specific columns?
- Why is the WHERE clause important?
- What happens if we update all rows accidentally?

### ⭐ Expressions
- How do we update using calculations?
- How do we update using another column?
- How do we update using data from another table?

### ⭐ Updating VARIANT / JSON
- How do we update nested JSON fields?
- Does updating JSON overwrite the whole object?

### ⭐ UPDATE vs MERGE
- When should we use UPDATE?
- When is MERGE better?
- How does MERGE support CDC patterns?

---

## 🔹 D. DELETE

### ⭐ Basics
- How do we delete specific rows?
- How do we delete all rows?
- Difference between TRUNCATE and DELETE?
- What is logical delete vs physical delete?

### ⭐ Time Travel
- Can deleted data be restored?
- When is recovery no longer possible?

---

## 🔹 E. Transactions & Safety
- What is auto‑commit?
- What are BEGIN / COMMIT / ROLLBACK?
- What happens if a query fails mid‑transaction?
- Can uncommitted data be read by others?
- Why doesn’t Snowflake lock like OLTP systems?

---

## 🔹 F. Performance Considerations
- Why can bulk DML be expensive?
- Why do frequent deletes hurt performance?
- When is staging + INSERT better than row‑by‑row?
- Why avoid small frequent DML operations?
- Impact of DML on storage and Fail‑safe?

---

## 🔹 G. Scenario Questions
- Deleted wrong rows — what now?
- Inserted duplicate records — how to fix?
- Need to insert millions of rows — best approach?
- Need to correct a column across table — safe path?
- UPDATE without WHERE — recovery options?
- Frequent deletes but storage keeps increasing — why?
- Upsert required in incremental load — what to use?

---

## 🔹 H. Trick Questions
- Does INSERT always commit automatically?
- Can DELETE permanently remove data instantly?
- Does UPDATE rewrite the whole table?
- Can two users overwrite each other’s changes?
- Does TRUNCATE behave the same as DELETE?
- Can we ROLLBACK after auto‑commit?
- Is MERGE just another name for UPDATE?
- Does DML always make tables slower?
