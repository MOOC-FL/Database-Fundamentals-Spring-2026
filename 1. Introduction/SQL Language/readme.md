# SQL Language: Comprehensive Guide

SQL (Structured Query Language) is the standard language for managing and manipulating relational databases. Let me provide you with a comprehensive overview of SQL, from basics to advanced concepts.

## **SQL Language Fundamentals**

### 1. **SQL Categories**

SQL statements are divided into several categories:

#### **DDL (Data Definition Language)**
Defines and modifies database structure:
- `CREATE` - Creates database objects (tables, indexes, views)
- `ALTER` - Modifies existing database objects
- `DROP` - Removes database objects
- `TRUNCATE` - Removes all records from a table
- `RENAME` - Renames database objects

#### **DML (Data Manipulation Language)**
Manipulates data within tables:
- `SELECT` - Retrieves data from database
- `INSERT` - Adds new data to tables
- `UPDATE` - Modifies existing data
- `DELETE` - Removes data from tables

#### **DCL (Data Control Language)**
Controls access to data:
- `GRANT` - Gives user privileges
- `REVOKE` - Removes user privileges

#### **TCL (Transaction Control Language)**
Manages transactions:
- `COMMIT` - Saves changes permanently
- `ROLLBACK` - Undoes changes
- `SAVEPOINT` - Sets a point in transaction to roll back to
- `BEGIN` or `START TRANSACTION` - Begins a transaction

## **Core SQL Syntax**

### **Basic SELECT Statement**
```sql
-- Select all columns from a table
SELECT * FROM employees;

-- Select specific columns
SELECT first_name, last_name, salary FROM employees;

-- Select with column aliases
SELECT 
    first_name AS "First Name",
    last_name AS "Last Name",
    salary * 12 AS "Annual Salary"
FROM employees;
```

### **WHERE Clause (Filtering)**
```sql
SELECT * FROM employees WHERE department = 'Sales';

-- Comparison operators
SELECT * FROM employees WHERE salary > 50000;

-- Multiple conditions
SELECT * FROM employees 
WHERE department = 'Sales' AND salary > 60000;

-- IN operator
SELECT * FROM employees WHERE department IN ('Sales', 'Marketing');

-- LIKE operator (pattern matching)
SELECT * FROM employees WHERE first_name LIKE 'J%';  -- Starts with J
SELECT * FROM employees WHERE email LIKE '%@gmail.com';  -- Ends with @gmail.com

-- BETWEEN operator
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 80000;

-- IS NULL / IS NOT NULL
SELECT * FROM employees WHERE manager_id IS NULL;
```

### **ORDER BY (Sorting)**
```sql
SELECT first_name, last_name, salary 
FROM employees 
ORDER BY salary DESC;  -- Descending order

-- Multiple sort criteria
SELECT first_name, last_name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

### **LIMIT and OFFSET (Pagination)**
```sql
-- Get first 10 records
SELECT * FROM employees LIMIT 10;

-- Pagination: skip first 20, get next 10
SELECT * FROM employees LIMIT 10 OFFSET 20;

-- Alternative syntax (some databases)
SELECT * FROM employees OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
```

## **Data Definition Language (DDL)**

### **Creating Database Objects**
```sql
-- Create a database (SQLite uses file, others use CREATE DATABASE)
-- MySQL/PostgreSQL:
CREATE DATABASE company;

-- Create a table
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    hire_date DATE DEFAULT CURRENT_DATE,
    salary DECIMAL(10, 2),
    department_id INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- Create an index
CREATE INDEX idx_employees_department ON employees(department_id);

-- Create a view
CREATE VIEW high_earners AS
SELECT * FROM employees WHERE salary > 100000;
```

### **Modifying Tables**
```sql
-- Add a column
ALTER TABLE employees ADD COLUMN phone_number VARCHAR(15);

-- Modify a column (syntax varies by database)
-- PostgreSQL:
ALTER TABLE employees ALTER COLUMN salary TYPE DECIMAL(12, 2);

-- MySQL:
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);

-- Rename a column
ALTER TABLE employees RENAME COLUMN phone_number TO phone;

-- Drop a column
ALTER TABLE employees DROP COLUMN phone;
```

### **Removing Objects**
```sql
-- Delete all rows but keep structure (DDL, not DML!)
TRUNCATE TABLE employees;

-- Drop a table
DROP TABLE employees;

-- Drop with cascade (removes dependent objects)
DROP TABLE employees CASCADE;

-- Drop a database
DROP DATABASE company;
```

## **Data Manipulation Language (DML)**

### **INSERT Statements**
```sql
-- Insert single row
INSERT INTO employees (first_name, last_name, email, salary)
VALUES ('John', 'Doe', 'john.doe@email.com', 75000);

-- Insert multiple rows
INSERT INTO employees (first_name, last_name, email, salary) VALUES
('Jane', 'Smith', 'jane.smith@email.com', 65000),
('Bob', 'Johnson', 'bob.johnson@email.com', 82000),
('Alice', 'Williams', 'alice.williams@email.com', 55000);

