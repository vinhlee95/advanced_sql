# Custom data type

```sql
-- ENUM TYPE
CREATE TYPE user_role AS ENUM  ('user', 'staff', 'admin');
ALTER  TABLE users ADD COLUMN  role user_role;

SELECT  * FROM users;

UPDATE users SET role = 'user' WHERE role IS NULL;
UPDATE users SET role = 'admin' WHERE  first_name = 'Foo';

SELECT  first_name, role FROM users WHERE first_name = 'Foo';
```