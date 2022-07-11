
### Merging Tables
SQL offers a varierty of approaches to merge table data:

- <code>inner join</code> : intersection between two tables only

#### 1 | Inner Join

- Similar to pandas' <code>merge</code> **'how=inner'** argument option
- Using table <code>references</code>: INNER JOIN table ON column_a = column_b 

```
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
```

a) Joining two tables

- We have two tables <code>customer</code> & <code>payment</code>
- **SELECT** takes in column names from both tables, using table references

```sql
SELECT
	c.customer_id,
	first_name,
	last_name,
	email,
	amount,
	payment_date
FROM
	customer c
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
WHERE
    c.customer_id = 2;
```

b) Joining three tables

- We have three tables <code>customer</code>, <code>payment</code> & <code>staff</code>
- In this case, we'll use two inner join operations, **p** and **c** then **p** and **s**

```
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
INNER JOIN staff s 
    ON p.staff_id = s.staff_id
```

```sql

SELECT
	c.customer_id,
	c.first_name customer_first_name,
	c.last_name customer_last_name,
	s.first_name staff_first_name,
	s.last_name staff_last_name,
	amount,
	payment_date
FROM
	customer c
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
INNER JOIN staff s 
    ON p.staff_id = s.staff_id
ORDER BY payment_date;

```
