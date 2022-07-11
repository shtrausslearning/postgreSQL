
### Inner Join

- Similar to pandas' <code>merge</code> **'how=inner'**

```sql

SELECT
	pka,
	c1,
	pkb,
	c2
FROM
	A
INNER JOIN B ON pka = fka;

```

