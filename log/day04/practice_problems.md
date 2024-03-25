 # Practice Problems

### [Distance Traveled]("https://www.interviewquery.com/questions/distance-traveled")

First Solution using COALESCE()

SELECT u.name, COALESCE(SUM(distance), 0) AS distance_traveled
FROM users u
LEFT JOIN rides r
    ON u.id = r.passenger_user_id
GROUP BY u.name
ORDER BY distance_traveled DESC

SECOND SOLUTION using IFNULL()

SELECT u.name, IFNULL(SUM(distance), 0) AS distance_traveled
FROM users u
LEFT JOIN rides r
    ON u.id = r.passenger_user_id
GROUP BY u.name
ORDER BY distance_traveled DESC

### [Customs Orders]("https://www.interviewquery.com/questions/customer-orders")

First Solution using HAVING

SELECT DISTINCT u.name as customer_name
FROM transactions t
JOIN users u
    ON t.user_id = u.id
GROUP BY t.user_id, EXTRACT(YEAR from created_at)
HAVING COUNT(EXTRACT(YEAR from created_at) = 2019) > 3
        AND 
        COUNT(EXTRACT(YEAR from created_at) = 2020) > 3

Second Solution using CTE

WITH cte AS (
    SELECT id, user_id, CAST(created_at AS YEAR) as year
    FROM transactions
),
t1 AS (
    SELECT DISTINCT user_id
    FROM cte
    WHERE year = 2019
    GROUP BY user_id
    HAVING COUNT(id)>3
),
t2 AS (
    SELECT DISTINCT user_id
    FROM cte
    WHERE year = 2020
    GROUP BY user_id
    HAVING COUNT(id)>3
)
SELECT name as customer_name
FROM users
WHERE id IN (SELECT * FROM t1) AND id IN (SELECT * FROM t2)

### [Upsell Transactions]("https://www.interviewquery.com/questions/upsell-transactions")

First Solution with CTE

WITH CTE AS (
    SELECT 
        user_id
        , DATE(created_at) AS date
    FROM transactions
    GROUP BY user_id, created_at
),
t2 AS (
    SELECT COUNT(user_id) as x
    FROM CTE
    GROUP BY user_id
    HAVING x > 1
)
SELECT COUNT(*) as num_of_upsold_customers
FROM t2
