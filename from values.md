
- VALUES provides a way to generate a “constant table” 
- Can be used in a query **without having to actually create and populate a table** on-disk

```sql
VALUES (1, 'one'), (2, 'two'), (3, 'three');
```

```sql
=> SELECT * FROM (VALUES (1, 'one'), (2, 'two'), (3, 'three')) AS t (num,letter);
 num | letter
-----+--------
   1 | one
   2 | two
   3 | three
(3 rows)
```

- instead of:

```sql
SELECT 1 AS column1, 'one' AS column2
UNION ALL
SELECT 2, 'two'
UNION ALL
SELECT 3, 'three';
```
