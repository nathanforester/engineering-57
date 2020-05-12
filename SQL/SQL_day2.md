# SQL_day2

- Auto-increment column will always be unique and is well suited to primary key
- NULL does not equate to nothing, but it does not equate to zero
- It is not an empty string
- SELECT * FROM <table_name> WHERE <column_name> IS NULL not = null
- Update and delete command syntax


- Normalisation - removing redundant date or information from the database
- 1NF we don't want 2 bits of data in 1 cell - ATOMIC
- NO repeating information (too many null values)
- functional dependency - non key attributes should not be part of the primary key
- 2NF composite keys - stored in two columns
- 3NF is 2NF except where non key attributes are not dependent on other non key attributes

### on delete cascade use case
- Allows deletion of the data in the primary key column to be cascaded to its foreign key reference
- This negates the need for manual deletion of data in other tables and keeps the data concurrent
### Foreign key referencing
- Assigning a foreign key to a column in a given table which references a primary key in another
### wild cards examples
- Using the LIKE keyword with %, [charset] or [^charset]
### SQL operators
- <> or != (not equal to), > greater than, < less than, = equal to, >= greater than or equal to, <= less than or =,
### BETWEEN - LIKE - AND - IN - NULL - IS NOT NULL - AS - WHERE
- SEE EXAMPLES IN SQL CODE -
- BETWEEN: between one value and another, inclusive of values stipulated
- LIKE: where you need to compare individual characters
- IN: getting the value of two specific attributes, listed individually (not 'SUM')
- AS: prevent raw data being presented from a query, using a custom column title
- WHERE: querying a specific condition
- NULL/NOT NULL: does not equate to zero, but indicates an empty field
### DISTINCT
- Very important in querying the databases where duplicate results are not wanted
### CONCATENATION
- Combine strings of information from separate fields based on a given database query

### EXAMPLES OF ALL FOLLOW IN SQL CODE BELOW

