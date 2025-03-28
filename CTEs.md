# SQL Assignment: Views, Common Table Expressions (CTEs), and Joins

This assignment contains exercises on Views, Common Table Expressions (CTEs), and Joins. These topics are more complex than previous exercises, so reviewing the following tutorials is recommended:

- [Tutorial on Views](https://www.sqlitetutorial.net/sqlite-create-view/)
- [Tutorial on CTEs](https://www.essentialsql.com/introduction-common-table-expressions-ctes/)
- [Tutorial on Joins](https://www.sqlitetutorial.net/sqlite-join/)

## Important Notes
- Some queries may be longer than 15 lines, so ensure your code is formatted for readability.
- Read each question carefully.
- Complete problems sequentially, as some rely on previous solutions.
- Save your answers as you progress.
- Some `Views` exercises require persistent database changes. Use `DROP VIEW` or rerun `seed.py` to reset the database if needed.
- To check existing tables and views, use `.tables` in `sqlite3`.

---

## Section 1: Views

**62)** Examine the `yum` table, which contains Yum! Brands, Inc. stock data from 2015 to 2019.

**63)** Query the `yum` table, aggregating by both **month** and **year** with the following columns:
   - Year (4 digits)
   - Month
   - Average open, high, low, and close prices
   - Total volume
   - Sort the results chronologically.

**64)** Save the results from the previous query as a view named `yum_by_month`.

**65)** Create a view named `trans_by_month` from the `transactions` table with the following columns:
   - Year
   - Month
   - Total sales for that month

**66)** Create a view named `trans_by_employee` from the `transactions` table with:
   - `employee_id`
   - Total sales corresponding to that employee

---

## Section 2: Common Table Expressions (CTEs)

CTEs help simplify queries and prevent redundancy. Unlike views, CTEs are temporary and disappear after the query runs.

**67)** Find the most common first initial for pets in the `pets` table.
   - Use a CTE to extract the **lowercased** first letter of each pet's name.
   - Use `SUBSTR()` and `LOWER()` functions.
   - Perform a `GROUP BY` on this CTE.

**68)** Generate taglines for employees using the `employees` table.
   - Format: `"Christine Thompson started in 2005 and makes $123,696 working in sales."`
   - Use a CTE with:
     - Full name (`firstname + " " + lastname`)
     - Job title (lowercase unless "IT")
     - Salary (formatted)
     - Year started
   - Perform string concatenation on this CTE.

**69)** Categorize sales based on company types (`LLC`, `Inc`, `Ltd`, `PLC`, or `Other`).
   - Use a CTE to create a `company_type` column.
   - Use `INSTR()` function to classify company names.
   - Outside the CTE, compute total revenue and transaction counts for each category.

---

## Section 3: Joins

This section integrates joins with views and CTEs for more advanced queries.

**70)** Identify which employee made each sale.
   - Join `employees` with `transactions` on `employee_id`.
   - Include `first_name` and `last_name` from `employees`.

**71)** Find the employee who generated the highest total sales.
   - Use a join similar to the previous problem.
   - The query will be complex without additional structuring.

**72)** Solve the previous problem using a join with the `trans_by_employee` view.

**73)** Solve the previous problem using a join with a CTE.

**74)** Identify employees who earned sales exceeding **1.5 times** their salary.
   - Use a view, CTE, or subquery for the join.

**75)** Find transactions that occurred before the employee was hired.
   - Ensure each transaction is listed in a single row.

**76)** Create a table of **monthly company revenue vs. Yum! trade volume** (2015-2019):
   - Columns: `year`, `month`, `company_revenue`, `yum_trade_volume`
   - Example row:
     ```
     | year | month | company_revenue | yum_trade_volume |
     |------|-------|-----------------|------------------|
     | 2017 | 03    | $100,000        | 125,000,000      |
     ```
   - No `WHERE` clause needed; the correct join type will produce the desired result.

**77)** Extend the previous problem to include:
   - **Lowest price** in that month
   - **Highest price** in that month

---

### Submission Guidelines
Ensure your SQL scripts are properly formatted and saved progressively. Good luck!

