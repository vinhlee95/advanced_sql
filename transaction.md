# SQL Transaction

## Keywords
`BEGIN, COMMIT, SAVEPOINT, ROLLBACK`

```shell
advanced_sql=# begin;
BEGIN
advanced_sql=*# update users set first_name = 'foo' where id = 58;
UPDATE 1
advanced_sql=*# savepoint insert_save_point;
SAVEPOINT
advanced_sql=*# insert into users values (1000, 'bar', 'foo');
INSERT 0 1
advanced_sql=!# rollback to insert_save_point;
ROLLBACK
advanced_sql=*# commit;
COMMIT
advanced_sql=# select * from users where id = 58;
 id | first_name | last_name |         email          | gender | age | language | role
----+------------+-----------+------------------------+--------+-----+----------+------
 58 | foo        | Gockeler  | wgockeler1l@eepurl.com | Male   |  66 | Thai     | user
(1 row)

advanced_sql=# select * from users where id = 1000;
 id | first_name | last_name | email | gender | age | language | role
----+------------+-----------+-------+--------+-----+----------+------
(0 rows)
```


## Resources
https://docs.microsoft.com/en-us/sql/t-sql/language-elements/begin-transaction-transact-sql?view=sql-server-ver15#permissions
