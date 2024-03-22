 # Practice Problems

Problem solving session using [InterviewQuery.com](https://www.interviewquery.com/)

### [SELECTive Wine Connoisseur]("https://www.interviewquery.com/questions/selective-wine-connoisseur")

First Solution (Using WHERE CLAUSE)

SELECT wines.id
FROM wines
WHERE 
    alcohol >= 13.00 AND
    ash < 2.4 AND
    color_intensity < 3

Second Solution (Using CTE)

WITH t1 AS (
    SELECT id, alcohol, ash, color_intensity
    FROM wines
    GROUP BY id
    HAVING
        alcohol >= 13.00 AND
        ash < 2.4 AND
        color_intensity < 3
    )
SELECT id
FROM t1

Third Solution (Using Subquery)

SELECT id
FROM (
    SELECT id, alcohol, ash, color_intensity
    FROM wines
    GROUP BY id
    HAVING
        alcohol >= 13.00 AND
        ash < 2.4 AND
        color_intensity < 3
)

### [Total Salary]("https://www.interviewquery.com/questions/total-salary")

First Solution (Using SUM)

SELECT SUM(salary) as total_salary
FROM employees

### [2nd Highest Salary]("https://www.interviewquery.com/questions/2nd-highest-salary")

First Solution (Using CTE)

WITH t1 AS (
    SELECT 
        emp.id, 
        emp.salary, 
        dep.name, 
        DENSE_RANK() OVER (ORDER BY salary DESC) AS rank
    FROM employees emp
    JOIN departments dep
        ON emp.department_id = dep.id
    WHERE dep.name = 'engineering'
)
SELECT salary
FROM t1
WHERE rank = 2

Second Solution (Using DISTINCT and LIMIT)

SELECT DISTINCT emp.salary
FROM employees emp
JOIN departments dep
    ON emp.department_id = dep.id
WHERE dep.name = 'engineering'
ORDER BY emp.salary DESC
LIMIT 1, 1 -- First Value limits how many, Second Value offsets which value

Third Solution (Using Subquery)

SELECT salary
FROM (
    SELECT 
        emp.id, 
        emp.salary, 
        dep.name, 
        DENSE_RANK() OVER (ORDER BY salary DESC) AS rank
    FROM employees emp
    JOIN departments dep
        ON emp.department_id = dep.id
    WHERE dep.name = 'engineering'
) x
WHERE x.rank = 2