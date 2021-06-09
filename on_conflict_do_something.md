# On Conflict Do Something Clause

```sql
-- Make "email" column to be unique
ALTER  TABLE users ADD UNIQUE (email);

-- Do nothing on conflicts
INSERT INTO users (id, first_name, last_name, email, gender, age, language) values
(1100, 'FOo', 'Bar', 'foobar@hostgator.com', 'Male', 20, 'Vietnamese') ON CONFLICT  DO NOTHING ;

-- Upsert
INSERT INTO users (id, first_name, last_name, email, gender, age, language) values
(1100, 'FOo', 'Bar', 'foobar@hostgator.com', 'Male', 20, 'Vietnamese')
ON CONFLICT (email) DO UPDATE
SET id = excluded.id, first_name = excluded.first_name, last_name = excluded.last_name,
    email = excluded.email, gender = excluded.gender, age = excluded.age, language = excluded.language;

SELECT * FROM users WHERE email = 'foobar@hostgator.com';
```