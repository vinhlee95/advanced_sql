# Bulk insert and export from (to) CSV

```sql
CREATE TABLE users (
  id int primary key,
  first_name varchar(255),
  last_name varchar(255),
  email varchar(255),
  gender varchar(20),
  age int,
  language varchar(255)
)

-- Bulk insert data from CSV
COPY users (id, first_name, last_name, email, gender, age, language) FROM
'path_to_csv_file/my_data.csv'
DELIMITER  ',' CSV HEADER;

-- Bulk export data to CSV
COPY users (id, first_name, last_name, email, gender, age, language) TO
'path_to_csv_file/my_data.csv'
DELIMITER  ',' CSV HEADER;
```