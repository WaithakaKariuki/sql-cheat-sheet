# SQL Cheat Sheet

## What is SQL? Why Do We Need It?

SQL is a database language that’s used to query and manipulate data in a database, while also giving you an efficient and convenient environment for database management.We can group commands into the following categories of SQL statements:

#### Data Definition Language (DDL) Commands

● CREATE: creates a new database object, such as a table

● ALTER: used to modify a database object

● DROP: used to delete database objects

#### Data Manipulation Language (DML) Commands

● INSERT: used to insert a new row (record) in a database table

● UPDATE: used to modify an existing row (record) in a database table

● DELETE: used to delete a row (record) from a database table

#### Data Control Language (DCL) Commands

● GRANT: used to assign user permissions to access database objects

● REVOKE: used to remove previously granted user permissions for accessing database objects

#### Data Query Language (DQL) Commands

● SELECT: used to select and return data (query) from a database

#### Data Transfer Language (DTL) Commands

● COMMIT: used to save a transaction within the database permanently

● ROLLBACK: restores a database to the last committed state

## SQL Data Types

Data types specify the type of data that an object can contain, such as numeric data or character data. We need to choose a data type to match the data that will be stored using the following list of essential pre-defined data types:
(Data Type - Used to Store)

●int - Integer data (exact numeric)

●smallint - Integer data (exact numeric)

●tinyint - Integer data (exact numeric)

●bigint - Integer data (exact numeric)

●decimal - Numeric data type with a fixed precision and scale (exact numeric)

●numeric - Numeric data type with a fixed precision and scale (exact numeric)

●float - Floating precision data (approximate numeric)

●moneyMonetary (currency) data

●datetime - Date and time data

●char(n) - Fixed length character data

●varchar(n) - Variable length character data

●text - Character string

●bit - Integer data that is a 0 or 1 (binary)

●image - Variable length binary data to store image files

●real - Floating precision number (approximate numeric)

●binary - Fixed length binary data

●cursor - Cursor reference

●sql_variant - Allows a column to store varying data types

●timestamp - Unique database that is updated every time a row is inserted or updated

●table - Temporary set of rows returned after running a table-valued function(TVF)

●xml - Stores xml data

## Data Definition Language

### CREATE Statement

CREATE TABLE:

sqlite3 pets_database.db

sqlite> .schema
CREATE TABLE cats (
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
  breed TEXT
);

OUTPUT FORMAT;

.headers on ->output the name of each column

.mode column -> now we are in column mode, enabling us to run the next two .width commands

.width auto -> adjusts and normalizes column width

.width NUM1, NUM2 -> customize column width

### ALTER Statement
ALTER TABLE:

●ALTER TABLE table_name
ADD column_name data_type;

●ALTER TABLE table_name
DROP COLUMN column_name;

●ALTER TABLE table_name
ALTER COLUMN column_name data_type;

### DROP Statement

●DROP TABLE table_name;

TRUNCATE TABLE statement:

●TRUNCATE TABLE table_name;

## Data Manipulation Language Statements 

### INSERT Statement

●INSERT INTO cats (name, age, breed) VALUES ('Maru', 3, 'Scottish Fold');

● INSERT INTO Student
VALUES ( 101, ’John’, ’Ray’, 78 ),
( 102, ‘Steve’, ’Jobs’, 89 );

● When using a sql file, each statement gets its own line ending with a ';'. Then in terminal run: $ sqlite3 table_database.db < table.sql

COPY DATA & INSERT:

INSERT INTO table_name2
SELECT * FROM table_name1
WHERE [condition];

### UPDATE Statement

●UPDATE [table name] SET [column name] = [new value] WHERE [column name] = [value];

●UPDATE cats SET name = "Hana" WHERE name = "Hannah";

●UPDATE table_name SET Marks = 85 WHERE FirstName = ‘Andy’;

### DELETE Statement

●DELETE FROM [table name] WHERE [column name] = [value];

●DELETE FROM cats WHERE id = 3;

Remove all rows (records) from a table without deleting the table with DELETE:

● DELETE FROM table_name;

## Data Query Language Statements

### SELECT Statement

#### SELECT FROM:
●SELECT [names of columns we are going to select] FROM [table we are selecting from];

●SELECT id, name, age, breed FROM cats;

●SELECT * FROM cats;

●SELECT * FROM cats WHERE name = "Maru";

●SELECT cats.name FROM cats;

●SELECT cats.name, dogs.name FROM cats, dogs;

#### DISTINCT:

●eliminates rows with duplicate values in a particular column

SELECT DISTINCT Marks FROM Student WHERE LastName LIKE ‘o%’;

### Selecting Based on Conditions:

#### The WHERE Clause:

●SELECT * FROM [table name] WHERE [column name] = [some value];

●SELECT * FROM cats WHERE name = "Maru";

●SELECT * FROM cats WHERE age < 2;

●SELECT StudentID, Marks FROM Student WHERE Marks != 89;

●SELECT StudentID, Marks FROM Student WHERE Marks >= 50;

●SELECT StudentID, Marks FROM Student WHERE Marks = 40 OR Marks = 56 OR Marks = 65;

