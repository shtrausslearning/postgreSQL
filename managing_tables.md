#### 1 | PostgreSQL Managing Tables

```sql
CREATE TABLE accounts (
	user_id serial PRIMARY KEY,
	username VARCHAR ( 50 ) UNIQUE NOT NULL,
	password VARCHAR ( 50 ) NOT NULL,
	email VARCHAR ( 255 ) UNIQUE NOT NULL,
	created_on TIMESTAMP NOT NULL,
        last_login TIMESTAMP 
);
```

#### 2 | Constraints

Set during CREATE TABLE column name type constraint

- <code>NOT NULL</code> – ensures that values in a column cannot be NULL.
- <code>UNIQUE</code> – ensures the values in a column unique across the rows within the same table.
- <code>PRIMARY KEY</code> – a primary key column uniquely identify rows in a table. A table can have one and only one primary key. The primary key constraint allows you to define the primary key of a table.
- <code>CHECK</code> – a CHECK constraint ensures the data must satisfy a boolean expression.
- <code>FOREIGN KEY</code> – ensures values in a column or a group of columns from a table exists in a column or group of columns in another table. Unlike the primary key, a table can have many foreign keys.
