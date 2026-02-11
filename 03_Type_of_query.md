# Explain Subqueries

## Subtask:
Provide a detailed explanation of what subqueries are, their types (scalar, row, table), and where they can be used in an SQL query (SELECT, FROM, WHERE, HAVING).

# What are Subqueries?

A subquery (also known as an inner query or nested query) is a query embedded within another SQL query. It is typically executed first, and its result is then used by the outer query.

## Purpose:
Subqueries are used to perform complex queries that cannot be easily achieved with a single SELECT statement. They allow you to:
*   Filter data based on the results of another query.
*   Perform calculations using aggregated data from a subset of your data.
*   Generate derived tables for further analysis.
*   Check for the existence of records.

## Types of Subqueries:

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

## General Usage Clauses:
Subqueries can be embedded in various parts of an SQL query:
*   **SELECT clause**: For scalar values.
*   **FROM clause**: As derived tables (table subqueries).
*   **WHERE clause**: For filtering rows (scalar, row, or table subqueries with `IN`, `EXISTS`, `ANY`, `ALL`).
*   **HAVING clause**: For filtering groups (scalar, row, or table subqueries with `IN`, `EXISTS`, `ANY`, `ALL`).

# Explain Correlated Subqueries

## Subtask:
Explain correlated subqueries, highlighting their key difference from regular subqueries (dependency on the outer query) and when to use them.

## Explain Correlated Subqueries

### Subtask:
Explain correlated subqueries, highlighting their key difference from regular subqueries (dependency on the outer query) and when to use them.

### Explanation of Correlated Subqueries

A **correlated subquery** is a subquery that depends on the outer query for its values. This means that the subquery cannot be executed independently of the outer query; it requires information from the outer query to produce its result. Unlike regular (non-correlated) subqueries, which are executed once and their results are used by the outer query, a correlated subquery is typically executed once for *each row* processed by the outer query.

**Key Difference from Regular Subqueries**:
*   **Regular Subquery (Non-Correlated)**: Executes once and provides a result set to the outer query. It's independent of the outer query's rows.
*   **Correlated Subquery**: Executes repeatedly, once for each candidate row of the outer query. It references a column from the outer query, making its execution dependent on the current row being processed by the outer query.

**Execution Flow**:
1.  The outer query starts processing its first row.
2.  For this row, the correlated subquery executes, using a value from the outer query's current row.
3.  The result of the subquery is then used by the outer query to filter or select the current row.
4.  This process repeats for every subsequent row of the outer query.

**When to Use Correlated Subqueries**:
Correlated subqueries are particularly useful for:
*   **Row-by-row processing**: When you need to perform a calculation or comparison for each row of the main query based on related data.
*   **Existence checks**: To determine if a related row exists in another table for each row of the outer query.
*   **Comparisons involving outer query data**: When the subquery's filtering condition relies on a column from the outer query.

**Common Operators**:
Correlated subqueries are often used with operators such as:
*   `EXISTS`
*   `NOT EXISTS`
*   Comparison operators (`=`, `!=`, `<`, `>`, `<=`, `>=`)
*   `IN` (though `IN` can also be used with non-correlated subqueries)

### Conceptual SQL Example:

Let's say we want to find all employees who earn more than the average salary in their respective departments. This requires comparing each employee's salary to the average salary of their specific department, which changes for each department.

```sql
SELECT
    e1.employee_id,
    e1.employee_name,
    e1.salary,
    e1.department_id
FROM
    Employees e1
WHERE
    e1.salary > (
        SELECT
            AVG(e2.salary)
        FROM
            Employees e2
        WHERE
            e2.department_id = e1.department_id -- This is the correlation: references e1.department_id
    );


In this example, the inner query `(SELECT AVG(e2.salary) FROM Employees e2 WHERE e2.department_id = e1.department_id)` calculates the average salary for the department of the `e1` employee currently being considered by the outer query. This inner query runs for each employee in the outer query.

## Explain Correlated Subqueries

### Subtask:
Explain correlated subqueries, highlighting their key difference from regular subqueries (dependency on the outer query) and when to use them.

### Explanation of Correlated Subqueries

A **correlated subquery** is a subquery that depends on the outer query for its values. This means that the subquery cannot be executed independently of the outer query; it requires information from the outer query to produce its result. Unlike regular (non-correlated) subqueries, which are executed once and their results are used by the outer query, a correlated subquery is typically executed once for *each row* processed by the outer query.

**Key Difference from Regular Subqueries**:
*   **Regular Subquery (Non-Correlated)**: Executes once and provides a result set to the outer query. It's independent of the outer query's rows.
*   **Correlated Subquery**: Executes repeatedly, once for each candidate row of the outer query. It references a column from the outer query, making its execution dependent on the current row being processed by the outer query.

**Execution Flow**:
1.  The outer query starts processing its first row.
2.  For this row, the correlated subquery executes, using a value from the outer query's current row.
3.  The result of the subquery is then used by the outer query to filter or select the current row.
4.  This process repeats for every subsequent row of the outer query.

**When to Use Correlated Subqueries**:
Correlated subqueries are particularly useful for:
*   **Row-by-row processing**: When you need to perform a calculation or comparison for each row of the main query based on related data.
*   **Existence checks**: To determine if a related row exists in another table for each row of the outer query.
*   **Comparisons involving outer query data**: When the subquery's filtering condition relies on a column from the outer query.

**Common Operators**:
Correlated subqueries are often used with operators such as:
*   `EXISTS`
*   `NOT EXISTS`
*   Comparison operators (`=`, `!=`, `<`, `>`, `<=`, `>=`)
*   `IN` (though `IN` can also be used with non-correlated subqueries)

### Conceptual SQL Example:

Let's say we want to find all employees who earn more than the average salary in their respective departments. This requires comparing each employee's salary to the average salary of their specific department, which changes for each department.

```sql
SELECT
    e1.employee_id,
    e1.employee_name,
    e1.salary,
    e1.department_id
FROM
    Employees e1
WHERE
    e1.salary > (
        SELECT
            AVG(e2.salary)
        FROM
            Employees e2
        WHERE
            e2.department_id = e1.department_id -- This is the correlation: references e1.department_id
    );
```

In this example, the inner query `(SELECT AVG(e2.salary) FROM Employees e2 WHERE e2.department_id = e1.department_id)` calculates the average salary for the department of the `e1` employee currently being considered by the outer query. This inner query runs for each employee in the outer query.
