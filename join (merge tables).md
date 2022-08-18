
## Merging Tables

PostgreSQL offers a varierty of approaches to merge table data:

- <code>inner join</code> : intersection between two tables only
- <code>left join</code> : include all left rows data, if right doesn't contain data it is allocated NaN
- <code>right join</code> : include all the right row data, if the left doesn't contain data, it is allocated NaN
- <code>full outer join</code> : include all possible combinations from both tables, if it doesn't exist, allocate NaN

### 1 | `Inner` Join

- Similar to pandas' <code>merge</code> **'how=inner'** argument option
- We include only those entries which are contained in all tables provided

#### MERGE TWO TABLES

- We have two tables <code>customer</code> & <code>payment</code>
- **SELECT** takes in column names from both tables, using table references
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

#### MERGE THREE TABLES

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

#### 2 | `Left` Join

- Similar to pandas' <code>merge</code> **'how=left'** argument option
- Include all left rows data, if right doesn't contain data, it is allocated NaN
- Let's show all the rows that didn't have rows values in the right table

```sql

SELECT
	film.film_id,
	film.title,
	inventory_id
FROM
	film
LEFT JOIN inventory 
   ON inventory.film_id = film.film_id
WHERE inventory.film_id IS NULL
ORDER BY title;

```

#### 3 | `Right` Join

- Similar to pandas' <code>merge</code> **'how=right'** argument option
- <code>films</code> is the **left** table and <code>film_reviews</code> is the **right** table
- We include all the right row data, if the left doesn't contain data, it is allocated NaN

```sql
SELECT 
   review, 
   title
FROM 
   films
RIGHT JOIN film_reviews 
   ON film_reviews.film_id = films.film_id;
```

#### 4 | `Full Outer` Join

- Similar to pandas' <code>merge</code> **'how=outer'** argument option
- Include all possible index combinations from both tables, if it doesn't exist in one table, allocate NaN

```sql

SELECT
	employee_name,
	department_name
FROM
	employees e
FULL OUTER JOIN departments d 
        ON d.department_id = e.department_id
WHERE
	employee_name IS NULL;

```
