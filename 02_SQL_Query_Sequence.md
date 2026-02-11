# 📊 SQL Execution Order Cheat Sheet

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
| **5** | **WINDOW** | Calculates `RANK()`, `ROW_NUMBER()`, etc. |
| **6** | `SELECT` | Pulls the final columns and applies aliases. |
| **7** | `DISTINCT` | Removes duplicate rows from the results. |
| **8** | `ORDER BY` | Sorts the final result set for display. |
| **9** | `LIMIT` / `OFFSET` | Constraints the number of rows returned. |

---

## 💡 Key Concept: The "Window" Gap

A common mistake is trying to filter by a `RANK()` in the `WHERE` clause. 

**Why it fails:** The `WHERE` clause (Step 2) finishes its job before the `RANK()` (Step 5) is even calculated.

### ❌ The Wrong Way
```sql
SELECT name, RANK() OVER(ORDER BY sales DESC) as sales_rank
FROM employees
WHERE sales_rank <= 5; -- 🛑 ERROR: sales_rank does not exist yet!
