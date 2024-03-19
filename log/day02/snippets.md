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

