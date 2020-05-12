# Databases & SQL day 1
- A database is a data repisotory

## Objectives
- Know terminology
- Build SQL Databases
- Know data types
- List tools
- Compare and explain different database structure
- Simple relational database structure

### Day 1
- database: structured set of data held in a computer
- accessible in various ways
- Attributes/characteristics - columns
- rows - tuples
- DBMS - database management system
- Relational database contain connected data
- Tables should be related e.g.
- DB online_store:
- TABLE items: item_code(primary key), unit price, item desc.
- TABLE customer:customer_id(p'key), f_name, l_name, email
- TABLE purchases: purchase_no, date, item_code(foreignkey),
- customer_id(foreignkey)
- number_of_complaints can be attributed to purchases or
- customer table
- column/table name: all lowercase, python_case
- Databases must be ATOMIC
- Primary key should remain constant and not change

### Database types
- Flat file: MS Excel
- Relational: MySQL
- Big Data- Mongo DB, Vertica, business intelligence and
- business analysis, digital age and IoT
- Relationship types:
- One to one: unique relationship between columns in Tables
- e.g name, i.d number
- One to many: one customer, many receipts/order_id
- many to many: can have two primary keys to make a composite
- key (e.g. pk sudent_id // pk course_id)
- student can access many courses
- many students can access more than one course
- We do not want to add columns unnecessarily
- Junction table is needed
- stores primary keys
- many to many becomes one to many relationship
- store primary key as a foreign key
- (student_id can access course_id)studentID - courses and student
- Many to many is generally avoided
- Candidate keys in a table could become primary keys
- Discretion of the DBA which (database administrator)
- Primary key constraints: unique, not null, immutable, each table has 1
- Foreign key used to reference primary key in another table
- Corresponds to correct row of info in table b
- Prevents actions that destroy links between tables
- Prevents invalid data, no uniqueness for foreign keys - cannot be
- deleted

### Database Language Types
- DML: Database markup language: SELECT, INSERT, UPDATE, DELETE
- DDL: Database definition language: CREATE, ALTER, DROP, TRUNCATE
- DCL: Database control language: GRANT, REVOKE
- TCL: Transaction control language: COMMIT, ROLLBACK, SAVEPOINT

### Types of database software
- Postgre (open source)
- Redis (uses kvargs)
- Mongo DB - Agile, scalable, uses JSON
- JSON - JavaScript Object Notation

### Data Types
- VARCHAR - Takes more processor power, not fixed number of chars
- CHAR - Fixed number of chars, if < than stipulated, fills blankspace
- with _ underscores (not good for memory efficiency, but 50% faster than
- VARCHAR
- INT - Whole number (also BIGINT, SMALLINT)
- DATE/TIME/DATETIME
- DECIMAL/NUMERICAL (p,s or precision, scale)
- BINARY - Image/file
- FLOAT - Large number for scientific purposes
- BIT - 1, 0, NULL (functions as Boolean)
- Phone numbers should be saved as char not INT (+ operand in area codes)
- VARCHAR(MAX) - Could be an entire encyclopedia!!!
- /*
- * / - multi line comments
- --this would represent a single line comment

### AZURE/SQL script/code example CREATE and ALTER Databases
```SQL
CREATE DATABASE nathan_db

USE nathan_db

CREATE TABLE film_table (
    Film_id INT IDENTITY (1,1),
    Film_name VARCHAR(50),
    Film_type VARCHAR(25),
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
    'Alien', 'sci-fi/horror', '19790525 10:34:09 AM', 'Ridley Scott', 'Alan Dean Foster', 'Sigourney Weaver', 'English', 'https: //www.imdb.com/title/tt0078748/', 'A deep space mining crew investigate a beacon from a nearby planet and end up falling victim to a predatory creature that hunts them one at a time'
);

SELECT * FROM film_table

### End file
