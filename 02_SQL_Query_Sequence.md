# 📊 SQL Execution Order Cheat Sheet (Extended Version)

This guide explains the logical sequence a SQL engine follows. Use this to troubleshoot why aliases or window functions (like `RANK()`) might be causing "Column Not Found" errors.

---

## 🏗️ The Logical Order of Execution

Even though we write `SELECT` first, the database executes it nearly last.

| Sequence | Clause | Purpose |
| :--- | :--- | :--- |
| **1** | `FROM` / `JOIN` | Identifies the source tables and merges them. |
| **2** | `WHERE` | Filters raw rows **before** any grouping happens. |
| **3** | `GROUP BY` | Clusters rows into buckets based on shared values. |
| **4** | `HAVING` | Filters the **buckets** (groups) created above. |
| **5** | **WINDOW** | Calculates `RANK()`, `ROW_NUMBER()`, and `PARTITION BY`. |
| **6** | `SELECT` | Pulls the final columns and applies aliases. |
| **7** | `DISTINCT` | Removes duplicate rows from the results. |
| **8** | `ORDER BY` | Sorts the final result set for display. |
| **9** | `LIMIT` / `OFFSET` | Constraints the number of rows returned. |

---

## 🔍 Understanding PARTITION BY

Within Step 5, `PARTITION BY` acts like a "Mini-Group By." It tells the window function to look at slices of the data rather than the whole thing.



### Example: Rank per Category
If you want to find the top 2 most expensive products in *every* category:

```sql
SELECT 
    category, 
    product_name, 
    price,
    RANK() OVER(PARTITION BY category ORDER BY price DESC) as rank_in_cat
FROM products;

## 💡 Key Concept: The "Window" Gap

A common mistake is trying to filter by a `RANK()` in the `WHERE` clause. 

**Why it fails:** The `WHERE` clause (Step 2) finishes its job before the `RANK()` (Step 5) is even calculated.
```

### ❌ The Wrong Way
```sql
SELECT name, RANK() OVER(ORDER BY sales DESC) as sales_rank
FROM employees
WHERE sales_rank <= 5; -- 🛑 ERROR: sales_rank does not exist yet!
```

###  ✅ The Right Way (Using a CTE)
To filter by a rank, you must "wrap" the ranking logic so it completes before the filter is applied.
```sql
WITH RankedData AS (
    SELECT name, RANK() OVER(ORDER BY sales DESC) as sales_rank
    FROM employees
)
SELECT * FROM RankedData 
WHERE sales_rank <= 5; -- ✅ WORKS: The rank was built inside the CTE.
```
