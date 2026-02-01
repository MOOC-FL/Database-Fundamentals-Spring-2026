#### 4. More about SQL
##### Types and expressions
- SQL uses types and expressions in the same way as programming. We've already seen many examples of SQL commands, but now is a good time to take a deeper look at the structure of the language.

- Each database system implements types and expressions in its own way, and there are many subtle differences in how databases work. So it's a good idea to check the documentation for the database you're using for details. 
##### Types
- In the table definition, each column is given a type:
```sql
CREATE TABLE Movies (
  id INTEGER PRIMARY KEY,
  name TEXT,
  release_year INTEGER
);
```
- Here, the column `name` type is `TEXT`(string) and the column `release_year` type is `INTEGER`(integer). These are the most common types available with these names in many databases. Examples of other common types include `TIMESTAMP`(timestamp), `REAL`(floating point), and `BLOB`(raw data).
> ##### TEXT vs. VARCHAR
> The traditional way to store a string in SQL is to use the type `VARCHAR`, which specifies the maximum length of the string in parentheses. For example, type `VARCHAR(10)`means that the string can have a maximum of 10 characters.
> 
> This is a throwback to old programming, where a string could be represented as a fixed-length array of characters. TEXTHowever, a type is more convenient because you don't have to figure out a maximum length.

#### Types of SQLite
- A special feature of SQLite is that the type in the table definition is just a guideline for what type the column should be. However, we can ignore the guideline and, for example, store a string instead of an integer:
```sql
INSERT INTO Movies (name, release_year) VALUES ('Lumikki', 'abc');
```
- Additionally, the type name can be any string, even if SQLite does not have such a type. This allows us to, for example, define a column in which to store a time:
```sql
CREATE TABLE Bookings (
  id INTEGER PRIMARY KEY,
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  description TEXT
);
```
- SQLite does not have a type `TIMESTAMP`, instead times are treated as strings, but here the type of the column indicates what is intended to be stored in it.
##### **SQL Table Expressions Reference**

##### **1. GENERATED/COMPUTED COLUMNS**
| Type | SQLite Syntax | Example | Purpose |
|------|---------------|---------|---------|
| **VIRTUAL** | `GENERATED ALWAYS AS (expression) VIRTUAL` | `discounted_price AS (price * 0.9)` | Computed on read, not stored |
| **STORED** | `GENERATED ALWAYS AS (expression) STORED` | `total AS (price * quantity)` | Computed and stored on write |

##### **2. DEFAULT VALUES**
| Expression Type | Syntax Example | Result Example |
|-----------------|----------------|----------------|
| **Current Time** | `DEFAULT (datetime('now'))` | '2024-01-15 14:30:00' |
| **Random ID** | `DEFAULT (hex(randomblob(4)))` | 'A1B2C3D4' |
| **Concatenation** | `DEFAULT ('ORD-' || strftime('%Y%m%d'))` | 'ORD-20240115' |
| **Math** | `DEFAULT (10 * 2.5)` | 25.0 |

##### **3. CHECK CONSTRAINTS**
| Constraint Type | Example | Purpose |
|-----------------|---------|---------|
| **Range** | `CHECK (age BETWEEN 0 AND 120)` | Validate numeric ranges |
| **Pattern** | `CHECK (email LIKE '%_@__%.__%')` | Validate email format |
| **Logic** | `CHECK (end_date > start_date)` | Ensure logical consistency |
| **List** | `CHECK (status IN ('active','inactive'))` | Limit to valid values |

##### **4. INDEXES ON EXPRESSIONS**
| Index Type | Syntax | Use Case |
|------------|--------|----------|
| **Expression Index** | `CREATE INDEX idx_lower ON table(lower(column))` | Case-insensitive search |
| **Partial Index** | `CREATE INDEX idx_active ON table(column) WHERE active=1` | Index subset of rows |
| **Covering Index** | `CREATE INDEX idx_cover ON table(a,b,c) INCLUDE (d,e)` | Include extra columns |

##### **5. VIEWS (VIRTUAL TABLES)**
| View Type | Example | Output |
|-----------|---------|--------|
| **Aggregate View** | 
```sql
CREATE VIEW SalesSummary AS
SELECT product_id, 
       SUM(quantity) as total_sold,
       AVG(price) as avg_price
FROM orders
GROUP BY product_id
``` | Pre-computed aggregates |
| **Joined View** |
```sql
CREATE VIEW OrderDetails AS
SELECT o.id, c.name, o.total
FROM orders o
JOIN customers c ON o.customer_id = c.id
```
 | Denormalized data |