```SQL

CREATE DATABASE nathan_db

USE nathan_db

CREATE TABLE film_table (
    Film_id INT IDENTITY (1,1),
    Film_name VARCHAR(50),
    Film_type VARCHAR(25) UNIQUE,
    Date_of_release DATETIME,
    Director VARCHAR (20),
    Writer VARCHAR (20),
    Star VARCHAR (20),
    Film_language VARCHAR (10),
    Official_website VARCHAR (100),
    Plot_summary VARCHAR (255),
);

DROP TABLE film_table

SP_HELP film_table

SELECT * from film_table

ALTER TABLE film_table
ADD star_rating INT;

ALTER TABLE film_table
ALTER COLUMN Film_name VARCHAR(10) NOT NULL;


ALTER TABLE film_table
DROP COLUMN star_rating;

INSERT INTO film_table (
    Film_name, Film_type, Date_of_release, Director, Writer, Star, Film_language, Official_website, Plot_summary
)
VALUES (
    'Alien', 'sci-fi/horror', '19790525 10:34:09 AM', 'Ridley Scott', 'Alan Dean Foster', 'Sigourney Weaver', 'English', 'https://www.imdb.com/title/tt0078748/', 'A deep space mining crew investigate a beacon from a nearby planet and end up falling victim to a predatory creature that hunts them one at a time'
);

UPDATE film_table
SET star = 'Dirty Harry'
WHERE film_name = 'Alien'

DELETE FROM film_table
WHERE film_name = 'Alien' --updates rows

SELECT * FROM film_table

CREATE TABLE employee_table (
    employee_id INT IDENTITY(1, 1),
    f_name VARCHAR(15),
    l_name VARCHAR(15),
    email VARCHAR (50),
    PRIMARY KEY (employee_id)
    );

CREATE TABLE salary (
    employee_order INT IDENTITY(1, 1),
    employee_number INT,
    a_g_salary DECIMAL(6, 2),
    PRIMARY KEY (employee_order),
    FOREIGN KEY (employee_number) REFERENCES employee_table(employee_id) ON DELETE CASCADE

);

INSERT INTO employee_table
VALUES (
    'nathan', 'forester', 'nfeugene86@gmail.com'
);

INSERT INTO employee_table
VALUES (
    'Hussain', 'Ali', 'hussainali@gmail.com'
);

INSERT INTO salary
VALUES (
    3, 6000.00
);

INSERT INTO salary
VALUES
(
    2, 7000.00
)

DELETE FROM employee_table
WHERE employee_id = '3'

DROP table salary
DROP table employee_table


SP_HELP salary

SP_HELP employee_table

SELECT * FROM employee_table
SELECT * FROM salary

--AGE INT CHECK (Age>=18)

--crrate two tables each with primary keys
--crrate a foreign key referencing a primary key in another table
--on delete cascade apply to primary key which will automatically delete that data from the foreign key reference

QUERY CODE:

SELECT * FROM Customers
WHERE city = 'Paris'

SELECT * FROM Employees
WHERE city = 'London'

SELECT * FROM Employees
WHERE TitleOfCourtesy = 'Dr.'

SELECT COUNT(EmployeeID) AS "Number of Employees" FROM Employees

SELECT COUNT(Discontinued) AS "Discontinued Products" FROM Products
WHERE Discontinued != 0

SELECT COUNT(Discontinued) AS "Discontinued Products" FROM Products WHERE Product = 'Ikura'

SELECT CompanyName, City, Country, Region
FROM Customers WHERE Region = 'BC'

SELECT * FROM Products
WHERE ProductName LIKE '%ura'

SELECT CustomerID, City From Customers
WHERE Country = 'France'

SELECT TOP 100 ComapanyName, City FROM Customers
WHERE Country = 'France'

'AND' - joining conditions

SELCT ProductName, UnitPrice FROM Products
WHERE CategoryID = 1 AND Discontinued = '0'

SELCT FROM p.ProductName, p.UnitPrice
FROM Products p
WHERE p.CategoryID = 1 AND
p.Discontinued = 0

SELECT ProductName, UnitPrice FROM Products
WHERE UnitsInStock > 0 AND UnitPrice > 29.99

SELECT ProductName, UnitPrice FROM Products
WHERE UniInStock > 0 OR UnitPrice > 29.99

SELECT p.ProductName, p.UnitPrice
FROM Products p
WHERE p.UnitsInStock > 0 OR p.UnitPrice > 29.99

SELECT COUNT (* )
FROM Products p
WHERE p.UnitsInStock > 0 AND p.UnitPrice > 29.99

SELECT DISTINCT Country FROM Customers --removes duplicates
WHERE ContactTitle = 'Owner' AND Country LIKE 'c%'

SELECT DISTINCT Country FROM Customers --removes duplicates
WHERE ContactTitle = 'Owner' AND Country LIKE '_e% '' --looks for second letter starts with e'


SELECT DISTINCT Country FROM Customers --removes duplicates
WHERE ContactTitle = 'Owner' AND Country LIKE '[^S]%' --EXCLUDES countries starting with S

SELECT ProductName
FROM Products WHERE ProductName LIKE 'CH%'

SELECT *
FROM Customers WHERE Region LIKE '_A' --ending in A and two chars long

SELECT *
FROM Customers WHERE Region IN('WA','SP') --more than one region

SELECT *
FROM EmployeeTerritories
WHERE TerritoryID BETWEEN 06800 AND 09999 --includes boundary values

--product_id and names of the products with a unit price below 5.00

SELECT p.ProductName, p.ProductID
FROM Products p
WHERE p.UnitPrice < 5.00

category name beginning with B or S

SELECT * FROM Categories
WHERE CategoryName LIKE '[^BS]%'

how many orders are there for employees 5 and 7

SELECT o.EmployeeID, COUNT (*) AS "Number of Orders"
FROM Orders o
GROUP BY EmployeeID
HAVING EmployeeID IN (5, 7)



SELECT CompanyName AS 'CompanyName',
City+ ',' +Country AS 'City'
FROM Customers

or

SELECT c.CompanyName AS 'Company Name', CONCAT(c.CITY, ',', c.Country) AS 'City'
FROM Countries c



SELECT FROM c.Customers AS "Company Name"

SELECT using Employees table and concatenate first name and last name together. Use a column alias to rename the column to Employee name

SELECT e.FirstName + ' ' + e.LastName AS 'Employee Name'
From Employees e

SELECT Region, CompanyName AS 'CompanyName', City + ',' + Country AS 'City'
From Customers
WHERE Region IS NULL --find a null value, IS NOT NULL - not null

SELECT statement to list the six countries that have region codes in the customer table

SELECT DISTINCT c.Country --no duplicate values
FROM Customers c
WHERE c.Region IS NOT NULL
