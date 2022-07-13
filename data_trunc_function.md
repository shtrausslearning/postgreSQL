
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
