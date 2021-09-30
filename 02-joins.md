# JOINS

Get two or more table's linked information.

## INNER JOINS

~~~~
SELECT o.order_id, o.customer_id, c.first_name
FROM orders o
JOIN customers c
ON o.customer_id = c.customer_id;
~~~~

## Exercise

Join the order_items on sql_store db and products tables on sql_inventory db containing order id, product name quantity and unit price.

Get all columns on order_items and products table:

~~~~
mysql> select * from order_items limit 1;
+----------+------------+----------+------------+
| order_id | product_id | quantity | unit_price |
+----------+------------+----------+------------+
|        1 |          4 |        4 |       3.74 |
+----------+------------+----------+------------+

mysql> select * from products limit 1;
+------------+-------------------+-------------------+------------+
| product_id | name              | quantity_in_stock | unit_price |
+------------+-------------------+-------------------+------------+
|          1 | Foam Dinner Plate |                70 |       1.21 |
+------------+-------------------+-------------------+------------+
~~~~

Create the statement:

~~~~
SELECT oi.order_id, oi.product_id, p.name, oi.quantity, oi.unit_price
FROM sql_store.order_items oi
JOIN sql_inventory.products p
ON oi.product_id = p.product_id
ORDER BY order_id;
~~~~

Result:

~~~~
+----------+------------+------------------------------+----------+------------+
| order_id | product_id | name                         | quantity | unit_price |
+----------+------------+------------------------------+----------+------------+
|        1 |          4 | Brocolinni - Gaylan, Chinese |        4 |       3.74 |
|        2 |          1 | Foam Dinner Plate            |        2 |       9.10 |
|        2 |          4 | Brocolinni - Gaylan, Chinese |        4 |       1.66 |
|        2 |          6 | Petit Baguette               |        2 |       2.94 |
|        3 |          3 | Lettuce - Romaine, Heart     |       10 |       9.12 |
|        4 |          3 | Lettuce - Romaine, Heart     |        7 |       6.99 |
|        4 |         10 | Broom - Push                 |        7 |       6.40 |
|        5 |          2 | Pork - Bacon,back Peameal    |        3 |       9.89 |
|        6 |          1 | Foam Dinner Plate            |        4 |       8.65 |
|        6 |          2 | Pork - Bacon,back Peameal    |        4 |       3.28 |
|        6 |          3 | Lettuce - Romaine, Heart     |        4 |       7.46 |
|        6 |          5 | Sauce - Ranch Dressing       |        1 |       3.45 |
|        7 |          3 | Lettuce - Romaine, Heart     |        7 |       9.17 |
|        8 |          5 | Sauce - Ranch Dressing       |        2 |       6.94 |
|        8 |          8 | Island Oasis - Raspberry     |        2 |       8.59 |
|        9 |          6 | Petit Baguette               |        5 |       7.28 |
|       10 |          1 | Foam Dinner Plate            |       10 |       6.01 |
|       10 |          9 | Longan                       |        9 |       4.28 |
+----------+------------+------------------------------+----------+------------+
~~~~

### SELF JOIN

Imgine you have the table below and needs to present it in a more human readable way. So instead of the reports_to column showing the name of the manager they report to istead of an id.

