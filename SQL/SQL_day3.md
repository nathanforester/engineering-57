### SQL day 3

## Arithmetic operators
- BODMAS: Brackets Other(indices) Division Multiplication Addition Subtraction

## Join sub queries
- LEFT JOIN: joins the left table with matching rows from the right table and omits non-matching data
- RIGHT JOIN: joins right table with matching rows from the left table and omits non-matching data
- INNER JOIN: joins both tables with all columns called with PK to FK values
- OUTER JOIN: not normally used, joins all tables into one table

## HAVING
- Checks column values after GROUP BY command parameter

## Union and Union ALL

## String methods
- Searches for string/sub strings, returns index and eliminates the need to hard code queries
- SELECT SUBSTRING(1,1)
- CHARINDEX{'a', 'text'} - if multiplie characters selected, treats as one character and indexes first instance of char
- LEFT(name, 5)
- RIGHT(name, 5): first left or right number of characters returned
- LTRIM/RTRIM: removes white Space
- REPLACE (<text>, '', '_ '): replaces element of first text, with specified element from last element, specified in the middle ''
- UPPER/LOWER: changes case

## Sorting

## Date functions
- GETDATE: get current date
- SYSDATETIME: get current local system time/date
- DATEADD: (d, 5, <column_name> AS <due_date_column_name): the '5' adds five days to the current date
- DATEDIFF: (d, <column_1>, <column_with_diff_date_time) - calculates date/time diff between columns
- YEAR/MONTH/DAY: get/set dates

## Case statements
- CASE STATEMENTS USE LOGICAL OPERATORS AND CAN ASSIGN ALIASES TO RETURNED VALUES


## Aggregate Functions
- SUM
- MIN
- MAX
- AVG
- COUNT : all perform functions finding the sum, minumum, maximum, average or number of elements in selected columns


