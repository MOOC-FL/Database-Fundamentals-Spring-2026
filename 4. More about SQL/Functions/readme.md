#### Functions
- Functions can appear as part of expressions, just like in programming. Here are some examples of SQLite functions:
# **SQLite Functions Reference Table**

#### **📊 CATEGORY 1: STRING FUNCTIONS**
| Function | Syntax | Description | Example | Result |
|----------|--------|-------------|---------|--------|
| **LENGTH** | `LENGTH(string)` | Returns string length in characters | `LENGTH('Hello')` | `5` |
| **LOWER** | `LOWER(string)` | Converts to lowercase | `LOWER('SQLite')` | `'sqlite'` |
| **UPPER** | `UPPER(string)` | Converts to uppercase | `UPPER('sqlite')` | `'SQLITE'` |
| **SUBSTR** | `SUBSTR(string, start, length)` | Extracts substring | `SUBSTR('Hello', 2, 3)` | `'ell'` |
| **TRIM** | `TRIM(string)` | Removes leading/trailing spaces | `TRIM('  hello  ')` | `'hello'` |
| **LTRIM** | `LTRIM(string)` | Removes leading spaces | `LTRIM('  hello')` | `'hello'` |
| **RTRIM** | `RTRIM(string)` | Removes trailing spaces | `RTRIM('hello  ')` | `'hello'` |
| **REPLACE** | `REPLACE(string, old, new)` | Replaces occurrences | `REPLACE('aabb', 'a', 'x')` | `'xxbb'` |
| **INSTR** | `INSTR(string, substring)` | Returns position of substring | `INSTR('hello', 'll')` | `3` |
| **|| (Concatenation)** | `string1 || string2` | Concatenates strings | `'Hello' || ' ' || 'World'` | `'Hello World'` |

#### **🔢 CATEGORY 2: NUMERIC FUNCTIONS**
| Function | Syntax | Description | Example | Result |
|----------|--------|-------------|---------|--------|
| **ABS** | `ABS(number)` | Absolute value | `ABS(-5.5)` | `5.5` |
| **ROUND** | `ROUND(number, decimals)` | Rounds to decimals | `ROUND(3.14159, 2)` | `3.14` |
| **MAX** | `MAX(x, y)` | Larger of two values | `MAX(10, 20)` | `20` |
| **MIN** | `MIN(x, y)` | Smaller of two values | `MIN(10, 20)` | `10` |
| **RANDOM** | `RANDOM()` | Random 64-bit integer | `RANDOM()` | `-9223372036854775808` to `9223372036854775807` |
| **RANDOMBLOB** | `RANDOMBLOB(n)` | Random binary data (n bytes) | `RANDOMBLOB(4)` | Random 4 bytes |
| **CEIL/CEILING** | `CEIL(number)` | Rounds up to integer | `CEIL(3.2)` | `4` |
| **FLOOR** | `FLOOR(number)` | Rounds down to integer | `FLOOR(3.8)` | `3` |
| **MOD** | `MOD(x, y)` | Remainder of x/y | `MOD(10, 3)` | `1` |
| **SIGN** | `SIGN(number)` | Sign of number (-1, 0, 1) | `SIGN(-5)` | `-1` |

#### **📅 CATEGORY 3: DATE/TIME FUNCTIONS**
| Function | Syntax | Description | Example | Result |
|----------|--------|-------------|---------|--------|
| **DATE** | `DATE(timestring, modifier...)` | Returns date | `DATE('now')` | Current date |
| **TIME** | `TIME(timestring, modifier...)` | Returns time | `TIME('now')` | Current time |
| **DATETIME** | `DATETIME(timestring, modifier...)` | Returns datetime | `DATETIME('now')` | Current datetime |
| **JULIANDAY** | `JULIANDAY(timestring, modifier...)` | Julian day number | `JULIANDAY('2024-01-15')` | `2460326.5` |
| **STRFTIME** | `STRFTIME(format, timestring)` | Formats datetime | `STRFTIME('%Y-%m', 'now')` | `'2024-01'` |
| **UNIXEPOCH** | `UNIXEPOCH(timestring)` | Unix timestamp | `UNIXEPOCH('now')` | Current Unix time |

#### **📋 Common STRFTIME Format Codes:**
| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | Year (4-digit) | `2024` |
| `%m` | Month (01-12) | `01` |
| `%d` | Day (01-31) | `15` |
| `%H` | Hour (00-23) | `14` |
| `%M` | Minute (00-59) | `30` |
| `%S` | Second (00-59) | `45` |
| `%W` | Week number (00-53) | `02` |
| `%w` | Weekday (0-6, 0=Sunday) | `1` |
| `%j` | Day of year (001-366) | `015` |

