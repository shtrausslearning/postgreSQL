
# Basic psql setup 

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

## 2 | Creating / Deleting Databases

Only the `user` which created the database, can delete them, user `ben` has permission `CREATE DB`, so we can create a new database

```sql
create database name 
```

And drop the database, when needed

```sql
drop database name
```

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

## 4 | Dump database to sql file

If we need to backup a database, we can use `pg_dump` & load it with `pg_restore` as we did above

```sql
pg_dump dvdrental > /Users/andrey/Desktop/dvdrental.sql 
```

## 5 | Connect to Database

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
