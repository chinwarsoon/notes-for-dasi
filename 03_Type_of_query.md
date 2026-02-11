```markdown
# Explain Subqueries

## Subtask:
Provide a detailed explanation of what subqueries are, their types (scalar, row, table), and where they can be used in an SQL query (SELECT, FROM, WHERE, HAVING).

## What are Subqueries?

A subquery (also known as an inner query or nested query) is a query embedded within another SQL query. It is typically executed first, and its result is then used by the outer query.

### Purpose:
Subqueries are used to perform complex queries that cannot be easily achieved with a single SELECT statement. They allow you to:
*   Filter data based on the results of another query.
*   Perform calculations using aggregated data from a subset of your data.
*   Generate derived tables for further analysis.
*   Check for the existence of records.

### Types of Subqueries:

1.  **Scalar Subquery**:
    *   **Definition**: A scalar subquery returns a single value (one row and one column).
    *   **Usage**: It can be used almost anywhere an expression that returns a single value is expected. Common uses include:
        *   **SELECT clause**: To return a single value for each row of the outer query.
        *   **WHERE clause**: To compare a value with the result of the subquery.
        *   **HAVING clause**: Similar to the WHERE clause, but for filtering groups.
        *   **SET clause**: In `UPDATE` statements.
        *   **VALUES clause**: In `INSERT` statements.

    *Example (in WHERE clause):*
    ```sql
    SELECT product_name, price
    FROM products
    WHERE price > (SELECT AVG(price) FROM products);
    ```

2.  **Row Subquery**:
    *   **Definition**: A row subquery returns a single row with one or more columns.
    *   **Usage**: It is primarily used in `WHERE` or `HAVING` clauses for comparison with multiple column values. It often involves comparison operators like `IN`, `ANY`, `ALL`.

    *Example (in WHERE clause with IN):*
    ```sql
    SELECT customer_name
    FROM customers
    WHERE (city, state) IN (SELECT city, state FROM suppliers WHERE supplier_id = 123);
    ```

3.  **Table Subquery**:
    *   **Definition**: A table subquery (or derived table) returns a table (one or more rows and one or more columns).
    *   **Usage**: It is most commonly used in the `FROM` clause of the outer query, where its result is treated as a temporary table. This allows you to perform operations on the result of a subquery as if it were a regular table.

    *Example (in FROM clause):*
    ```sql
    SELECT a.department_name, AVG(b.salary)
    FROM departments a
    JOIN (SELECT employee_id, department_id, salary FROM employees WHERE status = 'Active') b
    ON a.department_id = b.department_id
    GROUP BY a.department_name;
    ```

### General Usage Clauses:
Subqueries can be embedded in various parts of an SQL query:
*   **SELECT clause**: For scalar values.
*   **FROM clause**: As derived tables (table subqueries).
*   **WHERE clause**: For filtering rows (scalar, row, or table subqueries with `IN`, `EXISTS`, `ANY`, `ALL`).
*   **HAVING clause**: For filtering groups (scalar, row, or table subqueries with `IN`, `EXISTS`, `ANY`, `ALL`).
```
