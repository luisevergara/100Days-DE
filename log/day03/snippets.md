<!-- HAVING CLAUSE -->

SELECT JobTitle, COUNT(JobTitle)
FROM [SQL Tutorial].dbo.EmployeeDemographics
JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING COUNT(JobTitle) > 1

SELECT JobTitle, AVG(Salary)
FROM [SQL Tutorial].dbo.EmployeeDemographics
JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING AVG(Salary) > 45000
ORDER BY AVG(Salary)

/* Does Age impact on the average salary of employees? */

SELECT Age, AVG(Salary) AS Average_Salary
FROM [SQL Tutorial].dbo.EmployeeDemographics
JOIN [SQL Tutorial].dbo.EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY Age
HAVING AVG(Salary) > 50000

<!-- UPDATE - SET & DELETE -->

SELECT *
FROM [SQL Tutorial].dbo.EmployeeDemographics

UPDATE [SQL Tutorial].dbo.EmployeeDemographics
SET EmployeeID = 1012, Age = 31, Gender = 'Female' 
WHERE FirstName = 'Holly' AND LastName = 'Flax'

/* Delete with Condition, removal of condition may result in a DB Erasure */

DELETE (SELECT *)
FROM [SQL Tutorial].dbo.EmployeeDemographics
WHERE EmployeeID = 1005

<!-- ALIASING AS -->

/*
Aliasing
*/

SELECT FirstName + ' ' + LastName AS FullName
FROM [SQL Tutorial].dbo.EmployeeDemographics AS Demo

SELECT AVG(Age) AS AvgAge
FROM [SQL Tutorial].dbo.EmployeeDemographics

SELECT Demo.EmployeeID, Sal.Salary
FROM [SQL Tutorial].dbo.EmployeeDemographics AS Demo
JOIN [SQL Tutorial].dbo.EmployeeSalary AS Sal
	ON Demo.EmployeeID = Sal.EmployeeID

/* How you should not alias your queries */

SELECT a.EmployeeID, a.FirstName, a.FirstName, b.JobTitle, c.Age
FROM [SQL Tutorial].dbo.EmployeeDemographics a
LEFT JOIN [SQL Tutorial].dbo.EmployeeSalary b
	ON a.EmployeeID = b.EmployeeID
LEFT JOIN [SQL Tutorial].dbo.WareHouseEmployeeDemographics c
	ON a.EmployeeID = c.EmployeeID

/* How you should alias your queries */

SELECT Demo.EmployeeID, Demo.FirstName, Demo.LastName, Sal.JobTitle, Ware.Age
FROM [SQL Tutorial].dbo.EmployeeDemographics Demo
LEFT JOIN [SQL Tutorial].dbo.EmployeeSalary Sal
	ON Demo.EmployeeID = Sal.EmployeeID
LEFT JOIN [SQL Tutorial].dbo.WareHouseEmployeeDemographics Ware
	ON Demo.EmployeeID = Ware.EmployeeID