
```sql
SELECT DATE_TRUNC('hour', TIMESTAMP '2017-03-17 02:09:30');
```

```
     date_trunc
---------------------
 2017-03-17 02:00:00
(1 row)
```

```sql
SELECT date_trunc('minute', TIMESTAMP '2017-03-17 02:09:30');
```

```
 date_trunc
---------------------
 2017-03-17 02:09:00
(1 row)
```

```sql

SELECT
	staff_id,
	date_trunc('year', rental_date) y,
	COUNT (rental_id) rental
FROM
	rental
GROUP BY
	staff_id, y
ORDER BY
	staff_id
     
```

```
 staff_id |          y          | rental
----------+---------------------+--------
        1 | 2006-01-01 00:00:00 |     85
        1 | 2005-01-01 00:00:00 |   7955
        2 | 2005-01-01 00:00:00 |   7907
        2 | 2006-01-01 00:00:00 |     97
(4 rows)
```
