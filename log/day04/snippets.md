<!-- PARTITION BY -->

/*
Today's Topic: Partition By
*/

SELECT FirstName, LastName, Gender, Salary,
COUNT(Gender) OVER (PARTITION BY Gender) AS TotalGender
FROM EmployeeDemographics Demo
JOIN EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID

SELECT Gender, COUNT(Gender)
FROM EmployeeDemographics Demo
JOIN EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
GROUP BY Gender

<!-- CTE -->

/*
CTEs
*/

WITH CTE_Employee AS (
SELECT FirstName, LastName, Gender, Salary,
COUNT(Gender) OVER (PARTITION BY Gender) as TotalGender,
AVG(Salary) OVER (PARTITION BY Gender) as AvgSalary
FROM EmployeeDemographics Demo
JOIN EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
WHERE Sal.Salary > '45000'
)
SELECT FirstName, AvgSalary
FROM CTE_Employee

<!-- Temp Tables -->

/*
Temp Tables
*/

CREATE TABLE #temp_Employee (
EmployyeeID int,
JobTitle varchar(100),
Salary int
)

SELECT *
FROM #temp_Employee

INSERT INTO #temp_Employee VALUES
(1001, 'HR', 45000)

INSERT INTO #temp_Employee
SELECT *
FROM [SQL Tutorial]..EmployeeSalary

DROP TABLE IF EXISTS #temp_Employee2
CREATE TABLE #temp_Employee2 (
JobTitle varchar(50),
EmployeesPerJob int,
AvgAge int,
AvgSalary int
)

INSERT INTO #temp_Employee2
SELECT JobTitle, COUNT(JobTitle), AVG(Age), AVG(Salary)
FROM [SQL Tutorial]..EmployeeDemographics Demo
JOIN [SQL Tutorial]..EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
GROUP BY JobTitle

SELECT *
FROM #temp_Employee2

<!-- Stored Procedures -->

/*
Stored Procedures
*/

CREATE PROCEDURE TEST
AS
SELECT *
FROM EmployeeDemographics

EXEC TEST

CREATE PROCEDURE Temp_Employee
AS
CREATE TABLE #temp_Employee2 (
JobTitle varchar(50),
EmployeesPerJob int,
AvgAge int,
AvgSalary int
)

INSERT INTO #temp_Employee2
SELECT JobTitle, COUNT(JobTitle), AVG(Age), AVG(Salary)
FROM [SQL Tutorial]..EmployeeDemographics Demo
JOIN [SQL Tutorial]..EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
GROUP BY JobTitle

SELECT *
FROM #temp_Employee2

EXEC Temp_Employee @JobTitle = 'Salesman'

/* ALTER PROCEDURE */

USE [SQL Tutorial]
GO
/****** Object:  StoredProcedure [dbo].[Temp_Employee]    Script Date: 3/25/2024 12:05:26 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER PROCEDURE [dbo].[Temp_Employee]
@JobTitle nvarchar(100)
AS
CREATE TABLE #temp_Employee2 (
JobTitle varchar(50),
EmployeesPerJob int,
AvgAge int,
AvgSalary int
)

INSERT INTO #temp_Employee2
SELECT JobTitle, COUNT(JobTitle), AVG(Age), AVG(Salary)
FROM [SQL Tutorial]..EmployeeDemographics Demo
JOIN [SQL Tutorial]..EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
WHERE JobTitle = @JobTitle
GROUP BY JobTitle

SELECT *
FROM #temp_Employee2

<!-- Subquery -->

/*
Subqueries (in the SELECT, FROM, and WHERE Statement)
*/

SELECT *
FROM EmployeeSalary

-- Subquery in SELECT

SELECT EmployeeID, Salary, (SELECT AVG(Salary) FROM EmployeeSalary) AS AllAvgSalary
FROM EmployeeSalary

-- How to do it in PARTITION BY

SELECT EmployeeID, Salary, AVG(Salary) OVER () AS AllAvgSalary
FROM EmployeeSalary

-- Subquery in FROM

SELECT a.EmployeeID, AllAvgSalary
FROM (
	SELECT EmployeeID, Salary, AVG(Salary) OVER () AS AllAvgSalary
	FROM EmployeeSalary
) a

-- Subquery in WHERE

SELECT EmployeeID, JobTitle, Salary
FROM EmployeeSalary
WHERE EmployeeID in (
	SELECT EmployeeID
	FROM EmployeeDemographics
	WHERE Age > 30
)