
### 1 | Import Table 

- First create a header/empty table with the corresponding **column names**, **types** & 

```sql
CREATE TABLE persons (
  id SERIAL,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  dob DATE,
  email VARCHAR(255),
  PRIMARY KEY (id)
)
```

### 2 | Export Table Data from Database

- Copy entire table <code>persons</code> from database to local server 

```sql
COPY persons 
TO 'C:\tmp\persons_db.csv' 
DELIMITER ',' CSV HEADER;
```

- Copy only selected columns <coed>first_name</code>,<code>last_name</code>,<code>email</codE> from the table <code>persons</code>

```sql
COPY persons(first_name,last_name,email) 
TO 'C:\tmp\persons_partial_db.csv' 
DELIMITER ',' CSV HEADER;
```

- Export table without a <code>header</code>
  
```sql
COPY persons(email) 
TO 'C:\tmp\persons_email_db.csv' 
DELIMITER ',' CSV;
```

- Using <code>\copy</code> statement, we can also do the same thing

```sql
\copy (SELECT * FROM persons) to 'C:\tmp\persons_client.csv' with csv
```
