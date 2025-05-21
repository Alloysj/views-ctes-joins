# Views, Common Table Expressions (CTEs), and Joins

## Inspect the yum Table (62)
`PRAGMA table_info(yum);`

verify
`SELECT * FROM yum LIMIT 5;`

## Aggregate `yum` Data by Month and Year (63)
```
SELECT 
    strftime('%Y', date) AS year,
    strftime('%m', date) AS month,
    AVG(open) AS avg_open,
    AVG(high) AS avg_high,
    AVG(low) AS avg_low,
    AVG(close) AS avg_close,
    SUM(volume) AS total_volume
FROM yum
GROUP BY year, month
ORDER BY year, month;
```

## Save as a View (64)
```
CREATE VIEW yum_by_month AS
SELECT 
    strftime('%Y', date) AS year,
    strftime('%m', date) AS month,
    AVG(open) AS avg_open,
    AVG(high) AS avg_high,
    AVG(low) AS avg_low,
    AVG(close) AS avg_close,
    SUM(volume) AS total_volume
FROM yum
GROUP BY year, month;
```

### Check if view is created 
`SELECT * FROM yum_by_month LIMIT 5;`



## Create the `trans_by_month` View (65)

```
CREATE VIEW trans_by_month AS
SELECT
    strftime('%Y', transaction_date) AS year,
    strftime('%m', transaction_date) AS month,
    SUM(sales_amount) AS total_sales
FROM transactions
GROUP BY year, month;
```

## `trans_by_employee` View (66)

```
CREATE VIEW trans_by_employee AS
SELECT
    employee_id,
    SUM(sales_amount) AS total_sales
FROM transactions
GROUP BY employee_id;
```

## Find the Most Common First Initial for Pets (67)

```
WITH pet_initials AS (
    SELECT LOWER(SUBSTR(name, 1, 1)) AS first_initial
    FROM pets
)
SELECT first_initial, COUNT(*) AS count
FROM pet_initials
GROUP BY first_initial
ORDER BY count DESC
LIMIT 1;
```

## Create Employee Taglines (68)

```
WITH formatted_employees AS (
    SELECT
        first_name || ' ' || last_name AS full_name,
        CASE
            WHEN job_title = 'IT' THEN job_title
            ELSE LOWER(job_title)
        END AS job_title,
        '$' || printf('%,.2f', salary) AS formatted_salary,
        strftime('%Y', hire_date) AS hire_year
    FROM employees
)
SELECT full_name || ' started in ' || hire_year ||
       ' and makes ' || formatted_salary ||
       ' working in ' || job_title || '.' AS tagline
FROM formatted_employees;
```
## Categorize Companies by Type (69)

```
WITH company_types AS (
    SELECT 
        company_name,
        CASE 
            WHEN company_name LIKE '% LLC' THEN 'LLC'
            WHEN company_name LIKE '% Inc' THEN 'Inc'
            WHEN company_name LIKE '% Ltd' THEN 'Ltd'
            WHEN company_name LIKE '% PLC' THEN 'PLC'
            ELSE 'Other'
        END AS company_type,
        SUM(sales_amount) AS total_sales
    FROM transactions
    GROUP BY company_name
)
SELECT company_type, COUNT(*) AS company_count, SUM(total_sales) AS total_revenue
FROM company_types
GROUP BY company_type;
```

## Find Which Employee Made Each Sale (70)

```
SELECT 
    employees.first_name, 
    employees.last_name, 
    transactions.sales_amount, 
    transactions.transaction_date
FROM employees
JOIN transactions ON employees.employee_id = transactions.employee_id;
```

## Find the Employee with the Highest Total Sales (71)

```
SELECT 
    employees.first_name, 
    employees.last_name, 
    SUM(transactions.sales_amount) AS total_sales
FROM employees
JOIN transactions ON employees.employee_id = transactions.employee_id
GROUP BY employees.employee_id
ORDER BY total_sales DESC
LIMIT 1;
```

## `trans_by_employee` View (72)
```
SELECT 
    employees.first_name, 
    employees.last_name, 
    trans_by_employee.total_sales
FROM employees
JOIN trans_by_employee ON employees.employee_id = trans_by_employee.employee_id
ORDER BY trans_by_employee.total_sales DESC
LIMIT 1;
```

### Solve Using a CTE (73)
```
WITH employee_sales AS (
    SELECT employee_id, SUM(sales_amount) AS total_sales
    FROM transactions
    GROUP BY employee_id
)
SELECT 
    employees.first_name, 
    employees.last_name, 
    employee_sales.total_sales
FROM employees
JOIN employee_sales ON employees.employee_id = employee_sales.employee_id
ORDER BY employee_sales.total_sales DESC
LIMIT 1;
```

### Employees Who Generated Sales 1.5x Their Salary (74)
```
WITH employee_sales AS (
    SELECT employee_id, SUM(sales_amount) AS total_sales
    FROM transactions
    GROUP BY employee_id
)
SELECT 
    employees.first_name, 
    employees.last_name, 
    employees.salary, 
    employee_sales.total_sales
FROM employees
JOIN employee_sales ON employees.employee_id = employee_sales.employee_id
WHERE employee_sales.total_sales > 1.5 * employees.salary;
```
## Transactions That Occurred Before the Employee Was Hired (75)

Check for incorrect records where a transaction date is earlier than the employee's hire date
```
SELECT 
    transactions.transaction_id, 
    transactions.transaction_date, 
    employees.first_name, 
    employees.last_name, 
    employees.hire_date
FROM transactions
JOIN employees ON transactions.employee_id = employees.employee_id
WHERE transactions.transaction_date < employees.hire_date;
```

## Monthly Revenue vs. Yum! Trade Volume (76)

Join `trans_by_month` (company revenue) with `yum_by_month` (Yum! trading volume).
```
SELECT 
    trans_by_month.year, 
    trans_by_month.month, 
    '$' || printf('%,.2f', trans_by_month.total_sales) AS company_revenue, 
    yum_by_month.total_volume AS yum_trade_volume
FROM trans_by_month
JOIN yum_by_month 
ON trans_by_month.year = yum_by_month.year 
AND trans_by_month.month = yum_by_month.month
ORDER BY trans_by_month.year, trans_by_month.month;
```
## Extend the Previous Query with High/Low Prices (77)

```
SELECT 
    trans_by_month.year, 
    trans_by_month.month, 
    '$' || printf('%,.2f', trans_by_month.total_sales) AS company_revenue, 
    yum_by_month.total_volume AS yum_trade_volume, 
    yum_by_month.avg_low AS lowest_price, 
    yum_by_month.avg_high AS highest_price
FROM trans_by_month
JOIN yum_by_month 
ON trans_by_month.year = yum_by_month.year 
AND trans_by_month.month = yum_by_month.month
ORDER BY trans_by_month.year, trans_by_month.month;
```
.

### Run Queries
Run each query in your database to verify results.

If any column names differ in the actual schema, give the corrected schema name for updates