-- Insert from another table
INSERT INTO archive_employees 
SELECT * FROM employees WHERE hire_date < '2010-01-01';
```

### **UPDATE Statements**
```sql
-- Update all rows
UPDATE employees SET salary = salary * 1.05;  -- 5% raise for everyone

-- Update with condition
UPDATE employees 
SET salary = salary * 1.10
WHERE department = 'Engineering';

-- Update multiple columns
UPDATE employees
SET 
    salary = salary * 1.07,
    last_review = CURRENT_DATE
WHERE performance_rating = 'Excellent';
```

### **DELETE Statements**
```sql
-- Delete all rows (DML, slower than TRUNCATE for large tables)
DELETE FROM employees;

-- Delete with condition
DELETE FROM employees WHERE department = 'Temp';

-- Delete using subquery
DELETE FROM employees 
WHERE id IN (
    SELECT id FROM inactive_employees
);
```

## **Joins**

### **INNER JOIN**
```sql
-- Join two tables
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- Join multiple tables
SELECT 
    e.first_name,
    e.last_name,
    d.department_name,
    l.city
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
INNER JOIN locations l ON d.location_id = l.id;
```

### **OUTER JOINs**
```sql
-- LEFT JOIN (all employees, even without department)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- RIGHT JOIN (all departments, even without employees)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

-- FULL OUTER JOIN (all employees and all departments)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

### **CROSS JOIN and SELF JOIN**
```sql
-- CROSS JOIN (Cartesian product)
SELECT 
    e.first_name AS employee,
    m.first_name AS manager
FROM employees e
CROSS JOIN employees m;

-- SELF JOIN
SELECT 
    e.first_name AS employee,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

## **Aggregate Functions and GROUP BY**

### **Common Aggregate Functions**
```sql
-- Count rows
SELECT COUNT(*) FROM employees;

-- Count non-null values
SELECT COUNT(department_id) FROM employees;

-- Sum values
SELECT SUM(salary) AS total_salary FROM employees;

-- Average
SELECT AVG(salary) AS average_salary FROM employees;

-- Minimum and maximum
SELECT 
    MIN(salary) AS lowest_salary,
    MAX(salary) AS highest_salary
FROM employees;
```

### **GROUP BY**
```sql
-- Group by department
SELECT 
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- GROUP BY with multiple columns
SELECT 
    department,
    job_title,
    COUNT(*) AS count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department, job_title;

-- HAVING clause (filter groups)
SELECT 
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(salary) > 50000;
```

## **Subqueries**

### **Scalar Subqueries**
```sql
-- Subquery in SELECT
SELECT 
    first_name,
    last_name,
    salary,
    (SELECT AVG(salary) FROM employees) AS company_avg
FROM employees;

-- Subquery in WHERE
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### **Correlated Subqueries**
```sql
-- Employees earning more than their department average
SELECT 
    e1.first_name,
    e1.last_name,
    e1.salary,
    e1.department
FROM employees e1
WHERE salary > (
    SELECT AVG(salary) 
    FROM employees e2 
    WHERE e2.department = e1.department
);
```

### **IN, EXISTS, and Derived Tables**
```sql
-- IN with subquery
SELECT * FROM employees
WHERE department_id IN (
    SELECT id FROM departments WHERE location = 'New York'
);

-- EXISTS
SELECT * FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e 
    WHERE e.department_id = d.id AND e.salary > 100000
);

-- Derived table (subquery in FROM)
SELECT 
    dept_name,
    employee_count
FROM (
    SELECT 
        d.department_name AS dept_name,
        COUNT(e.id) AS employee_count
    FROM departments d
    LEFT JOIN employees e ON d.id = e.department_id
    GROUP BY d.department_name
) AS dept_stats
WHERE employee_count > 0;
```

## **Advanced SQL Features**

### **Window Functions**
```sql
-- ROW_NUMBER, RANK, DENSE_RANK
SELECT 
    first_name,
    last_name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank,
    RANK() OVER (ORDER BY salary DESC) AS company_rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_company_rank
FROM employees;

-- Running totals and moving averages
SELECT 
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total,
    AVG(amount) OVER (ORDER BY order_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS weekly_avg
FROM orders;

-- LAG and LEAD (access previous/next rows)
SELECT 
    month,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY month) AS prev_month_revenue,
    LEAD(revenue, 1) OVER (ORDER BY month) AS next_month_revenue
FROM monthly_sales;
```

### **Common Table Expressions (CTEs)**
```sql
-- Basic CTE
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 100000
)
SELECT 
    department,
    COUNT(*) AS high_earner_count
FROM high_earners
GROUP BY department;

-- Recursive CTE (for hierarchical data)
WITH RECURSIVE employee_hierarchy AS (
    -- Anchor member
    SELECT 
        id,
        first_name,
        last_name,
        manager_id,
        1 AS level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive member
    SELECT 
        e.id,
        e.first_name,
        e.last_name,
        e.manager_id,
        eh.level + 1
    FROM employees e
    INNER JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy ORDER BY level, last_name;
```

