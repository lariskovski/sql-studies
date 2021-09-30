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

Show all available databases:

~~~~
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| defaultdb          |
| information_schema |
| mysql              |
| performance_schema |
| sql_hr             |
| sql_inventory      |
| sql_invoicing      |
| sql_store          |
| sys                |
+--------------------+
9 rows in set (0.14 sec)
~~~~

Select a database to use:

``use sql_store;``

Show all tables on current database:

~~~~
mysql> show tables;
+---------------------+
| Tables_in_sql_store |
+---------------------+
| customers           |
| order_item_notes    |
| order_items         |
| order_statuses      |
| orders              |
| products            |
| shippers            |
+---------------------+
7 rows in set (0.15 sec)
~~~~

#### Statements vs Clauses

The following statement:

``SELECT foo FROM bar JOIN quux WHERE x = y;``

is made up of the following clauses:

- WHERE x = y
- SELECT foo
- FROM bar
- JOIN quux

### SELECT Clause

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


### WHERE Clause

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

#### IN Operator

``SELECT * FROM customers WHERE state IN ('va', 'ga', 'fl');``

With the IN operator it's possible o look for values in a list. Instead of:

``SELECT * FROM customers WHERE state = 'va OR state = 'ga' OR state = 'fl';``

#### BETWEEN Operator

Self explanatory.

``SELECT name, birth_date FROM customers WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';``

#### LIKE Operator

When the comparition doesnt need to exact. The LIKE operator comes in hand.

Get all customers that live in an avenue or trail:

``SELECT name, address FROM customers WHERE address LIKE '%avenue%' OR address LIKE '%trail%';``

#### REGEXP Operator

Like the LIKE operator but using regex.

``SELECT name, address FROM customers WHERE address REGEXP 'avenue|trail';``

> Note: the default regexp search comes with "implicit" percentage signs around the string. And | (pipe) indicates OR operator

Get customers which the name starts with ``L``:

``SELECT name, address FROM customers WHERE name REGEXP '^L';``

Get name that end in ``fa``, ``la`` and ``pa``:

``SELECT name, address FROM customers WHERE name REGEXP '[flp]a$';``

Get name that start with any character then an ``a``:

``SELECT name, address FROM customers WHERE name REGEXP '_a';``

#### IS NULL Operator

``SELECT name, address FROM custumers WHERE address IS NOT NULL;``

### ORDER

Get all items total cost in an order from highest to lowest price:

~~~~
SELECT order_id, product_name, quantity, unit_price, quantity * unit_price AS total_price
FROM items
ORDER BY total_price DESC``
~~~~

### LIMIT Clause

Limit the number of records returned from queries.

Get the three costumers with more points:

~~~~
SELECT name, phone_number
FROM custumers
ORDER BY points DESC
LIMIT 3;``
~~~~

#### OFFSET

We have the 10 items in a producsts table.

mysql> select * from products;
+------------+------------------------------+-------------------+------------+
| product_id | name                         | quantity_in_stock | unit_price |
+------------+------------------------------+-------------------+------------+
|          1 | Foam Dinner Plate            |                70 |       1.21 |
|          2 | Pork - Bacon,back Peameal    |                49 |       4.65 |
|          3 | Lettuce - Romaine, Heart     |                38 |       3.35 |
|          4 | Brocolinni - Gaylan, Chinese |                90 |       4.53 |
|          5 | Sauce - Ranch Dressing       |                94 |       1.63 |
|          6 | Petit Baguette               |                14 |       2.39 |
|          7 | Sweet Pea Sprouts            |                98 |       3.29 |
|          8 | Island Oasis - Raspberry     |                26 |       0.74 |
|          9 | Longan                       |                67 |       2.26 |
|         10 | Broom - Push                 |                 6 |       1.09 |
+------------+------------------------------+-------------------+------------+
10 rows in set (0.15 sec)

Say in a web app when we need the a pagination with 3 items per page, like below:

-- page 1:  1 - 3
-- page 2:  4 - 6
-- page 3:  7 - 9

The app would have to go in the database using the three following statements:

Get the first three elements with an offset of 0. Which means no items were 'skipped'.

~~~~
mysql> select * from products limit 0, 3;
+------------+---------------------------+-------------------+------------+
| product_id | name                      | quantity_in_stock | unit_price |
+------------+---------------------------+-------------------+------------+
|          1 | Foam Dinner Plate         |                70 |       1.21 |
|          2 | Pork - Bacon,back Peameal |                49 |       4.65 |
|          3 | Lettuce - Romaine, Heart  |                38 |       3.35 |
+------------+---------------------------+-------------------+------------+
3 rows in set (0.15 sec)
~~~~

Then, skip the first three items and get three more.

~~~~
mysql> select * from products limit 3, 3;
+------------+------------------------------+-------------------+------------+
| product_id | name                         | quantity_in_stock | unit_price |
+------------+------------------------------+-------------------+------------+
|          4 | Brocolinni - Gaylan, Chinese |                90 |       4.53 |
|          5 | Sauce - Ranch Dressing       |                94 |       1.63 |
|          6 | Petit Baguette               |                14 |       2.39 |
+------------+------------------------------+-------------------+------------+
3 rows in set (0.15 sec)
~~~~

Skip 6 items and get three  more. 

~~~~
mysql> select * from products limit 6, 3;
+------------+--------------------------+-------------------+------------+
| product_id | name                     | quantity_in_stock | unit_price |
+------------+--------------------------+-------------------+------------+
|          7 | Sweet Pea Sprouts        |                98 |       3.29 |
|          8 | Island Oasis - Raspberry |                26 |       0.74 |
|          9 | Longan                   |                67 |       2.26 |
+------------+--------------------------+-------------------+------------+
~~~~

So on, til the end is reached.

~~~~
mysql> select * from products limit 9, 3;
+------------+--------------+-------------------+------------+
| product_id | name         | quantity_in_stock | unit_price |
+------------+--------------+-------------------+------------+
|         10 | Broom - Push |                 6 |       1.09 |
+------------+--------------+-------------------+------------+
1 row in set (0.15 sec)
~~~~