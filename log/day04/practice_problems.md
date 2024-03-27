# Practice Problems

## Interview Query Site

### [Largest Salary By Department]("https://www.interviewquery.com/questions/largest-salary-by-department")

First Solution using GROUP BY

SELECT department, MAX(salary) AS largest_salary
FROM employees emp
GROUP BY department

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

## [SQL Practice]("https://www.sql-practice.com/")

Answers

### EASY

SELECT
  first_name,
  last_name,
  gender
FROM patients
WHERE gender = 'M';

SELECT
  first_name,
  last_name
FROM patients
WHERE allergies IS NULL;

SELECT
  first_name
FROM patients
WHERE first_name LIKE "C%";

SELECT
  first_name,
  last_name
FROM patients
WHERE weight between 100 AND 120;

UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;

/*  
SELECT concat(first_name, ' ', last_name) AS full_name
FROM patients;

SELECT first_name || ' ' || last_name
FROM patients;  
*/

SELECT pat.first_name, pat.last_name, prov.province_name
FROM patients pat
JOIN province_names prov
	ON pat.province_id = prov.province_id;

/*  
SELECT total_2010_birthdays
FROM (
SELECT YEAR(birth_date) as years, count(*) AS total_2010_birthdays
FROM patients
group by years
HAVING years = 2010)

SELECT COUNT(*) AS total_patients
FROM patients
WHERE YEAR(birth_date) = 2010;  
*/

/*  
SELECT first_name, last_name, height
FROM patients
order by height DESC
LIMIT 1;

SELECT
  first_name,
  last_name,
  MAX(height) AS height
FROM patients;

SELECT
  first_name,
  last_name,
  height
FROM patients
WHERE height = (
    SELECT max(height)
    FROM patients
  )  
*/

SELECT *
FROM patients
WHERE patient_id IN (1, 45, 534, 879, 1000);

SELECT count(*) as total_admissions
FROM admissions;

SELECT *
from admissions
WHERE admission_date = discharge_date;

/*  
SELECT patient_id, count(*) as total_admissions
from admissions
group by patient_id
HAVING patient_id = 579;

SELECT
  patient_id,
  COUNT(*) AS total_admissions
FROM admissions
WHERE patient_id = 579;  
*/

SELECT distinct city as unique_city
FROM patients
WHERE province_id = 'NS';

SELECT first_name, last_name, birth_date
FROM patients
WHERE height > 160 AND weight > 70;

SELECT first_name, last_name, allergies
FROM patients
WHERE allergies IS NOT NULL AND city = 'Hamilton';

### MEDIUM

SELECT DISTINCT YEAR(birth_date) as birth_year
FROM patients
order by birth_year;

/*  
SELECT first_name
FROM
(SELECT first_name, count(first_name) as total_times
FROM patients
group by first_name)
WHERE total_times = 1;

SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(first_name) = 1;
*/

SELECT patient_id, first_name
FROM patients
WHERE first_name LIKE 's%s' AND LENGTH(first_name) >= 6;

/*  
SELECT pat.patient_id, pat.first_name, pat.last_name
FROM patients pat
JOIN admissions adm
	ON pat.patient_id = adm.patient_id
WHERE adm.diagnosis = 'Dementia';

SELECT
  patient_id,
  first_name,
  last_name
FROM patients
WHERE patient_id IN (
    SELECT patient_id
    FROM admissions
    WHERE diagnosis = 'Dementia'
  );  
*/

SELECT first_name
FROM patients
ORDER BY LEN(first_name), first_name ASC;

/*  
select (select COUNT(\*) FROM patients WHERE gender = 'M') as male_count, (SELECT count(\*) FROM patients where gender = 'F') as female_count
FROM patients
LIMIT 1;

SELECT 
  (SELECT count(*) FROM patients WHERE gender='M') AS male_count, 
  (SELECT count(*) FROM patients WHERE gender='F') AS female_count;  
*/

/*  
SELECT 
	first_name,
    last_name,
    allergies
FROM patients
WHERE allergies = 'Penicillin' OR allergies = 'Morphine'
order by allergies, first_name, last_name

SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE
  allergies IN ('Penicillin', 'Morphine')
ORDER BY
  allergies,
  first_name,
  last_name;  
*/

