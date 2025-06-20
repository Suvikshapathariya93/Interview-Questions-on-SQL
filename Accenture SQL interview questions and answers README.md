## Accenture SQL Interview Questions and Answers

**1. How would you optimize a slow-running query with multiple joins?**
- To optimize a slow-running query with multiple `joins` in `MySQL`, follow these steps:
  - Analyze with `EXPLAIN`: Use `EXPLAIN` to understand the query execution plan, including table access methods, `join` order, and `indexes` being used.
  - Add `Indexes`: Ensure appropriate indexes exist on columns used in `joins` and `WHERE` clauses.
  - Optimize `Join` Order: Reorder `joins` if necessary so that smaller, filtered tables are processed first.
  - Use Selective `WHERE` Clauses: Apply filtering conditions early to reduce the number of rows being joined.
  - Avoid `SELECT`: Retrieve only the columns needed to minimize data handling.
  - Analyze Data Types: Ensure columns in joins have matching data types to avoid unnecessary conversions.
  - Use Derived Tables/`CTEs`: Simplify complex queries by breaking them into smaller parts.
  - Enable `Query` Cache: If data changes infrequently, enabling caching can improve performance.
  - Partition Large Tables: Partitioning can help reduce the number of rows scanned.
  - Review Server Configuration: Optimize `MySQL` settings like `join_buffer_size`, `sort_buffer_size`, and `query_cache_size`.

