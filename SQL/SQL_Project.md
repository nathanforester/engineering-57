### SQL Project

```SQL

1.1 SELECT c.CustomerID, c.CompanyName, c.Address, c.City, c.Country
FROM Customers c
WHERE City = 'Paris' OR City = 'London'

1.2 SELECT p.ProductName
FROM Products p
WHERE p.QuantityPerUnit LIKE '%bottle%'

1.3 SELECT p.ProductName, s.CompanyName, s.Country
FROM Products p
LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
WHERE p.QuantityPerUnit LIKE '%bottle%'

1.4 SELECT c.CategoryID, c.CategoryName, COUNT(p.ProductID) AS "Number of Products"
FROM Categories c
LEFT JOIN Products p ON c.CategoryID = p.CategoryID
GROUP BY C.CategoryID, c.CategoryName
ORDER BY "Number of Products" DESC

1.5 SELECT e.TitleOfCourtesy + ' ' + e.FirstName + ' ' + e.LastName AS "Employee Name",
e.Country
FROM Employees e
WHERE Country = 'UK'

1.6 SELECT r.RegionID, FORMAT(SUM(od.UnitPrice * (1-od.Discount) * od.Quantity), 'C') "Total Discounted Price"
FROM [Order Details] od
INNER JOIN Orders o  ON o.OrderID = od.OrderID
INNER JOIN EmployeeTerritories e ON o.EmployeeID = e.EmployeeID
INNER JOIN Territories t  ON e.TerritoryID = t.TerritoryID
INNER JOIN Region r ON  t.RegionID = r.RegionID
GROUP BY r.RegionID
HAVING  (SUM(od.UnitPrice * (1-od.Discount) * od.Quantity)) > 1000000

1.7 SELECT  COUNT(o.OrderID) AS "Total Freight"
FROM Orders o
WHERE o.Freight > 100.00 AND (ShipCountry = 'UK' OR ShipCountry = 'USA')


1.8SELECT TOP 1 o.OrderID, MAX(o.Discount) AS "Highest Discount",
ROUND(SUM(o.UnitPrice*(1-o.Discount) *o.Quantity),2) AS "Discounted Price",
SUM(o.UnitPrice * o.Quantity)
FROM [Order Details] o
GROUP BY o.OrderId

2.1 CREATE TABLE SpartansTable_nf (
	SpartanID INT IDENTITY(1,1),
	Title VARCHAR(4),
	Fname VARCHAR(20),
	Lname VARCHAR(20),
	UniversityAttended VARCHAR(50),
	CourseTaken VARCHAR (40),
	MarkAchieved VARCHAR (15)
)

2.2 INSERT INTO SpartansTable_nf (
	Title, Fname, Lname, UniversityAttended, CourseTaken, MarkAchieved
)

VALUES (
	'Mr.', 'Nathan', 'Forester', 'Swansea University', 'MSc Computer Science', 'Merit'
);


3.1 SELECT e.FirstName + ' ' + e.LastName, e.ReportsTo
FROM Employees e

3.2 ROUND(SUM(od.UnitPrice*(1-od.Discount)*od.Quantity),2) AS "Total Sales"
FROM Products p
INNER JOIN Suppliers s ON p.SupplierID = s.SupplierID
INNER JOIN [Order Details] od ON p.ProductID = od.ProductID
GROUP BY s.CompanyName
HAVING SUM(od.UnitPrice*od.Quantity) > 10000
ORDER BY "Total Sales" DESC

3.3 SELECT TOP 10 c.ContactName, ROUND(SUM(od.UnitPrice*Quantity*(1-Discount)), 2) AS "Total Value of Orders"
FROM Orders o
INNER JOIN Customers c ON o.CustomerID = c.CustomerID
INNER JOIN [Order Details] od ON o.OrderID = od.OrderID
GROUP BY c.ContactName, o.ShippedDate
HAVING DATEDIFF(yyyy, o.ShippedDate, GETDATE()) > 23
ORDER BY "Total Value of Orders" DESC

3.4 SELECT AVG(DATEDIFF(dd, o.OrderDate, o.ShippedDate))
FROM Orders o
GROUP BY MONTH(o.ShippedDate), YEAR(o.ShippedDate)

SELECT FORMAT(o.ShippedDate,'MM/yyyy'), AVG(DATEDIFF(dd, o.OrderDate, o.ShippedDate)) AS "Average Days"
FROM Orders o
GROUP BY YEAR(o.ShippedDate), MONTH(o.ShippedDate), FORMAT(o.ShippedDate,'MM/yyyy')
ORDER BY YEAR(o.ShippedDate), MONTH(o.ShippedDate)
