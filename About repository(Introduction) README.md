## This section covers SQL theoretical questions, ranging from basic to advanced, for interview preparation.

## Basic SQL Questions

**Q1: What is SQL?**

Answer: SQL (Structured Query Language) is a standard programming language used to interact with relational databases. It is used for querying, updating, and managing data stored in a database.

**Q2: What are the types of SQL commands?**

Answer: SQL commands are categorized into:
  - DDL (Data Definition Language): CREATE, ALTER, DROP
  - DML (Data Manipulation Language): SELECT, INSERT, UPDATE, DELETE
  - DCL (Data Control Language): GRANT, REVOKE
  - TCL (Transaction Control Language): COMMIT, ROLLBACK, SAVEPOINT

**Q3: What is a Primary Key?**

Answer: A primary key is a column (or a set of columns) that uniquely identifies each row in a table. It must contain unique values and cannot be null.

**Q4: What is the difference between WHERE and HAVING?**

Answer: WHERE filters rows before any aggregation is applied.
HAVING filters groups after aggregation.

## Intermediate SQL Questions

**Q5: What is a JOIN? List its types.**

Answer: A JOIN is used to combine rows from two or more tables based on a related column. Types of JOINs:
  - INNER JOIN: Returns rows with matching values in both tables.
  - LEFT JOIN: Returns all rows from the left table and matching rows from the right.
  - RIGHT JOIN: Returns all rows from the right table and matching rows from the left.
  - FULL OUTER JOIN: Returns rows when there is a match in either table.
  - CROSS JOIN: Returns the Cartesian product of two tables.

**Q6: Explain the concept of normalization.**

Answer: Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity. Common normal forms are:
  - 1NF: Eliminate duplicate columns and ensure each table has a primary key.
  - 2NF: Ensure all non-key attributes are fully dependent on the primary key.
  - 3NF: Remove transitive dependencies.
    
**Q7: What is a CTE (Common Table Expression)?**

Answer: A CTE is a temporary result set defined within a SQL query using the WITH keyword. It is used for better readability and reusability of query logic.

## Advanced SQL Questions

**Q8: What is a Window Function?**

Answer: A window function performs a calculation across a set of rows related to the current row within a result set. It uses the OVER clause. Common examples include ROW_NUMBER(), RANK(), and SUM().

**Q9: What are Indexes in SQL?**

Answer: Indexes are special lookup tables that improve the speed of data retrieval operations on a database table. Types include:
Clustered Index: Reorders the rows in the table.
Non-Clustered Index: Maintains a separate structure for the index.

**Q10: How would you optimize a slow SQL query?**

Answer: Techniques to optimize SQL queries include:
- Using appropriate indexes.
- Avoiding SELECT * and selecting only required columns.
- Writing efficient WHERE clauses and avoiding unnecessary calculations in conditions.
- Using EXPLAIN or query execution plans to identify bottlenecks.
  
**Q11: What is the difference between DELETE, TRUNCATE, and DROP?**

Answer:
- DELETE: Removes specific rows based on conditions. Can be rolled back.
- TRUNCATE: Removes all rows from a table. Faster than DELETE. Cannot be rolled back.
- DROP: Removes the entire table or database structure.
