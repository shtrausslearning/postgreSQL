
### Inner Join

- Similar to pandas' <code>merge</code> **'how=inner'** argument option
- We have two tables <code>customer</code> & <code>payment</code>
- Using table references:

```
INNER JOIN payment p 
    ON p.customer_id = c.customer_id
``

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