●SELECT StudentID, Marks FROM Student WHERE Marks = 89 AND LastName = ‘Jones’;

●SELECT StudentID, Marks FROM Student WHERE NOT LastName = ‘Jobs’;

#### ORDER BY Statements

ORDER BY:

●SELECT column_name FROM table_name ORDER BY column_name ASC|DESC;

●SELECT * FROM cats ORDER BY age;

●SELECT * FROM cats ORDER BY age DESC;

#### SQL aggregate functions:

COUNT:

●SELECT COUNT([column name]) FROM [table name] WHERE [column name] = [value]

●SELECT COUNT(owner_id) FROM cats WHERE owner_id = 1;

GROUP BY

●SELECT breed, COUNT(breed) FROM cats GROUP BY breed;

●SELECT breed, owner_id, COUNT(breed) FROM cats GROUP BY breed, owner_id;

#### LIKE with ‘%’:

SELECT * FROM Student
WHERE FirstName LIKE ‘Ro%’;

#### LIKE with ‘_’:

SELECT *FROM Student
WHERE FirstName LIKE ‘_e’;

#### LIMIT:

SELECT * FROM cats ORDER BY age DESC LIMIT 1;

#### BETWEEN:

●SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;

●SELECT name FROM cats WHERE age BETWEEN 1 AND 3;

#### NOT BETWEEN:

SELECT FirstName, Marks FROM Student WHERE Marks NOT BETWEEN 40 AND 50;

#### NULL:

●INSERT INTO cats (name, age, breed) VALUES (NULL, NULL, "Tabby");

●SELECT * FROM cats WHERE name IS NULL;


## Transactional Control Commands

DROP TABLE table_name;

## Table relations & JOINS:

### INNER JOIN:

- Returns all rows when there is at least one match in BOTH tables.

SELECT column_names
FROM first_table
INNER JOIN second_table
ON first_table.column_name = second_table.column_name;

SELECT
  cats.name,
  cats.breed,
  owners.name AS "owner_name"
FROM cats
INNER JOIN owners
ON cats.owner_id = owners.id;

### LEFT(OUTER)JOIN:

- Returns all rows from the left table, and the matched rows from the right table

SELECT column_name(s)
FROM first_table
LEFT JOIN second_table
ON first_table.column_name = second_table.column_name;

SELECT cats.name, cats.breed, owners.name
FROM cats
LEFT OUTER JOIN owners
ON cats.owner_id = owners.id;

### RIGHT OUTER JOIN:

- every row from the ‘right’ table (right of the RIGHT OUTER JOIN
keyword) is returned, and matching rows from the ‘left’ table are returned.

SELECT Employees.LastName, Employees.Title, EmployeeSalary.salary
FROM Employees
RIGHT OUTER JOIN EmployeeSalary
ON Employees.EmployeeID = EmployeeSalary.EmployeeID;

### FULL OUTER JOIN:

-returns all the matching and non-matching rows from both tables,
with each row being displayed no more than once.

SELECT Employees.LastName, Employees.Title, EmployeeSalary.salary
FROM Employees
FULL OUTER JOIN EmployeeSalary
ON Employees.EmployeeID = EmployeeSalary.EmployeeID;

### CROSS JOIN:

-Also known as the Cartesian Product, a CROSS JOIN between two tables (A and B)
‘joins’ each row from table A with each row in table B, forming ‘pairs’ of rows.

SELECT col_1, col_2 FROM table1
CROSS JOIN table2;

### SELF JOIN:

-A SELF JOIN is when you join a table with itself. This is useful when you want to query
and return correlatory information between rows in a single table.

SELECT t1.col1 AS “Column 1”, t2.col2 AS “Column 2”
FROM table1 AS t1
JOIN table1 AS t2
WHERE condition;

## SQL Table Constraints:

-Constraints define rules that ensure consistency and correctness of data.

CREATE TABLE table_name(
col_name data_type,
CONSTRAINT constraint_name
PRIMARY KEY (col_name(s))
);

CREATE TABLE table_name(
col_name data_type,
CONSTRAINT constraint_name
FOREIGN KEY (col_name)
REFERENCES
table_name(col_name)
);

## RENAME TABLE:

RENAME TABLE old_table_name TO new_table_name;

## The ACID Property

The term ACID stands for Atomicity, Consistency, Isolation, and Durability. These
individual properties represent a standardized group that are required to ensure the
reliable processing of database transactions.

#### Atomicity

The concept that an entire transaction must be processed fully, or not at all.

#### Consistency

The requirement for a database to be consistent (valid data types, constraints, etc) both before and after a transaction is completed.

#### Isolation

Transactions must be processed in isolation and they must not interfere with other
transactions.

#### Durability

After a transaction has been started, it must be processed successfully. This applies even if there is a system failure soon after the transaction is started.

## RDBMS

A Relational Database Management System (RDBMS) is a piece of software that allows you to perform database administration tasks on a relational database, including creating, reading, updating, and deleting data (CRUD). Relational databases store collections of data via columns and rows in various tables. Each table can be related to others via common attributes in the form of Primary and Foreign Keys.

