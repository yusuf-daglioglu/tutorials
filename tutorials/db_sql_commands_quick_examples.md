# DB SQL COMMANDS QUICK EXAMPLES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

```sql

------------------------------------------
------------------------------------------
-- CREATE SCHEME AND DATA
-- TESTES ONLY WITH: SQLite 3.46.1
------------------------------------------
------------------------------------------

-- TABLE NAME: customers

-- id|name |surname |
-- --+-----+--------+
--  1|user1|surname1|
--  2|user2|surname2|


-- TABLE NAME: orders

-- id|customer_id|status|active|
-- --+-----------+------+------+
--  1|          1|   100|     1|
--  2|          1|   101|     1|
--  3|          1|   102|     0|
--  4|          2|   103|     1|

drop table if exists customers;
drop table if exists orders;

CREATE TABLE customers (
 id INTEGER PRIMARY KEY AUTOINCREMENT,
 name TEXT,
 surname TEXT
);

INSERT INTO customers
(name, surname)
VALUES
('user1', 'surname1'),
('user2', 'surname2');

CREATE TABLE orders (
 id INTEGER PRIMARY KEY AUTOINCREMENT,
 customer_id INTEGER,
 status INTEGER,
 active BOOLEAN
);

INSERT INTO orders
(customer_id, status, active)
VALUES
(1, 100, true),
(1, 101, true),
(1, 102, false),
(2, 103, true);

------------------------------------------
------------------------------------------
-- EXAMPLE 1
------------------------------------------
------------------------------------------

-- show status list for each customer.
-- output should be:

-- name     status list
-- user1 100,101
-- user2 103

------------------------------------------
------------------------------------------
-- NOTES:
-- For below 2 solutions SQLite group_concat built-in function has been used.
-- For MSSQL "STRING_AGG" built-in function can be used. It gets same parameters.
------------------------------------------
------------------------------------------

-- SOLUTION 1 ---------------------------
-- with LEFT JOIN and GROUP BY
------------------------------------------
select 
customers.name,
group_concat(orders.status, ',') AS 'status list'
from customers
left join orders
on 
( 
 customers.id = orders.customer_id 
 AND
 orders.active = true
)
GROUP by customers.name

-- SOLUTION 2 ---------------------------
-- execute independent SQL inside SELECT.
------------------------------------------
select 
customers.name,
(
  (
 SELECT group_concat(orders.status , ',')
  FROM orders
  where 
 customers.id = orders.customer_id 
 AND
  orders.active = true
  )
) as 'status list'
from customers


------------------------------------------
------------------------------------------
-- EXAMPLE 2
------------------------------------------
------------------------------------------

-- show active and passive order-ids for each customer.
-- output should be:

-- name     passive-order-id-list   active-order-id-list
-- user1 3                       1,2
-- user2                         4

-- SOLUTION 1- ---------------------------
-- execute independent SQL inside SELECT.
------------------------------------------
select 
customers.name,
(
 SELECT 
 group_concat(orders.ID, ',') 
 FROM orders
 WHERE
 orders.active = FALSE
 AND
 customers.id = orders.customer_id 
) AS 'passive-order-id-list',
(
 SELECT 
 group_concat(orders.ID, ',') 
 FROM orders
 WHERE
 orders.active = TRUE
 AND
 customers.id = orders.customer_id 
) AS 'active-order-id-list'
from customers


-- SOLUTION 2 ----------------------------
-- Join "orders" multiple times
------------------------------------------

SELECT 
    c.name,
    po.passive_order_ids,
    ao.active_order_ids
FROM 
    customers c
LEFT JOIN (
    SELECT 
        customer_id, 
        GROUP_CONCAT(id) AS passive_order_ids
    FROM 
        orders
    WHERE 
        active = FALSE
    GROUP BY 
        customer_id
) AS po 
ON c.id = po.customer_id
LEFT JOIN (
    SELECT 
        customer_id, 
        GROUP_CONCAT(id) AS active_order_ids
    FROM 
        orders
    WHERE 
        active = TRUE
    GROUP BY 
        customer_id
) AS ao 
ON c.id = ao.customer_id;


```