~~~~
mysql> SELECT * FROM employees;
+-------------+------------+------------+-----------------------------+--------+------------+-----------+
| employee_id | first_name | last_name  | job_title                   | salary | reports_to | office_id |
+-------------+------------+------------+-----------------------------+--------+------------+-----------+
|       33391 | D'arcy     | Nortunen   | Account Executive           |  62871 |      37270 |         1 |
|       37270 | Yovonnda   | Magrannell | Executive Secretary         |  63996 |       NULL |        10 |
|       37851 | Sayer      | Matterson  | Statistician III            |  98926 |      37270 |         1 |
|       40448 | Mindy      | Crissil    | Staff Scientist             |  94860 |      37270 |         1 |
|       56274 | Keriann    | Alloisi    | VP Marketing                | 110150 |      37270 |         1 |
|       63196 | Alaster    | Scutchin   | Assistant Professor         |  32179 |      37270 |         2 |
|       67009 | North      | de Clerc   | VP Product Management       | 114257 |      37270 |         2 |
|       67370 | Elladine   | Rising     | Social Worker               |  96767 |      37270 |         2 |
|       68249 | Nisse      | Voysey     | Financial Advisor           |  52832 |      37270 |         2 |
|       72540 | Guthrey    | Iacopetti  | Office Assistant I          | 117690 |      37270 |         3 |
|       72913 | Kass       | Hefferan   | Computer Systems Analyst IV |  96401 |      37270 |         3 |
|       75900 | Virge      | Goodrum    | Information Systems Manager |  54578 |      37270 |         3 |
|       76196 | Mirilla    | Janowski   | Cost Accountant             | 119241 |      37270 |         3 |
|       80529 | Lynde      | Aronson    | Junior Executive            |  77182 |      37270 |         4 |
|       80679 | Mildrid    | Sokale     | Geologist II                |  67987 |      37270 |         4 |
|       84791 | Hazel      | Tarbert    | General Manager             |  93760 |      37270 |         4 |
|       95213 | Cole       | Kesterton  | Pharmacist                  |  86119 |      37270 |         4 |
|       96513 | Theresa    | Binney     | Food Chemist                |  47354 |      37270 |         5 |
|       98374 | Estrellita | Daleman    | Staff Accountant IV         |  70187 |      37270 |         5 |
~~~~

Self join comes to save the day, as if the join was with any other table:

~~~~
SELECT e.employee_id, e.first_name, e.job_title, m.first_name as manager
FROM sql_hr.employees as e
JOIN sql_hr.employees as m
ON e.reports_to = m.employee_id;
~~~~

Result:

~~~~~
+-------------+------------+-----------------------------+----------+
| employee_id | first_name | job_title                   | manager  |
+-------------+------------+-----------------------------+----------+
|       33391 | D'arcy     | Account Executive           | Yovonnda |
|       37851 | Sayer      | Statistician III            | Yovonnda |
|       40448 | Mindy      | Staff Scientist             | Yovonnda |
|       56274 | Keriann    | VP Marketing                | Yovonnda |
|       63196 | Alaster    | Assistant Professor         | Yovonnda |
|       67009 | North      | VP Product Management       | Yovonnda |
|       67370 | Elladine   | Social Worker               | Yovonnda |
|       68249 | Nisse      | Financial Advisor           | Yovonnda |
|       72540 | Guthrey    | Office Assistant I          | Yovonnda |
|       72913 | Kass       | Computer Systems Analyst IV | Yovonnda |
|       75900 | Virge      | Information Systems Manager | Yovonnda |
|       76196 | Mirilla    | Cost Accountant             | Yovonnda |
|       80529 | Lynde      | Junior Executive            | Yovonnda |
|       80679 | Mildrid    | Geologist II                | Yovonnda |
|       84791 | Hazel      | General Manager             | Yovonnda |
|       95213 | Cole       | Pharmacist                  | Yovonnda |
|       96513 | Theresa    | Food Chemist                | Yovonnda |
|       98374 | Estrellita | Staff Accountant IV         | Yovonnda |
|      115357 | Ivy        | Structural Engineer         | Yovonnda |
+-------------+------------+-----------------------------+----------+
~~~~~

Beautiful!

Notice this is a very good example of inner join as poor Yovonnda is also an employee but was left out of the result set as she had a NULL value for manager:

~~~~
|       37270 | Yovonnda   | Executive Secretary         | NULL     |
~~~~

If we wanted Yovonnda on the picture we'd have to use RIGHT JOIN instead of just JOIN as the latter is implicitly an INNER JOIN.

## Multiple Tables JOIN

Join orders, customers and order_statuses tables:

~~~~
SELECT  o.order_id,
        o.order_date,
        c.first_name,
        c.last_name, 
        s.name AS status
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id
JOIN order_statuses os
    ON o.status = os.order_status_id
~~~~

Join payments, clients and payment_methods tables:

~~~~
SELECT p.date,
       p.invoice_id,
       p.amount,
       c.name,
       pm.name
FROM payments p
JOIN clients c
    ON p.client_id = c.client_id
JOIN payment_mathod pm
    ON p.payment_method = pm.paymentd_method_id
~~~~