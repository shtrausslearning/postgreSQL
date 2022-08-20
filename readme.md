
## 1 | Setup PostgreSQL

First we need to setup <code>PostgreSQL</code>, since we probably will have more than one user accessing the databse

### Start/End Server

The server will exist until it is stopped using `pg_ctl -D ... stop`

```sql
pg_ctl -D /usr/local/var/postgres start # start server 
# pg_ctl -D /usr/local/var/postgres stop  # end server
```

- By default, a `superuser` is created, check which users exist `\du`
- Create additional users, by default the predefined user(s) are <code>super user accounts</code>

```sql
postgres=# \du
```

```sql
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 andrey    | Superuser, Create role, Create DB, Replication, Bypass RLS | {}

```

### Setup password

By default, no password is set for the default `superuser`, let's change the password by typing `\password`

```sql
postgres=# \password andrey
```

### Create new user

Create a new user, <code>ben</code>, by default the list of roles, attributes will be empty, so we need to add them 

```sql
postgres=# CREATE ROLE ben WITH LOGIN PASSWORD 'password'; 
postgres=# \du
```

```sql
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 andrey    | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 ben       |                                                            | {}
```
 
Let's allow the user <code>ben</code> to create databases, using <code>ALTER ROLE</code> user <code>CREATEDB</code>
 

```sql
postgres=# ALTER ROLE ben CREATEDB; 
postgres=# \du 
```


```sql
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 andrey    | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 ben       | Create DB                                                  | {}
```

### Restart session under new username

Quit current session in terminal, server keeps on running, typing <code>\q</code>

```sql
postgres=> \q
```

**Restart session** by typing <code>psql postgres -U username</code> 

```sql
psql postgres -U ben
```

<br>

## 2 | Creating / Deleting Databases

Only the `user` which created the database, can delete them, user `ben` has permission `CREATE DB`, so we can create a new database

```sql
create database name 
```

And drop the database, when needed

```sql
drop database name
```

<br>

## 3 | Restoring database from files

If we need to restore a database, we need to first **create the database** using `create database` and then use `pg_restore`

- We are restoring the files located in dvdrental.tar` via user `andrey` and storing it in database `dvdrental`
- Other options include `sql files or `dump` files

```sql
pg_restore -U andrey -d dvdrental /Users/andrey/Downloads/dvdrental.tar
```

Connect to the databse 

```sql
psql dvdrental -U andrey
```

Show the table schema using `\dt`, `\c` can be used to connect to the corresponding database

```sql
dvdrental=# \dt
```

```
            List of relations
 Schema |     Name      | Type  | Owner  
--------+---------------+-------+--------
 public | actor         | table | andrey
 public | address       | table | andrey
 public | category      | table | andrey
 public | city          | table | andrey
 public | country       | table | andrey
 public | customer      | table | andrey
 public | film          | table | andrey
 public | film_actor    | table | andrey
 public | film_category | table | andrey
 public | inventory     | table | andrey
 public | language      | table | andrey
 public | payment       | table | andrey
 public | rental        | table | andrey
 public | staff         | table | andrey
 public | store         | table | andrey
(15 rows)
```
 
Loading some data from a table `actor`, using `psql`'s `limit`

```sql
dvdrental=# select * from actor limit 5
dvdrental-# ;
 actor_id | first_name |  last_name   |      last_update       
----------+------------+--------------+------------------------
        1 | Penelope   | Guiness      | 2013-05-26 14:47:57.62
        2 | Nick       | Wahlberg     | 2013-05-26 14:47:57.62
        3 | Ed         | Chase        | 2013-05-26 14:47:57.62
        4 | Jennifer   | Davis        | 2013-05-26 14:47:57.62
        5 | Johnny     | Lollobrigida | 2013-05-26 14:47:57.62
(5 rows)
```

The uploader will become the `owner` of the `tables`, who can give rights to other users, check rights using `\z`

```sql
dvdrental=> \z
```

```
                                         Access privileges
 Schema |            Name            |   Type   | Access privileges | Column privileges | Policies 
--------+----------------------------+----------+-------------------+-------------------+----------
 public | actor                      | table    |                   |                   | 
 public | actor_actor_id_seq         | sequence |                   |                   | 
 public | actor_info                 | view     |                   |                   | 
 public | address                    | table    |                   |                   | 
 public | address_address_id_seq     | sequence |                   |                   | 
 public | category                   | table    |                   |                   | 
 public | category_category_id_seq   | sequence |                   |                   | 
 public | city                       | table    |                   |                   | 
 public | city_city_id_seq           | sequence |                   |                   | 
 public | country                    | table    |                   |                   | 
 public | country_country_id_seq     | sequence |                   |                   | 
 public | customer                   | table    |                   |                   | 
 public | customer_customer_id_seq   | sequence |                   |                   | 
 public | customer_list              | view     |                   |                   | 
 public | film                       | table    |                   |                   | 
 public | film_actor                 | table    |                   |                   | 
 public | film_category              | table    |                   |                   | 
 public | film_film_id_seq           | sequence |                   |                   | 
 public | film_list                  | view     |                   |                   | 
 public | inventory                  | table    |                   |                   | 
 public | inventory_inventory_id_seq | sequence |                   |                   | 
 public | language                   | table    |                   |                   | 
 public | language_language_id_seq   | sequence |                   |                   | 
 public | nicer_but_slower_film_list | view     |                   |                   | 
 ```
 
 Give access to other users via `pg_read_all_data` in `psql` (for all tables), this overides the below operations
 
 ```sql
 grant pg_read_all_data to ben;
 ```
 
 If we want to grant access to just one table, we can write
 
 ```sql
 grant select on actor to ben;
 ```
 
 Other options include `SELECT`, `UPDATE`, `INSERT`, `DELETE`
 
 Revoking access to specific operation 
 
