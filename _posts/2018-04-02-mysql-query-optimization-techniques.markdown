---
layout: post
title:  "SQL query optimization techniques"
date:   2018-04-02 20:00:00 +0200
categories: coding mysql query optimization
---

SQL query takes too long. Few tips to optimize:

1. **Nullable Columns**
Do not use `NOT IN` when comparing with nullable columns. Use `NOT EXISTS` instead.
Reason: When `NOT IN` is used in the query (even if the query doesn’t return rows with null values), SQL Server will check each result to see if it is null or not. Using `NOT EXISTS` will not do the comparison with nulls.
2. **Stored Procedure Names**
Do not begin your stored procedure’s name with `sp_`.
Reason: When the stored procedure is named `sp_` or `SP_`, SQL Server always checks in the system/master database even if the Owner/Schema name is provided. Providing a name without `SP_` to a stored procedure avoids this unnecessary check in the system/master database in SQL Server.
3. Use **SET NOCOUNT ON**
Use `SET NOCOUNT ON` with DML operations.
Reason: When performing DML operations (i.e. `INSERT, DELETE, SELECT`, and `UPDATE`), SQL Server always returns the number of rows affected. In complex queries with a lot of joins, this becomes a huge performance issue.
Using `SET NOCOUNT ON` will improve performance because it will not count the number of rows affected.
4. Avoid Using **GROUP BY, ORDER BY, and DISTINCT**
Avoid using `GROUP BY, ORDER BY`, and `DISTINCT` as much as possible.
Reason: When using `GROUP BY, ORDER BY`, or `DISTINCT`, SQL Server engine creates a work table and puts the data on the work table. 
After that, it organizes this data in work table as requested by the query, and then it returns the final result.
Use `GROUP BY, ORDER BY`, or `DISTINCT` in your query only when absolutely necessary.
5. **Avoid using functions in predicates**.The index is not used by the database if there is a function on the column. For example:
`SELECT * FROM TABLE1 WHERE UPPER(COL1)='ABC'`
As a result of the function `UPPER()`, the index on COL1 is not used by database optimizers. If the function cannot be avoided in the SQL, you need to create a function-based index in Oracle or generated columns in DB2 to improve performance.
6. **Index all the predicates** in JOIN, WHERE, ORDER BY and GROUP BY clauses.
We typically depend heavily on indexes to improve SQL performance and scalability.
Without proper indexes, SQL queries can cause table scans, which causes either performance or locking problems. It is recommended that all predicate columns be indexed. The exception being where column data has very low cardinality.
7. **Avoid using wildcard (%)** at the beginning of a predicate.The predicate `LIKE '%abc'` causes full table scan. For example:
`SELECT * FROM TABLE1 WHERE COL1 LIKE '%ABC'`
This is a known performance limitation in all databases.
8. **Avoid unnecessary columns** in `SELECT` clause.
Specify the columns in the `SELECT` clause instead of using `SELECT *`. The unnecessary columns places extra loads on the database, which slows down not just the single SQL, but the whole system.
9. **The ORDER BY clause** is mandatory in SQL if the sorted result set is expected.
The `ORDER BY` keyword is used to sort the result-set by specified columns. Without the `ORDER BY` clause, the result set is returned directly without any sorting.
The order is not guaranteed. Be aware of the performance impact of adding the `ORDER BY` clause, as the database needs to sort the result set, resulting in one of the most expensive operations in SQL execution.
10. **Use WITH for subqueries**
`WITH query_name (column_name1, ...) AS(SELECT ...)SELECT ...`
The `WITH` clause is for subquery factoring, also known as common table expressions or CTEs.
The `WITH` query_name clause lets you assign a name to a subquery block. 
You can then reference the subquery block multiple places in the query by specifying query_name. 
For example, Oracle Database optimizes the query by treating the query name as either an inline view or as a temporary table.


More here: [tips from IBM][ibm-link]{:target="_blank" rel="noopener"}.

[ibm-link]: https://www.ibm.com/support/knowledgecenter/en/SSZLC2_7.0.0/com.ibm.commerce.admin.doc/refs/rsdperformanceworkspaces_dup.htm