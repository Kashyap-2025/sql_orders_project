# SQL Practice Project 📊

This project is designed to practice **Database concepts, RDBMS, and SQL** queries hands-on using two tables:  
- **list**: stores order details (order_id, date, customer info, location).  
- **orders**: stores transaction details (amount, profit, quantity, category).  

The goal is to cover all major SQL topics with real-world queries.

---

## 📑 Table of Contents
1. [Database & Concepts / DDL](#1-database--concepts--ddl)
2. [DML](#2-dml)
3. [DQL](#3-dql)
4. [WHERE Clause](#4-where-clause)
5. [ORDER BY + LIMIT](#5-order-by--limit)
6. [Operators](#6-operators)
7. [Aggregation](#7-aggregation)
8. [Alias](#8-alias)
9. [GROUP BY + HAVING](#9-group-by--having)
10. [Functions](#10-functions)
11. [Dealing with NULLs](#11-dealing-with-nulls)
12. [CASE Operator](#12-case-operator)
13. [Joins](#13-joins)
14. [TCL](#14-tcl)
15. [DCL](#15-dcl)

---

## SQL Practice Project

This project is a **hands-on SQL practice** based on two tables:  
- `list` → stores order metadata (order_id, date, customer, state, city)  
- `orders` → stores order details (amount, profit, quantity, category, sub_category)

The queries cover **all core SQL concepts**:
1. Database & Concepts / DDL  
2. DML  
3. DQL  
4. WHERE Clause  
5. ORDER BY + LIMIT  
6. Operators  
7. Aggregations  
8. Alias  
9. GROUP BY & HAVING  
10. Functions  
11. Dealing with NULLs  
12. CASE Operator  
13. Joins  
14. TCL  
15. DCL  

## 📂 Files
- `practice_queries.sql` → all queries, organized section-wise with questions.  
- `README.md` → project description, **all questions & queries included**.  

## 🚀 How to Use
1. Create the `list` and `orders` tables in PostgreSQL.  
2. Run queries from `practice_queries.sql` or directly from this README.  
3. Modify queries and try your own experiments.  

---

## 📘 Full Questions and Queries

```sql
-- =========================
-- SECTION 1: Database & DDL
-- =========================
-- Q1: Create a database
CREATE DATABASE salesdb;
\c salesdb;

-- Q2: Create tables (already done in your case)
-- Example structure:
CREATE TABLE list (
    order_id VARCHAR(10) PRIMARY KEY,
    order_date DATE,
    customername VARCHAR(20),
    state VARCHAR(25),
    city VARCHAR(30)
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_id VARCHAR(20) REFERENCES list(order_id) ON DELETE CASCADE,
    amount NUMERIC(8,2),
    profit NUMERIC(8,2),
    quantity SMALLINT,
    category VARCHAR(100),
    sub_category VARCHAR(100)
);

-- =========================
-- SECTION 2: DML
-- =========================
-- Q1: Insert a record in list
INSERT INTO list (order_id, order_date, customername, state, city)
VALUES ('O1001', '2025-01-15', 'Amit Sharma', 'Gujarat', 'Ahmedabad');

-- Q2: Insert a record in orders
INSERT INTO orders (id, order_id, amount, profit, quantity, category, sub_category)
VALUES (1, 'O1001', 1500.00, 250.00, 3, 'Electronics', 'Mobiles');

-- Q3: Update city for a customer
UPDATE list
SET city = 'Surat'
WHERE customername = 'Amit Sharma';

-- Q4: Delete an order by id
DELETE FROM orders WHERE id = 1;

-- =========================
-- SECTION 3: DQL
-- =========================
-- Q1: Select all customers
SELECT * FROM list;

-- Q2: Select all orders
SELECT * FROM orders;

-- =========================
-- SECTION 4: WHERE Clause
-- =========================
-- Q1: Get all orders where profit > 500
SELECT * FROM orders WHERE profit > 500;

-- Q2: Orders from Gujarat customers
SELECT o.*
FROM orders o
JOIN list l ON o.order_id = l.order_id
WHERE l.state = 'Gujarat';

-- =========================
-- SECTION 5: ORDER BY + LIMIT
-- =========================
-- Q1: Top 5 orders by amount
SELECT * FROM orders
ORDER BY amount DESC
LIMIT 5;

-- Q2: Lowest 3 profit orders
SELECT * FROM orders
ORDER BY profit ASC
LIMIT 3;

-- =========================
-- SECTION 6: Operators
-- =========================
-- Q1: Orders with amount between 500 and 2000
SELECT * FROM orders
WHERE amount BETWEEN 500 AND 2000;

-- Q2: Orders with category Electronics or Furniture
SELECT * FROM orders
WHERE category IN ('Electronics', 'Furniture');

-- =========================
-- SECTION 7: Aggregation
-- =========================
-- Q1: Total number of orders
SELECT COUNT(*) AS total_orders FROM orders;

-- Q2: Average order amount
SELECT AVG(amount) AS avg_amount FROM orders;

-- Q3: Highest and lowest profit
SELECT MAX(profit) AS max_profit, MIN(profit) AS min_profit FROM orders;

-- =========================
-- SECTION 8: Alias
-- =========================
-- Q1: Customer and order amount
SELECT l.customername AS customer, o.amount AS order_value
FROM list l JOIN orders o ON l.order_id = o.order_id;

-- Q2: Rename aggregated column
SELECT category, SUM(amount) AS total_sales
FROM orders
GROUP BY category;

-- =========================
-- SECTION 9: GROUP BY + HAVING
-- =========================
-- Q1: Customers with more than 5 orders
SELECT l.customername, COUNT(*) AS total_orders
FROM list l
JOIN orders o ON l.order_id = o.order_id
GROUP BY l.customername
HAVING COUNT(*) > 5;

-- Q2: Categories with avg qty > 10
SELECT category, AVG(quantity) AS avg_qty
FROM orders
GROUP BY category
HAVING AVG(quantity) > 10;

-- =========================
-- SECTION 10: Functions
-- =========================
-- Q1: Max profit order per customer
SELECT l.customername, MAX(o.profit) AS max_profit
FROM list l
JOIN orders o ON l.order_id = o.order_id
GROUP BY l.customername;

-- Q2: Avg amount per sub_category
SELECT sub_category, ROUND(AVG(amount), 2) AS avg_amount
FROM orders
GROUP BY sub_category;

-- Q3: Round order amounts
SELECT id, order_id, ROUND(amount) AS rounded_amount
FROM orders;

-- =========================
-- SECTION 11: Dealing with NULLs
-- =========================
-- Q1: Replace missing city with 'Unknown'
SELECT order_id, COALESCE(city, 'Unknown') AS city
FROM list;

-- Q2: Show orders where city is NULL
SELECT * FROM list WHERE city IS NULL;

-- =========================
-- SECTION 12: CASE Operator
-- =========================
-- Q1: Categorize orders by quantity
SELECT order_id,
       CASE
           WHEN quantity < 5 THEN 'Small Order'
           WHEN quantity BETWEEN 5 AND 15 THEN 'Medium Order'
           ELSE 'Large Order'
       END AS order_size
FROM orders;

-- Q2: Categorize states into regions
SELECT state,
       CASE
           WHEN state IN ('Gujarat','Maharashtra') THEN 'West'
           WHEN state IN ('Delhi','Punjab','Haryana') THEN 'North'
           WHEN state IN ('Tamil Nadu','Kerala','Karnataka') THEN 'South'
           ELSE 'Other'
       END AS region
FROM list
GROUP BY state;

-- =========================
-- SECTION 13: Joins
-- =========================
-- Q1: Customers with order details
SELECT l.customername, l.state, l.city,
       o.order_id, o.amount, o.profit, o.category
FROM list l
JOIN orders o ON l.order_id = o.order_id;

-- Q2: Total sales and profit per customer
SELECT l.customername,
       SUM(o.amount) AS total_sales,
       SUM(o.profit) AS total_profit
FROM list l
JOIN orders o ON l.order_id = o.order_id
GROUP BY l.customername;

-- Q3: Top 3 customers by profit
SELECT l.customername,
       SUM(o.profit) AS total_profit
FROM list l
JOIN orders o ON l.order_id = o.order_id
GROUP BY l.customername
ORDER BY total_profit DESC
LIMIT 3;

-- =========================
-- SECTION 14: TCL
-- =========================
-- Q1: Update then rollback
BEGIN;
UPDATE orders SET profit = profit * 0.95 WHERE category = 'Electronics';
ROLLBACK;

-- Q2: Update then commit
BEGIN;
UPDATE orders SET profit = profit * 0.95 WHERE category = 'Electronics';
COMMIT;

-- =========================
-- SECTION 15: DCL
-- =========================
-- Q1: Grant select to analyst
GRANT SELECT ON orders TO analyst;

-- Q2: Revoke insert from analyst
REVOKE INSERT ON orders FROM analyst;

```

--End of Project
