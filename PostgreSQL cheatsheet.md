
#### 1 | Managing Tables

- Create new <code>table</code> using <code>CREATE TABLE</code>

```SQL
CREATE TABLE IF NOT EXISTS table_name(
    SERIAL PRIMARY KEY,
    c1 type NOT NULL,
    c1 type NULL,
    ...
)
```

- Modify the <code>table</code> using <code>ALTER TABLE</code>

```sql
ALTER TABLE table_nameID ADD COLUMN new_column_name TYPE;
ALTER TABLE table_nameID DROP COLUMN new_column_name;
ALTER TABLE table_nameID RENAME column_name TO new_column_name;
ALTER TABLE table_nameID ALTER COLUMN SET DEFAULT value
ALTER TABLE table_nameID ALTER COLUMN DROP DEFAULT
ALTER TABLE table_nameID ADD PRIMARY KEY (column)

ALTER TABLE table_nameID
DROP CONSTRAINT primary_key_name

ALTER TABLE table_nameID RENAME TO new_table_name
DROP TABLE IF EXISTS table_name CASCADE

```

#### 2 | Managing Views

#### 3 | Query data from tables

- Query **all data** from a table

```sql
SELECT * FROM table_nameID
```

- Query data from a **specified column** of all rows in a table

```sql

SELECT column_list 
FROM table_nameID

```

- Query data and select only unique rows

```sql
SELECT DISTINCT (column)
FROM table_nameID
```

- Query data from a <code>table</code> with a <code>filter</code>

```sql

SELECT *
FROM table_nameID
WHERE condition

```

- Select an alias to a column in the result set

```sql
SELECT column_1 AS new_column_1, ...
FROM table_nameID
```

- Query data using the <code>LIKE</code> operator
- Selects rows with column containing "value" subset

```sql
SELECT * FROM table_nameID
WHERE column LIKE '%value%'
```

- Query data using the <code>BETWEEN</code> operator

```sql
SELECT * FROM table_nameID
WHERE column BETWEEN low AND high;
```

- Query data using the <code>IN</code> operator
- Select rows if value1,value2.. are in <code>column</code>

```sql
SELECT * FROM table_nameID
WHERE column IN (value1,value2,...)
```

- Limit the numer of entries shown (postgreSQL only)

```sql
SELECT * FROM table_nameID
LIMIT limit_id OFFSET offset_id
ORDER BY column_name_id;
```

- Merging tables works similar to pandas' <code>merge</code> 
- Merging tables <code>inner join</code>, <code>left join</code>, <code>right join</code>, <code>full outer join</code>

```sql
SELECT * 
FROM table1
INNER JOIN table2 ON conditions
```

```sql
SELECT * 
FROM table1
LEFT JOIN table2 ON conditions
```

```sql
SELECT * 
FROM table1
RIGHT JOIN table2 ON conditions
```

```sql
SELECT * 
FROM table1
FULL OUTER JOIN table2 ON conditions
```

- Count the number of rows in a <code>table</code>

```sql
SELECT COUNT (*)
FROM table_id;
```

- Sort rows (pandas' <code>sort_values</code>) using <code>ORDER BY</code>

```sql
SELECT select_list
FROM table_id
ORDER BY column_id ASC [DESC], column_id2 ASC [DESC],...;
```

- Groupby clause, <code>GROUP BY</code>

```sql
SELECT *
FROM table_id
GROUP BY column_1, column_2, ...;
```

- <code>HAVING</code> **clause**

```sql
SELECT *
FROM table
GROUP BY column_1
HAVING condition;
```

#### 4 | Set operations

- Merge <code>queries</code> together 

```sql
SELECT * FROM table1
UNION
SELECT * FROM table2;
```

- Get intersection of <code>query</code> results

```sql
SELECT * FROM table1
INTERSECT
SELECT * FROM table2;
```

#### 5 | Modifying Data

- Insert new row into <code>table</code>

```sql
INSERT INTO table_id(column1,column2,...)
VALUES(value_1,value_2,...);
```

- Insert <code>values</code> into <code>table</code>

```sql
INSERT INTO table_id(column1,column2,...)
VALUES(value_1,value_2,...),
      (value_1,value_2,...),
      (value_1,value_2,...)...
```

- Update <code>table</code> rows ()for all rows)

```sql
UPDATE table_id
SET column_1 = value_1,
    ...;
```

- Update <code>table</code> rows based on a condition 

```sql

UPDATE table_id
SET column_1 = value_1,
    ...
WHERE condition;

```

- Delete rows 

```sql
DELETE FROM table_id;

DELETE FROM table_id    
WHERE condition;
```

#### 6 | SQL data types

Exact numerical data types

- int	-2,147,483,648	2,147,483,647
- bigint	-9,223,372,036,854,775,808	9,223,372,036,854,775,807
- smallint	-32,768	32,767
- tinyint	0	255
- bit	0	1
- decimal	-10^38 +1	10^38 -1
- numeric	-10^38 +1	10^38 -1

Approximate numerical data types

- float	-1.79E + 30	1.79E + 308
- real	-3.40E + 38	3.40E + 38

Date & Time

- datetime	Jan 1, 1753	Dec 31, 9999
- smalldatetime	Jan 1, 1900	Jun 6, 2079
- date	Stores a date like June 30, 1991	-
- time	Stores a time like 12:30 P.M.	

#### 7 | SQL Constraint 

- NOT NULL
    - Ensures that a column cannot have a NULL value
- DEFAULT
    - Provide a default value in a column when none is specified
- UNIQUE 
    - Ensures that all values in a column are unique 
- PRIMARY 
    - A primary key is a field that uniquely identifies each row/record in a database table
- FOREIGN 
    - Uniquely identifies a row/record in any other database table. A foreign key is a key used to link two tables together
- CHECK
    - Ensures that all values in a column satisfy certain conditions   

#### 8 | CREATE, DROP & USE DATABASE

```sql
CREATE DATABASE database;
DROP DATABASEA database;
SHOW DATABASES;
USE testDB1; 
```

#### 9 | CREATE, DROP & USE TABLE

- General SQL operations

```sql
CREATE TABLE CUSTOMERS(
  ID   INT              NOT NULL,
  NAME VARCHAR (20)     NOT NULL,
  AGE  INT              NOT NULL,
  ADDRESS  CHAR (25) ,
  SALARY   DECIMAL (18, 2),  /* The (18,2) simply means that we can have 18 digits with 2 of them after decimal point*/
  PRIMARY KEY (ID)
);

DESC CUSTOMERS;

DROP TABLE CUSTOMERS;

DESC CUSTOMERS;
);
```

```
Field	Type	Null	Key	Default	Extra
ID	int(11)	NO	PRI	NULL	
NAME	varchar(20)	NO		NULL	
AGE	int(11)	NO		NULL	
ADDRESS	char(25)	YES		NULL	
SALARY	decimal(18,2)	YES		NULL	
```