/*  
SELECT patient_id, diagnosis
FROM
(SELECT patient_id, diagnosis, COUNT(*) as num_admissions
FROM admissions
group by patient_id, diagnosis
ORDER BY num_admissions desc)
WHERE num_admissions > 1;

SELECT
  patient_id,
  diagnosis
FROM admissions
GROUP BY
  patient_id,
  diagnosis
HAVING COUNT(*) > 1;  
*/

SELECT city, COUNT(*) as total_number_patients
FROM patients
group by city
order by total_number_patients DESC, city ASC;

/*  
WITH t1 AS (SELECT first_name, last_name, 'Doctor' as role
FROM doctors),
t2 AS (SELECT first_name, last_name, 'Patient' AS role
       FROM patients)
SELECT * FROM t1
UNION ALL
SELECT * FROM t2

SELECT first_name, last_name, 'Patient' as role FROM patients
    union all
select first_name, last_name, 'Doctor' from doctors;  
*/

/*  
select allergies, count(*) as popularity
FROM patients
group by allergies
HAVING allergies IS NOT NULL
ORDER BY popularity DESC;

SELECT
  allergies,
  COUNT(*) AS total_diagnosis
FROM patients
WHERE
  allergies IS NOT NULL
GROUP BY allergies
ORDER BY total_diagnosis DESC  
*/

/*  
select first_name, last_name, birth_date
FROM patients
WHERE year(birth_date) between 1970 AND 1979
order by birth_date;

SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE year(birth_date) LIKE '197%'
ORDER BY birth_date ASC  
*/

select concat(upper(last_name), ',', lower(first_name)) as new_name_format
from patients
ORDER BY first_name DESC;

select province_id, Sum(height) as total_height
FROM patients
group by province_id
having total_height >= 7000;

SELECT Max(weight) - min(weight) as weight_diff
FROM patients
WHERE last_name = 'Maroni';

SELECT DAY(admission_date) as day_number, count(*) as number_of_admissions
FROM admissions
group by day_number
order by number_of_admissions DESC;

/*  
SELECT *
FROM admissions
WHERE patient_id = '542'
ORDER BY admission_date DESC
LIMIT 1;

SELECT *
FROM admissions
WHERE patient_id = 542
GROUP BY patient_id
HAVING
  admission_date = MAX(admission_date);  
*/

SELECT patient_id, attending_doctor_id, diagnosis
FROM admissions
WHERE (patient_id % 2 <> 0 AND attending_doctor_id IN (1, 5, 19)) OR
	(attending_doctor_id like '%2%' AND LEN(patient_id) = 3)

SELECT doc.first_name, doc.last_name, count(patient_id) as total_num_admissions
FROM admissions adm
JOIN doctors doc
	ON adm.attending_doctor_id = doc.doctor_id
group by doc.first_name, doc.last_name

select doc.doctor_id, concat(doc.first_name, ' ', doc.last_name) as full_name, min(adm.admission_date) as first_admission, max(adm.admission_date) as last_admission
FROM admissions adm
JOIN doctors doc
	ON adm.attending_doctor_id = doc.doctor_id
group by doctor_id

SELECT pvn.province_name, COUNT(*) as total_patients
FROM patients pat
JOIN province_names pvn
	ON pat.province_id = pvn.province_id
group by pvn.province_name
ORDER By total_patients DESC;

SELECT pat.first_name || ' ' || pat.last_name as patient_fullname, adm.diagnosis, doc.first_name || ' ' || doc.last_name as doctor_fullname
FROM admissions adm
JOIN patients pat
	ON adm.patient_id = pat.patient_id
JOIN doctors doc
	on adm.attending_doctor_id = doc.doctor_id

/*  
SELECT first_name, last_name, count(*) AS duplicates
FROM patients
group by first_name, last_name
HAVING duplicates > 1;

select first_name, last_name, row_num
FROM (
SELECT *, Row_number() OVER (PARTITION BY first_name, last_name) as row_num
from patients)
WHERE row_num > 1;  
*/

SELECT
	first_name || ' ' || last_name as full_name,
    ROUND(height / 30.48, 1) as height_feet,
    ROUND(weight * 2.205, 0) AS weight_pound,
    birth_date,
    CASe
    	WHEN gender = 'M' THEN 'Male'
        ELSE 'Female'
    END AS gender_nonabv
FROM patients

SELECT patient_id, first_name, last_name
FROM patients
WHERE patient_id NOT IN (
	SELECT patient_id
  	FROM admissions
)