```SQL

SELECT * FROM Employees e RIGHT OUTER JOIN Region r
ON (r.RegionID = e.EmployeeID)


SELECT * FROM Region r FULL OUTER JOIN Categories c
ON (r.RegionID = c.CategoryID)

SELECT p.ProductName, p.ProductID
FROM Products p
WHERE p.UnitPrice < 5.00

SELECT c.CategoryName, c.CategoryID
FROM Categories c
WHERE cCategoryName LIKE '^[BS]%'

SELECT COUNT(*) o.EmployeeID AS "Number of Orders"
FROM Orders o
WHERE o.EmployeeID IN(5, 7)

SELECT DISTINCT c.Countries
FROM Countries c
WHERE c.Region IS NOT NULL

SELECT * FROM Region r
LEFT OUTER JOIN Categories c
ON (r.RegionID = c.CategoryID)

SELECT UnitPrice, Quantity, Discount,
	UnitPrice * Quantity AS "Gross Total" --total before deductions
FROM [Order Details]

--Add a new SQL column to show the 'Net Total' which has the discount column applied to it
SELECT O.UnitPrice, o.Quantity, o.Discount, o.UnitPrice * .Quantity * (1-o.Discount) AS "NET TOTAL"
FROM [Order Details] o
ORDER BY "Net Total" --ASC by default, must specify DESC

SELECT UnitPrice, Quantity, Discount,
SUM(UnitPrice * Quantity) AS "Gross Total" --total before deductions
FROM [Order Details]


SELECT O.UnitPrice, o.Quantity, o.Discount, o.UnitPrice * .Quantity * (1-o.Discount) AS "NET TOTAL"
FROM [Order Details] o
GROUP BY o.orderID, o.OrderID, o.UnitPrice, o.Quantity, o.Discount
ORDER BY "Net Total"

SELECT PostCode "Post Code"
LEFT (PostCode CHARINDEX{'', PostCode}1) AS "Post Code something"
	CHARINDEX('',PostCode)AS "Space Used", Country
FROM Customers
WHERE Country = 'UK'

SELECT c.PostalCode AS "Post Code",
LEFT(PostalCode, CHARINDEX(' ',PostalCode)-1) AS "Postal Code Region",
CHARINDEX (' ', PostalCode) AS "Space Found", c.Country
FROM Customers c
WHERE Country = 'UK' --This one works and provides the postal code region

Provide list of products with only a single quote (Alias cannot be used in WHERE)
USE the wild card %

SELECT p.ProductsName
FROM Products p
Where CHARINDEX('''',p.ProductName) > 0 --returns the same as below

SELECT p.ProductsName
FROM Products p
WHERE p.ProductName LIKE '%''%' --returns the names

SELECT DATEADD(d,5,OrderDate) AS "Due Date",
	DATEDIFF(d,OrderDate,ShippedDate) AS "Ship Days"
FROM Orders

SELECT FirstName + '' + LastName AS "Employee Name",
DATEDIFF(yyyy, BirthDate, GETDATE()) AS "Age"
FROM Employees --get age in years of employee (e.g. 72 years)

SELECT FirstName + '' + LastName AS "Employee Name",
DATEDIFF(dd, BirthDate, GETDATE())/365.25 AS "Age"
FROM Employees --precise age (e.g. 71.427789)

SELECT CASE
WHEN DATEDIFF(d,OrderDate, ShippedDate) < 10 THEN 'On Time'
ELSE 'Overdue'
END AS "Status"
FROM Orders --Overdue/On-time

SELECT DATEDIFF(dd, OrderDate, ShippedDate) AS "Days For Delivery",
  CASE
WHEN DATEDIFF(dd, OrderDate, ShippedDate) < 10 THEN 'On Time'
ELSE 'Overdue'
END AS "Status"
FROM Orders --gives number of days in addition to status above example

SELECT FirstName + '' + LastName AS "Employee Name",
DATEDIFF(dd, BirthDate, GETDATE())/365.25 AS "Age",
 CASE
WHEN DATEDIFF(yyyy, Birthdate, GETDATE()) > 65
THEN 'Retired'
WHEN DATEDIFF(yyyy, Birthdate, GETDATE()) = 60
THEN 'Retrirement due'
ELSE 'More than 5 years to go'
END AS "Employee Status"
FROM Employees --check retirement status and show name/age

--Aggregate functions

SELECT * FROM Products

SELECT SUM(UnitsOnOrder) AS "T on order",
	AVG(UnitsOnOrder) AS "Avg on Order",
	MIN(UnitsOnOrder) AS "Min on Order",
	MAX(UnitsOnOrder) AS "Max on Order"
FROM Products

SELECT SupplierID
SUM(UnitsOnOder) AS "Total On Order"
	AVG(UnitsOnOrder) AS "Avg On Order",
	MAX(UnitsOnOrder) AS "Max On Order"
FROM Products
GROUP BY SupplierID

SELECT p.SupplierID,
	AVG(p.UnitsOnOrder) AS "Avg On Order",
	MAX(p.UnitsOnOrder) AS "Max On Order"
FROM Products p
GROUP BY p.SupplierID --syntax for it to work in Northwind_db

SELECT p.CategoryID
	AVG(p.ReorderLevel) AS "Average Re-orders",
FROM Products p
GROUP BY CategoryID --find average number of re-orders

SELECT TOP 1 p.CategoryID,
	AVG(p.ReorderLevel) AS "Average Re-orders"
FROM Products p
GROUP BY CategoryID
ORDER BY "Average re-orders" DESC --find the highest average order level

SELECT SupplierID,
SUM(UnitsOnOrder) AS "Total On Order",
	AVG(UnitsOnOrder) AS "Avg On Order"
FROM Products
GROUP BY SupplierID
HAVING(UnitsOnOrder)>5

SELECT student.name, course.name
FROM student INNER JOIN course ON student.course = course.id

SELECT p.SupplierID AS "Supplier Name", s.CompanyName, AVG(p.UnitsOnOrder) AS "Average Units on Orders",
FROM Products p
INNER JOIN Suppliers s
ON p.SupplierID = s.SupplierID
GROUP BY p.SupplierID, s.CompanyName

SELECT ProductName, UnitPrice, CompanyName AS "Supplier",
	CategoryName AS "Category"
From Products p
INNER JOIN Suppliers s ON p.SupplierID = s.SupplierID
INNER JOIN Categories c ON p.CategoryID = c.CategoryID

--SELECT o.OrderID, o.OrderID, o.OrderDate, o.Freight
--FROM Orders o
--INNER JOIN Customers c ON o.CustomerID =  
--INNER JOIN

SELECT OrderID CONVERT(VARCHAR(1), OrderDate,103) AS
[DD/MM/YYYY]
FROM Orders;/* Before 2012*/

SELECT OrderID, FORMAT(OrderDate,'11/MM/yyyy')
FROM Orders;/* After 2012*/ --converting dates

Select o.OrderID, FORMAT(o.OrderDate, 'dd/mm/yyyy')
FROM Orders o
