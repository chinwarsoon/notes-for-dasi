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
```

## 💡 Key Concept: The "Window" Gap
```sql
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

## Use the GROUP BY clause in SQL
You should use GROUP BY clause in SQL when you want to perform aggregate calculations (like SUM, AVG, COUNT, MAX, MIN) on subsets of your data, rather than on the entire dataset. It groups rows that have the same values in specified columns into summary rows.

Here are the main scenarios when you would use GROUP BY:

**1.**   To get summary statistics per category: For example, finding the total sales for each product category, the average salary per department, or the number of flats sold per town (as in our previous example).

**2.**   When using aggregate functions with non-aggregated columns: If your SELECT statement includes both aggregate functions (e.g., COUNT(*)) and non-aggregate columns (e.g., town, month), all non-aggregate columns must be listed in the GROUP BY clause. This tells the database how to group the rows before applying the aggregate function.

**3.**   In combination with HAVING: After GROUP BY creates groups, you can use HAVING to filter those groups based on conditions related to the aggregate functions. For instance, filtering for towns where total sales are greater than a certain amount, as we did in the previous query.

In essence, GROUP BY allows you to break down your data into meaningful segments and then apply aggregate functions to each segment.

The CASE statement in SQL allows you to implement if-then-else logic directly within your queries. It's incredibly useful for defining different outputs based on various conditions.

## How to Use CASE
### The basic syntax for a CASE statement is:

CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE resultN -- Optional: if none of the above conditions are met
END AS new_column_name

WHEN condition THEN result: You can have multiple WHEN clauses. If condition is true, result is returned.
ELSE resultN: This is optional. If no WHEN condition is met, the value specified in ELSE is returned. If ELSE is omitted and no WHEN condition is met, NULL is returned.
END: This marks the end of the CASE statement.
AS new_column_name: You typically give the result of the CASE statement an alias to make it a named column in your output.

### Where to Use CASE
You can use CASE in various parts of an SQL query:

SELECT clause: To create new columns or modify existing column values based on conditions (most common use).
WHERE clause: To apply conditional filtering.
ORDER BY clause: To define custom sorting logic.
GROUP BY clause: To group data based on custom categories.

### Query Sequence with CASE
CASE statements are evaluated within the clause they are part of. So, the overall query sequence remains the same (FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT), but the CASE logic is applied at the appropriate stage.
