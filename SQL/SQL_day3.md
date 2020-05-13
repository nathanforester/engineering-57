### SQL day 3

```SQL

SELECT * FROM Employees e RIGHT OUTER JOIN Region r
ON (r.RegionID = e.EmployeeID)


SELECT * FROM Region r FULL OUTER JOIN Categories c
ON (r.RegionID = c.CategoryID)
