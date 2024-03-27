# Day 04

*SQL*🫡

## Lessons Learned

- Contrasting PARTITION BY with GROUP BY
    - PARTITION BY divides result sets into partitions for window functions.
    - The GROUP BY statement is going to reduce the number of rows in our output by actually rolling them up and then calculating the sums or averages for each group.
- Create Tables In Memory With CTEs WITH [Table Name] AS (Subquery)
    - You can create multiple CTEs as long as you keep adding commas.
- Difference between CTEs and Temp Tables CREATE TABLE #temp_table ()
    - Mass Insert using a SELECT query
    - Use cases in Stored Procedures and Improving Processing Speeds
- Working with Stored Procedures to reap the benefits of input parameters, reduced network traffic, improved performance, and ease of updates.
    - CREATE PROCEDURE [Name] AS [Subquery]
    - EXEC [Name] @Parameters
    - Modifying a Stored Procedure Opens the ALTER PROCEDURE
- How to use Subqueries in the SELECT, FROM, and WHERE Statement
    - SELECT : Works similar to a PARTITION BY
        - OVER () is a valid way to write a PARTITION BY
    - FROM : Works similar to a CTE or Temp Table
        - It's better to use CTEs or Temp Tables for performance.
    - WHERE : Works similar to a JOIN
        - You can only work with 1 column at a time in the Where Subquery

## Questions

Are Stored Procedures the same as Jobs?

Can all flavors of SQL compare datetime types with an =?

