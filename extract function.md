
```sql
WITH stats AS (
   SELECT
       status_code,
       (MAX(ARRAY[EXTRACT('epoch' FROM period), entries]))[2] AS last_value,
       AVG(entries) AS mean_entries,
       STDDEV(entries) AS stddev_entries
   FROM
       server_log_summary
   WHERE
        period > '2020-08-01 17:00 UTC'::timestamptz
   GROUP BY
       status_code
)
SELECT * FROM stats;
```

- We have this, where period is of `timestamptz` type

```sql
EXTRACT('epoch' FROM period)
```
