## Connect to Database

### Connecting to existing database

When you have a username already setup `psql database -u username`

- Let's connect to the database through user <code>ben</code>
- access symbol has changed to a > from # (no longer using a **Super User** account)

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

Database related commands:

- <code>\list</code> - Lists all the databases in Postgres
- <code>\connect</code> - Connect to a database
- <code>\dt</code> list the Tables in the currently connected database
- <code>\d table</code> show the table information

- Let's connect to a particular database <code>super_awesome_application</code>

```sql
postgres=> \connect super_awesome_application 
```

```
You are now connected to database "super_awesome_application" as user "ben".
```

- When we have some <code>tables</code>, we can call <code>\dt</code>

```sql
postgres=> \dt 
```

- Now we can create, read, update and delete data with the user <code>ben</code>