#### **📁 CATEGORY 4: AGGREGATE FUNCTIONS**
| Function | Syntax | Description | Example |
|----------|--------|-------------|---------|
| **COUNT** | `COUNT(expression)` | Count of non-NULL values | `SELECT COUNT(*) FROM users` |
| **SUM** | `SUM(expression)` | Sum of values | `SELECT SUM(price) FROM products` |
| **AVG** | `AVG(expression)` | Average of values | `SELECT AVG(age) FROM users` |
| **MAX** | `MAX(expression)` | Maximum value | `SELECT MAX(salary) FROM employees` |
| **MIN** | `MIN(expression)` | Minimum value | `SELECT MIN(temperature) FROM readings` |
| **GROUP_CONCAT** | `GROUP_CONCAT(expression, separator)` | Concatenated string | `SELECT GROUP_CONCAT(name, ', ') FROM cities` |

#### **🔍 CATEGORY 5: TYPE CONVERSION FUNCTIONS**
| Function | Syntax | Description | Example | Result |
|----------|--------|-------------|---------|--------|
| **CAST** | `CAST(expression AS type)` | Type conversion | `CAST('123' AS INTEGER)` | `123` |
| **TYPEOF** | `TYPEOF(expression)` | Returns data type | `TYPEOF(3.14)` | `'real'` |
| **COALESCE** | `COALESCE(x, y, ...)` | First non-NULL value | `COALESCE(NULL, NULL, 'default')` | `'default'` |
| **NULLIF** | `NULLIF(x, y)` | NULL if x=y, else x | `NULLIF(5, 5)` | `NULL` |
| **IFNULL** | `IFNULL(x, y)` | y if x is NULL | `IFNULL(NULL, 'N/A')` | `'N/A'` |

#### **📦 CATEGORY 6: BLOB FUNCTIONS**
| Function | Syntax | Description | Example |
|----------|--------|-------------|---------|
| **HEX** | `HEX(blob)` | Hexadecimal representation | `HEX(x'010203')` → `'010203'` |
| **ZEROBLOB** | `ZEROBLOB(n)` | Zero-filled blob of n bytes | `ZEROBLOB(8)` → 8 zero bytes |
| **LENGTH** | `LENGTH(blob)` | Bytes in blob | `LENGTH(x'010203')` → `3` |

#### **🎯 PRACTICAL EXAMPLES TABLE**
| Use Case | SQL Query | Result |
|----------|-----------|--------|
| **Extract username from email** | `SELECT SUBSTR(email, 1, INSTR(email, '@')-1) FROM users` | `'john'` from `'john@example.com'` |
| **Calculate age from birthdate** | `SELECT DATE('now') - DATE(birthdate) FROM persons` | Age in years |
| **Format phone number** | `SELECT '(' || SUBSTR(phone,1,3) || ') ' || SUBSTR(phone,4,3) || '-' || SUBSTR(phone,7) FROM contacts` | `(123) 456-7890` |
| **Generate random password** | `SELECT SUBSTR(hex(randomblob(8)),1,12)` | Random 12-char string |
| **Find orders from last 30 days** | `SELECT * FROM orders WHERE date >= DATE('now', '-30 days')` | Recent orders |
| **Categorize by price range** | 
```sql
SELECT 
  CASE 
    WHEN price < 10 THEN 'Budget'
    WHEN price < 50 THEN 'Mid-range'
    ELSE 'Premium'
  END AS category,
  COUNT(*)
FROM products
GROUP BY category
```
| Count by price category |

#### **⚡ PERFORMANCE TIPS TABLE**
| Function | Performance Consideration | Recommendation |
|----------|---------------------------|----------------|
| **LENGTH** | Very fast on TEXT | Use for validation checks |
| **LIKE** with `%` prefix | Cannot use indexes | Avoid `LIKE '%suffix'` |
| **SUBSTR** in WHERE | May prevent index use | Use on small datasets |
| **UPPER/LOWER** | Case-insensitive searches | Consider `COLLATE NOCASE` instead |
| **RANDOM** | CPU intensive on large datasets | Cache results if needed |
| **Aggregates** | Use indexes for GROUP BY | Index columns in GROUP BY |

