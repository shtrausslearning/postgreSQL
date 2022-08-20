
## 1 | SELECT 

Usable with clauses:

- Select distinct rows using `DISTINCT` operator
- Sort rows using `ORDER BY`
- Filter rows using `WHERE`
- Select a subset of rows from a table using `LIMIT` 
- Group rows into groups using `GROUP BY`
- Filter groups using `HAVING`
- Join with other tables using joins such as `INNER JOIN`, `LEFT JOIN`, `FULL OUTER JOIN`, `CROSS JOIN`
- Perform set operations using `UNION`, `INTERSECT`, and `EXCEPT`

### Selecting one column 

Select one column `first_name` and limit the number of entries to 4 rows

```sql
dvdrental=> select first_name from customer limit 4;

 first_name 
------------
 Jared
 Mary
 Patricia
 Linda
(4 rows)
```

### Selecting multiple columns

```sql
dvdrental=> select first_name,last_name from customer limit 4;

 first_name | last_name 
------------+-----------
 Jared      | Ely
 Mary       | Smith
 Patricia   | Johnson
 Linda      | Williams
(4 rows)
```

### Selecting all columns in a table

```sql
dvdrental=> select * from customer limit 4;

 customer_id | store_id | first_name | last_name |                email                | address_id | activebool | create_date |       last_update       | active 
-------------+----------+------------+-----------+-------------------------------------+------------+------------+-------------+-------------------------+--------
         524 |        1 | Jared      | Ely       | jared.ely@sakilacustomer.org        |        530 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           1 |        1 | Mary       | Smith     | mary.smith@sakilacustomer.org       |          5 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           2 |        1 | Patricia   | Johnson   | patricia.johnson@sakilacustomer.org |          6 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           3 |        1 | Linda      | Williams  | linda.williams@sakilacustomer.org   |          7 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
(4 rows)
```

### Select multiple columns and merge them into one

Select multiple columns and use an `alias` for the column with `as`

```sql
dvdrental=> select first_name || ' ' || last_name as firs_last from customer limit 4;

    firs_last     
------------------
 Jared Ely
 Mary Smith
 Patricia Johnson
 Linda Williams
(4 rows)
```

<br>

## 2 | ORDER BY

```
ORDER BY sort_expresssion [ASC | DESC] [NULLS FIRST | NULLS LAST]
```

### One column

Order by one colum only `ASC` (by default)

```sql
dvdrental=> select first_name,last_name from customer order by first_name limit 4;

 first_name | last_name 
------------+-----------
 Aaron      | Selby
 Adam       | Gooch
 Adrian     | Clary
 Agnes      | Bishop
(4 rows)
```

Order by one column only `DESC` 

```sql
dvdrental=> select first_name,last_name from customer order by first_name DESC limit 4;

 first_name | last_name 
------------+-----------
 Zachary    | Hite
 Yvonne     | Watkins
 Yolanda    | Weaver
 Wilma      | Richards
(4 rows)
```

Using `nulls last`, if the table has `NaN` values


```sql
dvdrental=> select first_name,last_name from customer order by first_name DESC nulls last limit 4;

 first_name | last_name 
------------+-----------
 Zachary    | Hite
 Yvonne     | Watkins
 Yolanda    | Weaver
 Wilma      | Richards
(4 rows)
```

```sql
dvdrental=> select first_name, length(first_name) as len from customer order by len DESC limit 4;
 first_name  | len 
-------------+-----
 Christopher |  11
 Jacqueline  |  10
 Christine   |   9
 Elizabeth   |   9
(4 rows)
```

### Multiple Columns

```sql
dvdrental=> select first_name,last_name from customer order by first_name ASC, last_name DESC limit 4;

 first_name | last_name 
------------+-----------
 Aaron      | Selby
 Adam       | Gooch
 Adrian     | Clary
 Agnes      | Bishop
(4 rows)
```