### **UNION, INTERSECT, EXCEPT**
```sql
-- UNION (combine results, removes duplicates)
SELECT first_name, last_name FROM current_employees
UNION
SELECT first_name, last_name FROM former_employees;

-- UNION ALL (keeps duplicates)
SELECT first_name, last_name FROM employees_us
UNION ALL
SELECT first_name, last_name FROM employees_europe;

-- INTERSECT (common rows)
SELECT customer_id FROM online_orders
INTERSECT
SELECT customer_id FROM in_store_purchases;

-- EXCEPT (rows in first but not second)
SELECT product_id FROM all_products
EXCEPT
SELECT product_id FROM discontinued_products;
```

## **SQLite-Specific Features**

```sql
-- AUTOINCREMENT (SQLite specific)
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL
);

-- INSERT OR REPLACE (upsert)
INSERT OR REPLACE INTO employees (id, first_name, last_name, salary)
VALUES (1, 'John', 'Updated', 85000);

-- INSERT OR IGNORE
INSERT OR IGNORE INTO departments (id, name) VALUES (1, 'Engineering');

-- Date and time functions
SELECT 
    DATE('now') AS today,
    DATETIME('now', '+1 day') AS tomorrow,
    STRFTIME('%Y-%m', 'now') AS year_month;

-- JSON support (SQLite 3.9+)
SELECT 
    json_extract(data, '$.name') AS name,
    json_extract(data, '$.age') AS age
FROM users;

-- Full-text search (FTS5)
CREATE VIRTUAL TABLE documents USING fts5(title, content);
INSERT INTO documents VALUES ('SQL Guide', 'This is a guide to SQL...');
SELECT * FROM documents WHERE documents MATCH 'sql guide';
```

## **Best Practices and Performance Tips**

### **1. Indexing**
```sql
-- Create indexes on frequently queried columns
CREATE INDEX idx_employees_department ON employees(department_id);
CREATE INDEX idx_employees_name ON employees(last_name, first_name);
CREATE UNIQUE INDEX idx_employees_email ON employees(email);

-- Composite indexes for multi-column queries
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date DESC);

-- Partial indexes (SQLite, PostgreSQL)
CREATE INDEX idx_high_salary ON employees(salary) WHERE salary > 100000;
```

### **2. Query Optimization**
```sql
-- Use EXPLAIN to see query plan
EXPLAIN QUERY PLAN 
SELECT * FROM employees WHERE department = 'Sales';

-- Be specific in SELECT (avoid SELECT *)
SELECT id, first_name, last_name FROM employees;

-- Use EXISTS instead of IN for large datasets
-- Instead of:
SELECT * FROM customers WHERE id IN (SELECT customer_id FROM orders);

-- Use:
SELECT * FROM customers c 
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);
```

### **3. Transactions**
```sql
-- Always use transactions for multiple operations
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- Only commit if both succeed
COMMIT;

-- Or rollback on error
-- ROLLBACK;
```

## **Sample Database Schema with Queries**

```sql
-- Complete example schema
CREATE TABLE departments (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);

CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    hire_date DATE,
    salary DECIMAL(10, 2),
    department_id INTEGER,
    manager_id INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(id),
    FOREIGN KEY (manager_id) REFERENCES employees(id)
);

CREATE TABLE projects (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    budget DECIMAL(12, 2),
    start_date DATE,
    end_date DATE
);

CREATE TABLE employee_projects (
    employee_id INTEGER,
    project_id INTEGER,
    role VARCHAR(50),
    hours_worked DECIMAL(5, 2),
    PRIMARY KEY (employee_id, project_id),
    FOREIGN KEY (employee_id) REFERENCES employees(id),
    FOREIGN KEY (project_id) REFERENCES projects(id)
);

-- Complex query example
SELECT 
    d.name AS department,
    e.first_name || ' ' || e.last_name AS employee_name,
    COUNT(DISTINCT ep.project_id) AS project_count,
    SUM(ep.hours_worked) AS total_hours,
    AVG(e.salary) OVER (PARTITION BY d.id) AS dept_avg_salary
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
LEFT JOIN employee_projects ep ON e.id = ep.employee_id
WHERE e.hire_date >= '2020-01-01'
GROUP BY d.id, e.id
HAVING COUNT(DISTINCT ep.project_id) > 0
ORDER BY d.name, e.last_name;
```

## **Learning Resources**

1. **Practice Platforms:**
   - SQLZoo (https://sqlzoo.net/)
   - LeetCode SQL Problems
   - HackerRank SQL Track
   - Mode Analytics SQL Tutorial

2. **Reference Documentation:**
   - SQLite: https://www.sqlite.org/lang.html
   - PostgreSQL: https://www.postgresql.org/docs/
   - MySQL: https://dev.mysql.com/doc/

3. **Interactive Learning:**
   - SQL Fiddle (test queries online)
   - DB Fiddle (supports multiple databases)
   - SQLite Online (https://sqliteonline.com/)

SQL is a powerful language that becomes more intuitive with practice. Start with basic SELECT queries and gradually work your way up to complex joins, subqueries, and window functions. The key is consistent practice with real-world scenarios!
