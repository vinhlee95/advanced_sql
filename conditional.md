# SQL Conditional statement

```sql
with avg_quantity as (
  select avg(quantity) as avg_quantity, user_handle, sku
  from purchases p where user_handle = p.user_handle and sku = p.sku
  group by user_handle, sku
)
select user_handle, sku, avg_quantity,
  case when avg_quantity > 10 then 'large'
    when avg_quantity > 5 then 'medium'
    else  'small'
    end as quantity_type
from avg_quantity where user_handle = 59 order by avg_quantity;
```

## SQL Coalesce function
The SQL Coalesce and IsNull functions are used to handle NULL values. During the expression evaluation process the NULL values are replaced with the user-defined value.

```sql
update  users set first_name = null where id = 58;
select coalesce(first_name, 'no name') as first_name from users where id = 58;
```

### Calculate total salary

```sql
create table salary (
  employee_id int pk,
  hourly_wage int, -- this is nullable
  salary int,
  commission int, -- this is nullable
  num_sales int
)
```

```sql
select employee_id, 
  cast(coalesce(hourly_wage * 40, salary + (comission * num_sales), salary)) as decimal (10, 2) as weekly_salary
  from salary
```

## Resources
https://www.sqlshack.com/using-the-sql-coalesce-function-in-sql-server/