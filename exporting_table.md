
- copy entire table <code>persons<< from database to local server 

```sql
COPY persons TO 'C:\tmp\persons_db.csv' DELIMITER ',' CSV HEADER;
```

- copy only selected columns from the table <code>persons</code>

```sql
COPY persons(first_name,last_name,email) 
TO 'C:\tmp\persons_partial_db.csv' DELIMITER ',' CSV HEADER;
```

```sql
COPY persons(email) 
TO 'C:\tmp\persons_email_db.csv' DELIMITER ',' CSV;
```
