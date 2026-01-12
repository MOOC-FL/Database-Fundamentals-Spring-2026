In SQL, search conditions are primarily specified in the `WHERE` clause. Here are the key components and operators you can use:

## **Basic WHERE Clause**
```sql
SELECT * FROM table_name 
WHERE condition;
```

## **Comparison Operators**
- **`=`** : Equal
- **`<>` or `!=`** : Not equal
- **`>`** : Greater than
- **`<`** : Less than
- **`>=`** : Greater than or equal
- **`<=`** : Less than or equal

```sql
SELECT * FROM employees 
WHERE salary > 50000;
```

## **Logical Operators**
- **`AND`** : All conditions must be true
- **`OR`** : At least one condition must be true
- **`NOT`** : Negates a condition

```sql
SELECT * FROM products 
WHERE price > 100 AND category = 'Electronics';
```

## **Pattern Matching (LIKE)**
- **`%`** : Matches any sequence of characters
- **`_`** : Matches any single character

```sql
SELECT * FROM customers 
WHERE name LIKE 'J%'; -- Names starting with J
WHERE email LIKE '%@gmail.com'; -- Gmail addresses
WHERE phone LIKE '555-___-____'; -- Pattern matching
```

## **Range Conditions**
- **`BETWEEN`** : Inclusive range check
- **`IN`** : Match any value in a list

```sql
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

SELECT * FROM products 
WHERE category IN ('Books', 'Electronics', 'Clothing');
```

## **NULL Values**
- **`IS NULL`** : Check for NULL
- **`IS NOT NULL`** : Check for non-NULL

```sql
SELECT * FROM employees 
WHERE manager_id IS NULL;
```

## **Advanced Conditions**

### **Subqueries**
```sql
SELECT * FROM products 
WHERE price > (SELECT AVG(price) FROM products);
```

### **EXISTS/NOT EXISTS**
```sql
SELECT * FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.id
);
```

### **CASE Statements in WHERE**
```sql
SELECT * FROM employees 
WHERE 
    CASE 
        WHEN department = 'Sales' THEN salary > 50000
        WHEN department = 'IT' THEN salary > 60000
        ELSE salary > 40000
    END;
```

## **Examples Combined**

```sql
-- Complex condition
SELECT * FROM orders 
WHERE 
    (status = 'Shipped' OR status = 'Processing')
    AND total_amount > 1000
    AND order_date >= '2024-01-01'
    AND customer_id IN (
        SELECT id FROM customers 
        WHERE country = 'USA' AND loyalty_level > 3
    );
```

## **Performance Tips**
1. Use indexes on filtered columns
2. Avoid functions on indexed columns in WHERE clause
3. Use `=` before `LIKE` when possible
4. Consider full-text search for text pattern matching
5. Use `EXPLAIN` to analyze query execution

## **Common Functions in WHERE**
```sql
-- Date functions
WHERE YEAR(order_date) = 2024
WHERE DATEDIFF(day, order_date, GETDATE()) <= 30

-- String functions
WHERE UPPER(name) = 'JOHN'
WHERE LEN(description) > 100

-- Mathematical functions
WHERE ROUND(price, 2) > 99.99
```

The WHERE clause is evaluated after FROM and before GROUP BY, HAVING, and ORDER BY in SQL execution order.
