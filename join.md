
### Merging Tables
SQL offers a varierty of approaches to merge table data:

- <code>inner join</code> : intersection between two tables only

#### 1 | Inner Join

- Similar to pandas' <code>merge</code> **'how=inner'** argument option
- We have two tables <code>customer</code> & <code>payment</code>
- Using table <code>references</code>: INNER JOIN table ON column_a = column_b 

```
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
```

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

