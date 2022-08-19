### 1 | Connect to database

- Let's connect to the database through user <code>ben</code>
- Access symbol has changed to a > from # (no longer using a **Super User** account)

```sql
psql postgres -U ben
```

- Once this is done, you need to add at least one user who has permission to access databases (aside from the super users, who can access everything)

```sql
postgres=> GRANT ALL PRIVILEGES ON DATABASE super_awesome_application TO ben; 
postgres=> \list 
```

```
                                  List of databases
           Name            | Owner  | Encoding | Collate | Ctype | Access privileges 
---------------------------+--------+----------+---------+-------+-------------------
 databasename              | andrey | UTF8     | C       | C     | 
 postgres                  | andrey | UTF8     | C       | C     | 
 super_awesome_application | ben    | UTF8     | C       | C     | =Tc/ben          +
                           |        |          |         |       | ben=CTc/ben
 template0                 | andrey | UTF8     | C       | C     | =c/andrey        +
                           |        |          |         |       | andrey=CTc/andrey
 template1                 | andrey | UTF8     | C       | C     | =c/andrey        +
                           |        |          |         |       | andrey=CTc/andrey
 test                      | andrey | UTF8     | C       | C     | 
```

`PostgreSQL` related commands:

- <code>\list</code> - Lists all the databases in Postgres
- <code>\connect</code> - Connect to a database
- <code>\dt</code> list the Tables in the currently connected database
- <code>\d table</code> show the table information

Let's connect to a particular database <code>super_awesome_application</code>

```
postgres=> \connect super_awesome_application 
```

```
You are now connected to database "super_awesome_application" as user "ben".
```

When we have some <code>tables</code>, we can call <code>\dt</code>

```
postgres=> \dt 
```

Now we can create, read, update and delete data with the user <code>ben</code>


### 1 | Create Empty Table

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
