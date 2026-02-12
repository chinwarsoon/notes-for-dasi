# Section A - Introduction
<details>
   <summary> Click to open or close this section </summary>
Think of a database like a massive, multi-story library. Usually, you just ask for a book. But sometimes, you need to do a little "pre-research" before you can find exactly what you're looking for.

Here is how these four concepts help you do that research, explained like you're five.

---

## 1. Subqueries (The "Question Inside a Question")

A **Subquery** is when you ask one question to get an answer that you then use to ask your main question.

* **The Scenario:** You want to find all the kids in class who are taller than "The Average Kid."
* **How it works:** 1.  First, you ask: "What is the average height?" (The Subquery)
2.  The answer comes back: "50 inches."
3.  Then you ask: "Who is taller than 50 inches?" (The Main Query)

> **Key Rule:** The subquery usually runs **once**, finishes, and hands the result to the main query.

---

## 2. Correlated Subquery (The "Check-Back")

This is a subquery that is a bit "co-dependent." It can’t finish its job until it looks at every single row in your main list.

* **The Scenario:** You want to find the fastest runner *in each specific grade*.
* **How it works:**
1. You look at Jimmy in Grade 1.
2. You ask a subquery: "Is Jimmy the fastest *only in Grade 1*?"
3. Then you move to Sarah in Grade 2.
4. You ask the subquery again: "Is Sarah the fastest *only in Grade 2*?"



> **Key Difference:** Unlike a regular subquery, this one runs **over and over again** for every person on your list.

---

## 3. Derived Table (The "Pop-Up Shop")

A **Derived Table** is like a temporary table you build right in the middle of your sentence. It doesn't exist in the library; you just create it for a moment, use it, and then it vanishes.

* **The Scenario:** You have a big pile of messy Legos. You want to find the rarest red piece.
* **How it works:**
1. You grab all the red pieces and put them in a small, temporary box (The Derived Table).
2. You look **only inside that box** to find the rarest one.
3. Once you find it, you throw the box away.



---

## 4. Common Table Expression / CTE (The "Bookmark")

A **CTE** is just like a Derived Table, but it’s much neater. It’s like giving your temporary box a **name** at the very top of your page so you can refer back to it easily.

* **The Scenario:** You’re writing a long story and don't want to keep explaining who "The Secret Club" is.
* **How it works:**
1. At the top of your paper, you write: "Let 'The Secret Club' be the group of kids who own blue bikes."
2. Now, in your main story, you can just say: "Show me the members of The Secret Club."

### Summary Table

| Concept | What is it? | Best described as... |
| --- | --- | --- |
| **Subquery** | A query inside another query. | A "nested" question. |
| **Correlated** | A subquery that refers to the outer query. | A "check-back" loop. |
| **Derived Table** | A temporary table created in the `FROM` clause. | A "disposable" box. |
| **CTE** | A named temporary result at the top. | A "labeled" bookmark. |

To show you how these work in action, let’s imagine a small school database with two tables: **Students** and **Grades**.

</details>

# Section B - Samples
<details>
   <summary> Click to open or close this section </summary>
   
## The Sample Data

**Students Table**
| StudentID | Name | ClassID |
| :--- | :--- | :--- |
| 1 | Alice | A |
| 2 | Bob | A |
| 3 | Charlie | B |

**Grades Table**
| StudentID | Subject | Score |
| :--- | :--- | :--- |
| 1 | Math | 95 |
| 2 | Math | 80 |
| 3 | Math | 70 |

---

## 1. Subquery Example

We want to find students who scored higher than the **average** score of 81.6.

```sql
SELECT Name 
FROM Students 
WHERE StudentID IN (
    SELECT StudentID 
    FROM Grades 
    WHERE Score > 81.6 -- This is the inner "question"
);

```

* **What happened:** The database found the IDs of the high-achievers first, then looked up their names.

---

## 2. Correlated Subquery Example

We want to find students who scored higher than the average **for their specific class**.

```sql
SELECT s.Name
FROM Students s
WHERE 85 < (
    SELECT AVG(Score)
    FROM Grades g
    JOIN Students s2 ON g.StudentID = s2.StudentID
    WHERE s2.ClassID = s.ClassID -- This links the inner query to the outer row
);

```

* **What happened:** For every student in the list, the database "checks back" to calculate the average of just that student's class.

---

## 3. Derived Table Example

We want to treat a list of "Top Scores" as if it were its own real table and join it to our students.

```sql
SELECT s.Name, TopScores.Score
FROM Students s
JOIN (
    SELECT StudentID, Score 
    FROM Grades 
    WHERE Score > 90
) AS TopScores -- This is the "Pop-up Table"
ON s.StudentID = TopScores.StudentID;

```

* **What happened:** We created a mini-table called `TopScores` on the fly and used it just for this one command.

---

## 4. CTE (Common Table Expression) Example

This does the exact same thing as the Derived Table above, but it’s much easier to read because we define the "TopScores" at the very beginning.

```sql
WITH TopScores AS (
    SELECT StudentID, Score 
    FROM Grades 
    WHERE Score > 90
)
SELECT s.Name, ts.Score
FROM Students s
JOIN TopScores ts ON s.StudentID = ts.StudentID;

```

* **What happened:** We bookmarked the `TopScores` logic at the top, making the main `SELECT` statement clean and simple.

</details>

# Section C - Choosing the Query

<details>
   <summary> Click to open or close this section </summary>


Choosing the right tool for the job is usually a trade-off between **speed** (performance) and **clarity** (readability).

Here is a guide to help you judge which one to grab from your toolbox:

---

## 1. Use a Subquery when...

* **The task is simple:** You just need a single value (like an average or a max) to use as a filter.
* **Checking existence:** You want to see if a record exists in another table using `EXISTS` or `IN`.
* **Why:** It’s quick to write and very standard.

## 2. Use a CTE when...

* **Readability is key:** If your query is getting long, move the logic to the top with a `WITH` clause.
* **You need to reuse data:** If you refer to the same temporary result three different times in one query, a CTE lets you define it once.
* **Recursion:** If you are dealing with "family trees" or organizational charts (Recursive CTEs), this is your only option.
* **Why:** It makes your code look like a clean story rather than a messy "Inception" movie of nested brackets.

## 3. Use a Derived Table when...

* **One-off calculations:** You need to group data (using `GROUP BY`) and then immediately join it to another table.
* **Old systems:** Some very old database versions don't support CTEs.
* **Why:** It’s great for "group then join" logic, though CTEs are generally preferred for this now because they are easier to read.

## 4. Use a Correlated Subquery when...

* **Row-by-row logic is unavoidable:** You need to compare a value in a row against a specific subset related *only* to that row (like "find the top price in *this* category").
* **Warning:** Use these sparingly! Because they run "over and over," they can be very slow on large tables.

---

### The Decision Matrix

| If you want... | Use this... |
| --- | --- |
| **Clean, organized code** | CTE |
| **A simple "Yes/No" check** | Subquery |
| **Performance on huge data** | Join or Subquery (Avoid Correlated) |
| **Hierarchical/Tree data** | Recursive CTE |
| **To calculate then Join** | Derived Table or CTE |

---

### A Rule of Thumb for Beginners

If you find yourself putting a query inside another query, ask: **"Is this getting hard to read?"** * If **No**: Keep the **Subquery**.

* If **Yes**: Move it to the top as a **CTE**.

</details>
