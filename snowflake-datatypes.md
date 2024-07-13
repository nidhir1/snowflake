![Snowflake Tinitiate Image](snowflake_tinitiate.png)

# Snowflake
&copy; TINITIATE.COM

##### [Back To Context](./README.md)

# Snowflake Data Types
* Snowflake provides a variety of data types, each with unique strengths and limitations.

## Commonly used data types in Snowflake:
* **Numeric** data types are used to store numbers. There are several different numeric data types available, each with its own range and precision. The most common numeric data types are:

    * **NUMBER** - Stores numbers with a specified precision and scale. For example, NUMBER(10,2) can store numbers from -99999999.99 to 99999999.99.
        * **Example:** `price NUMBER(10,2)`.

    * **INT** (or **INTEGER**) - Stores whole numbers.
        * **Example:** `quantity INT`.

    * **FLOAT** (or **DOUBLE**, **REAL**) - Stores floating-point numbers.
        * **Example:** `temperature FLOAT`.

* **Date and Time** data types are used to store dates and times. The most common date and time data types are:
    * **DATE** - Stores a date in the format YYYY-MM-DD.
        * **Example:** `birthdate DATE`.

    * **TIME** - Stores a time of day.
        * **Example:** `appointment_time TIME`.

    * **DATETIME** - Stores a date and time value without timezone.
        * **Example:** `event_time DATETIME`.
    
    * **TIMESTAMP** - Stores a date and time value.
        * **Example:** `event_timestamp TIMESTAMP`.

    * **TIMESTAMP_NTZ** - Same as TIMESTAMP, Stores a timestamp without timezone in the format YYYY-MM-DD HH:MI:SS.
        * **Example:** `created_at TIMESTAMP_NTZ`.

    * **TIMESTAMP_LTZ** - Stores a timestamp with timezone information in the format YYYY-MM-DD HH:MI:SS TZ.
        * **Example:** `updated_at TIMESTAMP_LTZ`.

    * **TIMESTAMP_TZ** - Stores a timestamp with a timezone offset in the format YYYY-MM-DD HH:MI:SS ±TZ.
        * **Example:** `order_date TIMESTAMP_TZ`.

* **String** data types are used to store text. The most common string data types are:
    * **CHAR** - Stores a fixed-length string.
        * **Example:** `country_code CHAR(2)`.

    * **VARCHAR** - Stores a variable-length string with a maximum length specified.
        * **Example:** `name VARCHAR(50)`.

    * **TEXT** - Stores variable-length strings with no specified length limit.
        * **Example:** `description TEXT`.

* **Binary** data types are used to store binary data, such as images or files.
    * **BINARY** - Stores fixed-length binary data.
        * **Example:** `binary_data BINARY(10)`.

    * **VARBINARY** - Stores variable-length binary data.
        * **Example:** `image VARBINARY`.

* **Boolean** data types are used to store true or false values.
    * **BOOLEAN** - Stores true or false values.
        * **Example:** `is_active BOOLEAN`.

* **Variant** data types are used to store semi-structured data, such as JSON, Avro, or XML.
    * **VARIANT** - Stores semi-structured data.
        * **Example:** `metadata VARIANT`.

* **Geospatial** data types are used to store geographic data.
    * **GEOGRAPHY** - Stores geographic data.
        * **Example:** `location GEOGRAPHY`.

* **Additional Data Types** include:
    * **ARRAY** - Stores an ordered collection of elements.
        * **Example:** `tags ARRAY`.
    
    * **OBJECT** - Stores a collection of key-value pairs.
        * **Example:** `config OBJECT`.

    * **BINARY** - Stores binary data.
        * **Example:** `file_data BINARY`.
    
    * **SEMISTRUCTURED** - Used for semi-structured data types, including VARIANT, ARRAY, and OBJECT.
        * **Example:** `payload SEMISTRUCTURED`.

##### [Back To Context](./README.md)
***
| &copy; TINITIATE.COM |
|----------------------|