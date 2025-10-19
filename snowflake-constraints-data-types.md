# Snowflake Constraints and Data Types Tutorial

## Table of Contents
1. [Introduction](#introduction)
2. [Constraints in Snowflake](#constraints-in-snowflake)
    - [NULL and NOT NULL Properties](#null-and-not-null-properties)
    - [Unique, Primary, and Foreign Keys](#unique-primary-and-foreign-keys)
    - [Check Constraints](#check-constraints)
    - [Data Validation Considerations](#data-validation-considerations)
3. [Data Types in Snowflake](#data-types-in-snowflake)
    - [Numeric Data Types](#numeric-data-types)
    - [String Data Types](#string-data-types)
    - [Binary Data Types](#binary-data-types)
    - [Boolean Data Types](#boolean-data-types)
    - [Date and Time Data Types](#date-and-time-data-types)
    - [Semi-Structured Data Types](#semi-structured-data-types)
    - [Geospatial & Variant Data Types](#geospatial--variant-data-types)
4. [Examples](#examples)
5. [Best Practices](#best-practices)

---

## Introduction

In Snowflake, constraints and data types are essential for ensuring **data integrity, accuracy, and proper storage**. Snowflake supports a wide array of constraints for structured data and diverse data types, including numeric, string, semi-structured, and geospatial formats.

---

## Constraints in Snowflake

### NULL and NOT NULL Properties

- **NULL**: Column can store missing or undefined values.
- **NOT NULL**: Column must always have a value; cannot be left empty.

**Example:**
```sql
CREATE TABLE customer_data (
    customer_id INT NOT NULL,
    name STRING NOT NULL,
    email STRING NULL
);
```

### Unique, Primary, and Foreign Keys

- **UNIQUE**: Ensures values in a column are unique.
- **PRIMARY KEY**: Identifies unique row; implicitly NOT NULL.
- **FOREIGN KEY**: Establishes referential integrity between tables.

**Examples:**
```sql
-- Unique constraint
CREATE TABLE employees (
    emp_id INT UNIQUE,
    emp_name STRING
);

-- Primary key
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE
);

-- Foreign key
ALTER TABLE orders
ADD CONSTRAINT fk_customer
FOREIGN KEY (customer_id) REFERENCES customer_data(customer_id);
```

### Check Constraints

- Ensures column values meet a specified condition.
- Useful for validating data integrity.

**Example:**
```sql
CREATE TABLE product (
    product_id INT PRIMARY KEY,
    price NUMBER CHECK(price >= 0)
);
```

### Data Validation Considerations

- Snowflake **does not physically enforce foreign key or unique constraints** for performance reasons.
- Constraints act as **metadata**; validation should be performed via queries or ETL processes.

---

## Data Types in Snowflake

### Numeric Data Types

| Type | Description |
|------|-------------|
| NUMBER / DECIMAL / NUMERIC | Arbitrary precision numbers |
| INTEGER / INT / BIGINT / SMALLINT / TINYINT | Whole numbers |
| FLOAT / FLOAT4 / FLOAT8 / DOUBLE | Floating-point numbers |

**Example:**
```sql
CREATE TABLE numeric_demo (
    id INT,
    price DECIMAL(10,2),
    rating FLOAT
);
```

### String Data Types

| Type | Description |
|------|-------------|
| STRING / VARCHAR | Variable-length string |
| CHAR / CHARACTER | Fixed-length string |
| TEXT | Long text data |

**Example:**
```sql
CREATE TABLE string_demo (
    name STRING,
    description TEXT
);
```

### Binary Data Types

- **BINARY**: Stores binary objects such as images, files, or encrypted content.

**Example:**
```sql
CREATE TABLE binary_demo (
    file_id INT,
    file_data BINARY
);
```

### Boolean Data Types

- **BOOLEAN**: Stores TRUE, FALSE, or NULL.

**Example:**
```sql
CREATE TABLE boolean_demo (
    is_active BOOLEAN
);
```

### Date and Time Data Types

| Type | Description |
|------|-------------|
| DATE | Stores only the date |
| TIME | Stores only the time |
| TIMESTAMP / TIMESTAMP_NTZ / TIMESTAMP_LTZ / TIMESTAMP_TZ | Stores date and time with optional time zone |

**Example:**
```sql
CREATE TABLE datetime_demo (
    event_date DATE,
    event_time TIME,
    event_timestamp TIMESTAMP_NTZ
);
```

### Semi-Structured Data Types

- **VARIANT**: Holds JSON, XML, Avro, Parquet, or other semi-structured data.
- **OBJECT**: Stores key-value pairs.
- **ARRAY**: Stores ordered lists.

**Example:**
```sql
CREATE TABLE semi_structured_demo (
    data VARIANT
);
INSERT INTO semi_structured_demo VALUES ('{"name": "John", "age": 30}');
```

### Geospatial & Variant Data Types

- **GEOGRAPHY**: Stores geospatial data (points, polygons, multipolygons).
- **VARIANT**: Can store geospatial or other complex data types.

**Example:**
```sql
CREATE TABLE geospatial_demo (
    location GEOGRAPHY
);
INSERT INTO geospatial_demo VALUES (GEOGRAPHY::ST_POINT(-122.41, 37.77));
```

---

## Examples

**Combining Constraints and Data Types:**
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_total DECIMAL(10,2) CHECK(order_total >= 0),
    order_date DATE NOT NULL,
    metadata VARIANT
);
```

**Using Semi-Structured and Geospatial Types:**
```sql
INSERT INTO orders(metadata) VALUES ('{"source": "web", "campaign": "spring_sale"}');
INSERT INTO geospatial_demo(location) VALUES (GEOGRAPHY::ST_POLYGON('((-122.5 37.7, -122.5 37.8, -122.4 37.8, -122.4 37.7, -122.5 37.7))')); 
```

---

## Best Practices

1. Define **NOT NULL** for mandatory fields to ensure completeness.
2. Use **PRIMARY and UNIQUE keys** for logical integrity.
3. Leverage **CHECK constraints** for data validation.
4. Use **appropriate data types** to optimize storage and performance.
5. Use **VARIANT** for semi-structured data and **GEOGRAPHY** for spatial analytics.
6. Enforce constraints in queries or ETL pipelines as Snowflake may not enforce all constraints physically.

---

## References

- [Snowflake Data Types Documentation](https://docs.snowflake.com/en/sql-reference/data-types.html)
- [Snowflake Constraints Documentation](https://docs.snowflake.com/en/user-guide/tables-constraints.html)
- [Semi-Structured Data in Snowflake](https://docs.snowflake.com/en/user-guide/semistructured-overview.html)
- [Geospatial Data Types](https://docs.snowflake.com/en/user-guide/geospatial.html)

