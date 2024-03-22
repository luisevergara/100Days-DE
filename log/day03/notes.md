# Day 03

*SQL*🫡

## Lessons Learned

- Conditions after aggregating with GROUP BY with the HAVING Clause 
- Updating and Deleting with the statements (UPDATE - SET) and (DELETE - FROM)
- Safeguarding before DELETE using the SELECT *
- The correct use for ALIASING (AS) Columns and Tables
    - When using a JOIN, you can use the * after an Alias to SELECT all the columns of only 1 table. *Demo.\**
- Tips for SQL Interview Questions
    - Practice solving SQL problems in multiple ways to deepen understanding
    - Breakdown complex problems into manageable steps
    - Pre-process data to clean up before addressing complex logic problems
    - Data comparison with one table may require the use of self-joins for efficient analysis
- First Introduction to WINDOWN Functions which applies functions through fragments of the whole data in a specified order and size.
    - OVER()
    - RANK(), DENSE_RANK()
    - ROW_NUMBER()
    - PARTITION BY
    - LEAD(), LAG()
- First Introduction to Subqueries and CTE Tables

## Questions

Can you protect databases somehow from accidental DELETE misfires?

The order in which you write the JOIN statements in SQL matters right? How does the system execute multiple JOIN statements to make the final table?

How does SQL handle Falsey and Truthy Values?

How does subqueries change use cases when used in the SELECT, FROM and WHERE clauses?

Which has better performance, CTE or Subqueries?