#### **🚨 COMMON PITFALLS TABLE**
| Pitfall | Wrong | Right | Reason |
|---------|-------|-------|--------|
| **Case-sensitive comparison** | `WHERE name = 'john'` | `WHERE LOWER(name) = 'john'` | `'John'` won't match |
| **Date comparison as strings** | `WHERE date > '2024-01-01'` | `WHERE DATE(date) > '2024-01-01'` | String comparison fails |
| **NULL handling** | `WHERE price != NULL` | `WHERE price IS NOT NULL` | NULL != NULL is NULL |
| **Integer division** | `SELECT 5/2` → `2` | `SELECT 5.0/2` → `2.5` | Integer division truncates |
| **Substring indexing** | `SUBSTR('Hello', 0, 3)` → `'He'` | `SUBSTR('Hello', 1, 3)` → `'Hel'` | SQLite uses 1-based indexing |

#### **🔄 FUNCTION CHAINING TABLE**
| Chain | Result | Use Case |
|-------|--------|----------|
| `UPPER(TRIM(name))` | Clean, uppercase names | Data normalization |
| `ROUND(AVG(price), 2)` | Average rounded to 2 decimals | Financial reports |
| `DATE('now', 'start of month', '+1 month', '-1 day')` | Last day of current month | Date calculations |
| `COALESCE(NULLIF(description, ''), 'No description')` | Replace empty with default | Display formatting |
| `SUBSTR(email, INSTR(email, '@') + 1)` | Domain from email | Email analysis |

#### **📈 REAL-WORLD SCENARIOS**

#### **1. User Management System**
```sql
-- Create username from email
UPDATE users 
SET username = LOWER(SUBSTR(email, 1, INSTR(email, '@')-1));

-- Find inactive users (last login > 90 days ago)
SELECT * FROM users 
WHERE DATE(last_login) < DATE('now', '-90 days');

-- Generate display name
SELECT 
  UPPER(SUBSTR(first_name, 1, 1)) || '. ' || last_name AS display_name,
  LENGTH(password) >= 8 AS has_strong_password
FROM users;
```

#### **2. E-commerce Analytics**
```sql
-- Monthly sales report
SELECT 
  STRFTIME('%Y-%m', order_date) AS month,
  COUNT(*) AS order_count,
  SUM(total) AS revenue,
  ROUND(AVG(total), 2) AS avg_order,
  MAX(total) AS largest_order
FROM orders
GROUP BY month
ORDER BY month DESC;

-- Product price analysis
SELECT 
  CASE 
    WHEN price BETWEEN 0 AND 10 THEN '0-10'
    WHEN price BETWEEN 10 AND 50 THEN '10-50'
    ELSE '50+'
  END AS price_range,
  COUNT(*) AS product_count,
  ROUND(AVG(stock), 0) AS avg_stock
FROM products
GROUP BY price_range;
```

#### **3. Log Analysis**
```sql
-- Error frequency by hour
SELECT 
  STRFTIME('%H', timestamp) AS hour,
  COUNT(*) AS error_count,
  GROUP_CONCAT(DISTINCT error_code, ', ') AS error_codes
FROM logs
WHERE error_code IS NOT NULL
GROUP BY hour
ORDER BY error_count DESC;

-- Session duration in minutes
SELECT 
  user_id,
  ROUND((JULIANDAY(logout_time) - JULIANDAY(login_time)) * 1440, 1) AS minutes_online
FROM sessions;
```

#### **🔧 FUNCTION REFERENCE QUICK LOOKUP**
| Need | Function | Example |
|------|----------|---------|
| **Clean text input** | `TRIM()` | `TRIM(user_input)` |
| **Extract part of string** | `SUBSTR()` | `SUBSTR(phone, 1, 3)` |
| **Format dates** | `STRFTIME()` | `STRFTIME('%Y-%m-%d', date)` |
| **Handle missing values** | `COALESCE()` | `COALESCE(price, 0)` |
| **Random sampling** | `RANDOM()` | `ORDER BY RANDOM() LIMIT 10` |
| **Case-insensitive search** | `LOWER()` or `COLLATE NOCASE` | `WHERE LOWER(name) = 'john'` |
| **Calculate differences** | `JULIANDAY()` or `DATE()` | `DATE('now') - DATE(birthdate)` |
| **Group text values** | `GROUP_CONCAT()` | `GROUP_CONCAT(tags, ', ')` |

This comprehensive reference table helps you quickly find the right function for any SQLite task!