#### **6. TRIGGER EXPRESSIONS**
| Trigger Type | Example | When Executed |
|--------------|---------|---------------|
| **BEFORE INSERT** |
```sql
CREATE TRIGGER validate_email
BEFORE INSERT ON users
BEGIN
  SELECT CASE
    WHEN NEW.email NOT LIKE '%@%' THEN
      RAISE(ABORT, 'Invalid email')
  END;
END
``` | Before row inserted |
| **AFTER UPDATE** |
```sql
CREATE TRIGGER update_timestamp
AFTER UPDATE ON products
BEGIN
  UPDATE products 
  SET modified = datetime('now')
  WHERE id = NEW.id;
END
```
| After row updated |

#### **7. COMMON TABLE EXPRESSIONS (CTEs)**
| CTE Type | Structure | Purpose |
|----------|-----------|---------|
| **Simple CTE** |
```sql
WITH temp AS (
  SELECT * FROM users WHERE active=1
)
SELECT * FROM temp
``` | Temporary result set |
| **Recursive CTE** |
```sql
WITH RECURSIVE tree AS (
  SELECT id, parent_id FROM nodes WHERE parent_id IS NULL
  UNION ALL
  SELECT n.id, n.parent_id 
  FROM nodes n JOIN tree t ON n.parent_id = t.id
)
SELECT * FROM tree
```
| Hierarchical/tree data |

#### **8. WINDOW FUNCTIONS**
| Function Category | Example | Returns |
|-------------------|---------|---------|
| **Ranking** | `RANK() OVER (ORDER BY salary DESC)` | 1, 2, 2, 4... |
| **Aggregate** | `SUM(sales) OVER (PARTITION BY region)` | Running total per region |
| **Navigation** | `LAG(price) OVER (ORDER BY date)` | Previous row's value |

## **9. SELECT EXPRESSIONS**
| Expression | Example | Result Type |
|------------|---------|-------------|
| **CASE** | 
```sql
CASE 
  WHEN score >= 90 THEN 'A'
  WHEN score >= 80 THEN 'B'
  ELSE 'C'
END
```
| Conditional value |
| **COALESCE** | `COALESCE(name, 'Unknown')` | First non-null value |
| **NULLIF** | `NULLIF(column, 'N/A')` | NULL if equal |
| **String Concatenation** | `first_name || ' ' || last_name` | Full name |

#### **10. AGGREGATE EXPRESSIONS**
| Function | Example | Returns |
|----------|---------|---------|
| **COUNT with DISTINCT** | `COUNT(DISTINCT user_id)` | Unique users |
| **GROUP_CONCAT** | `GROUP_CONCAT(product_name, ', ')` | 'Apple, Banana, Cherry' |
| **FILTER Clause** | `SUM(amount) FILTER (WHERE status='paid')` | Conditional sum |

---

#### **PRACTICAL EXAMPLE: E-COMMERCE DATABASE**

```sql
-- Table with multiple expression types
CREATE TABLE Orders (
    -- Generated column
    order_id TEXT PRIMARY KEY 
        DEFAULT ('ORD-' || strftime('%Y%m%d-') || substr(hex(randomblob(3)), 1, 6)),
    
    -- Check constraints with expressions
    customer_id INTEGER NOT NULL,
    order_date TEXT DEFAULT (date('now')),
    total_amount REAL CHECK (total_amount >= 0),
    discount REAL CHECK (discount >= 0 AND discount <= total_amount),
    net_amount REAL GENERATED ALWAYS AS (total_amount - discount) STORED,
    
    -- Complex check
    CHECK (order_date >= '2020-01-01'),
    
    -- Foreign key with expression
    FOREIGN KEY (customer_id) REFERENCES Customers(id)
);

-- Index on expression
CREATE INDEX idx_order_date_year 
ON Orders(strftime('%Y', order_date));

-- View with expressions
CREATE VIEW CustomerLifetimeValue AS
SELECT 
    customer_id,
    COUNT(*) as total_orders,
    SUM(total_amount) as lifetime_spend,
    AVG(total_amount) as avg_order_value,
    MAX(order_date) as last_order_date,
    julianday('now') - julianday(MIN(order_date)) as customer_days
FROM Orders
GROUP BY customer_id;
```

---

#### **EXPRESSION PERFORMANCE GUIDE**

| Expression Type | Performance Impact | When to Use |
|-----------------|-------------------|-------------|
| **VIRTUAL Column** | ⚡ Fast read, slower compute | When storage is limited |
| **STORED Column** | 💾 Faster reads, slower writes | When frequently queried |
| **Index on Expression** | 📈 Fast lookups, slower writes | For frequent WHERE clauses |
| **Check Constraints** | ⚠️ Minimal overhead | Always for data integrity |
| **Views** | 🔄 No storage, compute on read | For complex queries |

---

**Key Takeaways:**
1. **Generated Columns** = Automatic calculations
2. **Check Constraints** = Data validation
3. **Expression Indexes** = Optimized queries
4. **Views** = Simplified complex queries
5. **Triggers** = Automated business logic



