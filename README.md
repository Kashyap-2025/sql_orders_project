# SQL Practice Project

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
- `README.md` → project description & usage instructions.  

## 🚀 How to Use
1. Create the `list` and `orders` tables in PostgreSQL.  
2. Run queries from `practice_queries.sql` step by step.  
3. Modify queries and try your own experiments.  

## 📝 Example Queries
```sql
-- Top 3 customers by profit
SELECT l.customername, SUM(o.profit) AS total_profit
FROM list l
JOIN orders o ON l.order_id = o.order_id
GROUP BY l.customername
ORDER BY total_profit DESC
LIMIT 3;
```

## 🎯 Goal
This project helps in **mastering SQL with real-world practice** and serves as a **portfolio project on GitHub**.
