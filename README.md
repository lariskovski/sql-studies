# MOSH'S AWESOME SQL COURSE RESUME

[Mosh Courses](https://codewithmosh.com)

What is a database? A collection of data stored in a format that can be easily accessed.

To manage databases a Database Management System (DBMS) is used.

Using the DBMS it's possible to query or modify data.

Databases can be either Relational or Non-Relational.

In Relational databases data is stored across tables that are linked to each other using *relationships* to each other. Structured Query Language is used to query or modify data. Example of Relational DBMSs: Mysql, SQL Server, Oracle.

Non-Relation databases are different and don't use SQL for managing data.

> We can use database to refer to a server with multiple databases or database as in a collection of tables.

A table is how data is organized in rows and columns. Each column is a property of the data and each row is an entry or information unit.

[What is a database schema?](https://www.educative.io/blog/what-are-database-schemas-examples)

## The Basics: SQL 101

### Getting To Know The Territory

Few commands to know where to look when using the dark scary world of CLI.

``show databases;``

``use items;``

show tables;

### SELECT

Retrieve data.

#### Pro Tip: Number of Rows and Columns

Get the total number of entries/rows on a given table:

``SELECT COUNT(*) FROM items;``

Get all the columns in a given table:

``SELECT * FROM items LIMIT 1;``

#### New Output Column

Can perform operations and use AS to add alias to add a new column on output although it can be used to just give an existing column a prettier name.

``SELECT item, price, price / 1.1 AS 10_discount_price FROM items;``

#### Distinct Values

Returns all values without repetition; aka unique values.

``SELECT name, age, DISTINCT state FROM items``


### WHERE

Filter data.

``SELECT name, birth_date FROM customers WHERE birth_date >= '1990-01-01';``

#### Comparition Operators

Available comparition operators for the WHERE clause:

``< > <= >= = != <>``

The two last ones mean **not equal**.

``SELECT name, address, phone_number FROM customers WHERE state != 'NY';``

Both ``ny`` and ``NY`` give the same result. Which means WHERE is not case sensitive.

#### Logical Operators

Logial operators: AND, OR, NOT

``SELECT name, address, phone_number FROM customers WHERE state = 'NY' AND points > 1000;``

Execise:

From order_items get the items  for order #6 where the total price is greater than 30:

``select * from order_items where order_id = 6 and quantity * unit_price > 30;``

#### IN

``SELECT * FROM customers WHERE state IN ('va', 'ga', 'fl');``

With the IN operator it's possible o look for values in a list. Instead of:

``SELECT * FROM customers WHERE state = 'va OR state = 'ga' OR state = 'fl';``

#### BETWEEN

Self explanatory.

#### LIKE

#### REGEXP

#### NULL

#### ORDER!

#### LIMIT


