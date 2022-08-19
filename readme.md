### Setup PostgreSQL

First we need to setup <code>PostgreSQL</code>

#### Start Server

```sql
pg_ctl -D /usr/local/var/postgres start # start server 
# pg_ctl -D /usr/local/var/postgres stop  # end server
```

- By default, a superuser is created, check which users are installed
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

By default, no password is set for the default <code>superuser</code>, let's change the password by typing <code>\password</code>

```sql
postgres=# \password andrey
```

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

Quit current session in terminal, server keeps on running, typing <code>\q</code>

```sql
postgres=> \q
```

- **Restart session** by typing <code>psql postgres -U username</code> 

```sql
psql postgres -U ben
```
