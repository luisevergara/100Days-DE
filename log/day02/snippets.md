<!-- SELECT Statement -->

/*
Select Statement
*, Top (Limit), Distinct, Count, As, Max, Min, Avg
*/

SELECT *
FROM [SQL Tutorial].dbo.EmployeeSalary

SELECT DISTINCT(Gender)
FROM EmployeeDemographics

SELECT TOP 5 *
FROM EmployeeSalary

SELECT Max(Salary) AS max_salary
FROM EmployeeSalary

<!-- CREATE TABLE [Table Name] -->

CREATE TABLE EmployeeDemographics (
    EmployeeID int,
    FirstName varchar(50),
    LastName varchar(50),
    Age int,
    Gender varchar(50)
)

<!-- INSERT INTO [Table Name] VALUES -->

INSERT INTO EmployeeDemographics VALUES
( 1001, 'Frank', 'Killian', 32, 'Male' ),
( 1002, 'Jill', 'Anderson', 23, 'Female' ),
( 1003, 'Christopher', 'Murphy', 45, 'Male' )

<!-- WHERE Statement -->

/*
Where Statement
=, <>, <, >, And, Or, Like, Null, Not Null, In
*/

SELECT *
FROM EmployeeDemographics
WHERE FirstName IN ('Jim', 'Michael')

SELECT *
FROM EmployeeDemographics
WHERE LastName = 'Scott'

SELECT *
FROM EmployeeDemographics
WHERE Age >= 43

SELECT *
FROM EmployeeDemographics
WHERE LastName = 'Scott' AND Gender = 'Female'

SELECT *
FROM EmployeeDemographics
WHERE LastName LIKE 'M%'

<!-- GROUP BY & ORDER BY -->

/*
Group By, Order By
*/

SELECT *
FROM EmployeeDemographics
ORDER BY 4 DESC, 5 DESC

SELECT Gender, COUNT(Gender) AS CountGender
FROM EmployeeDemographics
WHERE Age > 31
GROUP BY Gender

SELECT Age, COUNT(Age) As CountAge
FROM EmployeeDemographics
GROUP BY Age

SELECT JobTitle, COUNT(JobTitle) AS Headcount
FROM EmployeeSalary
GROUP BY JobTitle
ORDER BY Headcount DESC

<!-- BASIC JOINS -->

/*
Inner Joins, Full/Left/Right Outer Joins
*/

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
INNER JOIN [SQL Tutorial].dbo.EmployeeSalary
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
FULL OUTER JOIN [SQL Tutorial].dbo.EmployeeSalary
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
LEFT OUTER JOIN [SQL Tutorial].dbo.EmployeeSalary
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
RIGHT OUTER JOIN [SQL Tutorial].dbo.EmployeeSalary
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

SELECT EmployeeDemographics.EmployeeID, FirstName, LastName, Salary
FROM [SQL Tutorial].dbo.EmployeeDemographics
INNER JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
WHERE FirstName <> 'Michael'
ORDER BY Salary DESC

SELECT JobTitle, AVG(Salary) AS Average_Pay
FROM [SQL Tutorial].dbo.EmployeeDemographics
INNER JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
WHERE JobTitle = 'Salesman'
GROUP BY JobTitle

<!-- UNION & UNION ALL -->

/*
Union & Union All
*/

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
UNION
SELECT *
FROM [SQL Tutorial].dbo.WareHouseEmployeeDemographics

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics
UNION ALL
SELECT *
FROM [SQL Tutorial].dbo.WareHouseEmployeeDemographics

SELECT EmployeeID, FirstName, Age
FROM [SQL Tutorial].dbo.EmployeeDemographics
UNION
SELECT EmployeeID, JobTitle, Salary

<!-- CASE Statement -->

SELECT FirstName, LastName, Age,
CASE
	WHEN Age = 38 THEN 'Stanley'
	WHEN Age > 30 THEN 'Old'
	ELSE 'Baby'
END
FROM [SQL Tutorial].dbo.EmployeeDemographics
WHERE Age IS NOT NULL
ORDER BY Age

SELECT FirstName, LastName, JobTitle, Salary,
CASE
	WHEN JobTitle = 'Salesman' THEN Salary + (Salary * 0.10)
	WHEN JobTitle = 'Accountant' THEN Salary + (Salary * 0.05)
	WHEN JobTitle = 'HR' THEN Salary + (Salary * 0.000001)
	ELSE Salary + (Salary * 0.03)
END AS SalaryAfterRaise
FROM [SQL Tutorial].dbo.EmployeeDemographics
JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID

SELECT JobTitle, Salary,
CASE
    WHEN JobTitle IS NULL THEN 'Revision'
    ELSE 'Good'
END AS IsNull
FROM [SQL Tutorial].dbo.EmployeeSalary