```sql
revoke select on actor to ben; 
```
 
 
 Revoking all privileges from user on `table`
 
 ```sql
 revoke all privileges on actor from ben;
 ```
 

<br>

## 4 | Dump database to sql file

If we need to backup a database, we can use `pg_dump` & load it with `pg_restore` as we did above

```sql
pg_dump dvdrental > /Users/andrey/Desktop/dvdrental.sql 
```

<br>

## 5 | Upload CSV file to Table

First, lets create a <code>table</code> header <code>details</code>, which is in **CAPS**

```sql
CREATE TABLE DETAILS(emp_id SERIAL,
first_name   VARCHAR(50),
last_name VARCHAR(50),
dob DATE,
city VARCHAR(40));
```

Next, We can copy data, located in a local file to newly created table <code>details</code>, written in **lower case**

```sql
COPY details(emp_id,first_name,last_name,dob,city)
FROM '/Users/andrey/Documents/data.txt' 
DELIMITER ','
CSV HEADER;
```

### Utilising psycopg2

Utilising `psycopg2` (via python) may seem a little longer, but we can run it via a script

```python

import psycopg2

DATABASE = 'X'    # database [\list] (which we \connect to)
USER = 'X'        # superused id
PASSWORD = 'X'    # password
HOST = 'XXX.X.X.X' # host ip
PORT = 'XXXX'      # port number  
  
conn = psycopg2.connect(database=f'{DATABASE}',
                        user=f'{USER}', password=f'{PASSWORD}', 
                        host=f'{HOST}', port=f'{PORT}'
)
  
conn.autocommit = True
cursor = conn.cursor()
  
# Create Table Header (SQL query)
sql = '''CREATE TABLE DETAILS(emp_id SERIAL,
first_name   VARCHAR(50),
last_name VARCHAR(50),
dob DATE,
city VARCHAR(40));'''

cursor.execute(sql)
  
# Copy Data into Table (SQL query)
sql2 = '''COPY details(emp_id,first_name,last_name,dob,city)
FROM '/Users/andrey/Documents/data.txt' # absolute path to file
DELIMITER ','
CSV HEADER;'''
  
cursor.execute(sql2)
  
# Fetch the table data (SQL query)
sql3 = '''select * from details;'''
cursor.execute(sql3)
for i in cursor.fetchall():
    print(i)
  
conn.commit()
conn.close()    

```

Confirm the data has been uploaded via 

```sql
TABLE details;
```

```
 emp_id | first_name | last_name |    dob     |   city   
--------+------------+-----------+------------+----------
      1 | Max        | Smith     | 2002-02-03 | Sydney
      2 | Karl       | Summers   | 2004-04-10 | Brisbane
      3 | Sam        | Wilde     | 2005-02-06 | Perth
```

### Table operations

List databases <code>\list</code> & Connect to databse <code>\connect</code>:

- <code>\dt</code> - show tables 
- <code>CREATE TABLE DETAILS((table formats))</code> create table header
- <code>TABLE table_name</code> view table stored in database
- <code>DROP table_name</code> remove table from database 

<br>

## 6 | Dumping Tables in CSV format

`CSV` files are commonly used with `pandas`, we may need to use them with `psql` as well

### Create an empty table

First create a header/empty table with the corresponding **column names**, **types** & 

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

### Export entire table from Database

Copy entire table <code>persons</code> from database to local server using `copy`

```sql
copy persons 
to 'C:\tmp\persons_db.csv' 
delimiter ',' CSV HEADER;
```

### Export only specific data from table

Copy only selected columns <coed>first_name</code>,<code>last_name</code>,<code>email</codE> from the table <code>persons</code>

```sql
COPY persons(first_name,last_name,email) 
TO 'C:\tmp\persons_partial_db.csv' 
DELIMITER ',' CSV HEADER;
```

### Export without a header 

Export table without a <code>header</code>
  
```sql
copy persons(email) 
to 'C:\tmp\persons_email_db.csv' 
delimiter ',' CSV;
```

### Using user copy command `\copy`

Using <code>\copy</code> statement, we can also do the same thing

```sql
\copy (SELECT * FROM persons) to 'C:\tmp\persons_client.csv' with csv
```

<br>

## 7 | Connect to Database

When you have a username already setup `psql database -u username`

- Let's connect to the database through user <code>ben</code>
- `Access symbol` has changed to a > from # (no longer using a **Super User** account)

```sql
psql postgres -U ben
```

Once this is done, you need to add at least one user who has permission to access databases (aside from the super users, who can access everything)

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

### Important database commands:

- <code>\l</code> - Lists all the databases in Postgres
- <code>\c</code> - Connect to a database
- <code>\dt</code> list the Tables in the currently connected database
- <code>\d table</code> show the table information
- <code>\z table</code> show table information with accessibility options

Let's connect to a particular database <code>super_awesome_application</code>

```sql
postgres=> \c super_awesome_application 
```

```
You are now connected to database "super_awesome_application" as user "ben".
```

When we have some <code>tables</code>, we can call <code>\dt</code>

```sql
postgres=> \dt 
```

Now we can create, read, update and delete data with the user <code>ben</code>
