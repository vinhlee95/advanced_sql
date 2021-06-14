# CTE

```sql
CREATE TABLE users (
  id int primary key,
  first_name varchar(255),
  last_name varchar(255),
  email varchar(255),
  gender varchar(20),
  age int,
  language varchar(255)
);

create table purchases (
  date date,
  user_handle int,
  sku int,
  quantity int
);

create table products (
  sku int,
  product text,
  price money
);
```

## Problem
Find the average quantity a single user (:id) is purchasing of each product

### Using sub-query
```sql
select user_handle, sku,
(select avg(quantity) from Purchases
  where user_handle = p.user_handle and sku = p.sku
) from Purchases p
where user_handle = :id
group by user_handle, sku;
```

### Using CTE
```sql
with avg_quantity as (
  select avg(quantity), user_handle, sku
  from purchases p where user_handle = p.user_handle and sku = p.sku
  group by user_handle, sku
) -- this CTE is now reusable!!!
select * from avg_quantity where user_handle = 59;
```

## CTE can be recursive
Create an `employee` table:

```sql
create table employee (
  id int pk,
  first_name varchar,
  last_name varchar,
  manager_id int
);

insert into employee (id, first_name, last_name, manager_id)
values (1, 'Foo', 'Bar', null);

insert into employee (id, first_name, last_name, manager_id)
values (2, 'James', 'Bond', 1);

insert into employee (id, first_name, last_name, manager_id)
values (3, 'Tom', 'Cruise', 2);

insert into employee (id, first_name, last_name, manager_id)
values (4, 'Super', 'Man', 1);
```

### Problem
Show the "chain" of management for each employee based on manager id.

```sql
WITH RECURSIVE employee_chain AS (
  SELECT
    id,
    first_name,
    last_name,
    first_name || ' ' || last_name AS chain
  FROM employee
  WHERE manager_id IS NULL
  UNION ALL
  SELECT
    employee.id,
    employee.first_name,
    employee.last_name,
    chain || '->' || employee.first_name || ' ' || employee.last_name
  FROM employee_chain
  JOIN employee
    ON employee.manager_id = employee_chain.id
)
 
SELECT
  first_name,
  last_name,
  chain
FROM employee_chain;
```

Results:

```text
 first_name | last_name |              chain
------------+-----------+---------------------------------
 Foo        | Bar       | Foo Bar
 James      | Bond      | Foo Bar->James Bond
 Super      | Man       | Foo Bar->Super Man
 Tom        | Cruise    | Foo Bar->James Bond->Tom Cruise
(4 rows)
```


## Resources
https://learnsql.com/blog/sql-subquery-cte-difference/