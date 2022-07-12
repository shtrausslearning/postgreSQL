
# Detecting Anomalities

## I. Mathematical Foundation 

### 1. Mean Value

- Let's calculate the mean value of an array n

```SQL
SELECT avg(n)
FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n;
```

```
 
        avg         
--------------------
 4.3333333333333333
(1 row)
```

### 2. Standard Deviation

- Let's calculate the standard deviation of an array n

```SQL
SELECT stddev(n)
FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n;
```

```
       stddev       
--------------------
 3.0822070014844882
(1 row)
```

### 3. Get the range of accepable values

```SQL
SELECT
   avg(n) - stddev(n) AS lower_bound,
   avg(n) + stddev(n) AS upper_bound
FROM
   unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n;
```

```
    lower_bound     |    upper_bound     
--------------------+--------------------
 1.2511263318488451 | 7.4155403348178215
```
\d table

### 4. Final Code

- Using the query, we found that the value 12 is outside the range of acceptable values and identified it as an anomaly

```SQL
WITH series AS (
   SELECT *
   FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n
),
bounds AS (
   SELECT
       avg(n) - stddev(n) AS lower_bound,
       avg(n) + stddev(n) AS upper_bound
   FROM
       series
)
SELECT
   n,
   n NOT BETWEEN lower_bound AND upper_bound AS is_anomaly
FROM
   series,
   bounds;
   
```

```
 n  | is_anomaly 
----+------------
  2 | f
  3 | f
  5 | f
  2 | f
  3 | f
 12 | t
  5 | f
  3 | f
  4 | f
(9 rows)
```

## II. Understanding Z-Score

- Another way to represent a range of acceptable values is using a <code>z-score</code>
- <code>Z-score</code>, sometimes called **standard score**, is the **number of standard deviations from the mean** 
- In the previous section, our acceptable range was one standard deviation from the mean, or in other words, a z-score in the range ±1

```SQL
WITH series AS (
   SELECT *
   FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n
),
stats AS (
   SELECT
       avg(n) series_mean,
       stddev(n) as series_stddev
   FROM
       series
)
SELECT
   n,
   (n - series_mean) / series_stddev as zscore
FROM
   series,
   stats;
```

- Like before, we can detect anomalies by searching for values that are outside the acceptable range using the <code>z-score</code>
- Using the z-score, we identified 12 as an anomaly in this series

```SQL
WITH series AS (
   SELECT *
   FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n
),
stats AS (
   SELECT
       avg(n) series_avg,
       stddev(n) as series_stddev
   FROM
       series
),
zscores AS (
   SELECT
       n,
       (n - series_avg) / series_stddev AS zscore
   FROM
       series,
       stats
)
SELECT
   *,
   zscore NOT BETWEEN -1 AND 1 AS is_anomaly
FROM
   zscores;
```

```
n  |         zscore          | is_anomaly 
----+-------------------------+------------
  2 | -0.75703329861022517346 | f
  3 | -0.43259045634870009448 | f
  5 |  0.21629522817435006346 | f
  2 | -0.75703329861022517346 | f
  3 | -0.43259045634870009448 | f
 12 |      2.4873951240050256 | t
  5 |  0.21629522817435006346 | f
  3 | -0.43259045634870009448 | f
  4 | -0.10814761408717501551 | f
(9 rows)
```

## III. Optimising Z-Score

- Learn how to optimize the Z-score in a way that anomaly detection becomes easier
- So far, we used **one standard deviation from the mean** or a z-score of ±1 to identify anomalies 
- Changing the **z-score threshold** can affect our results
- For example, let’s see what anomalies we identify:
    - When the z-score is greater than 0.5
    - When it is greater than 3.0

```SQL
WITH series AS (
   SELECT *
   FROM unnest(array[2, 3, 5, 2, 3, 12, 5, 3, 4]) AS n
),
stats AS (
   SELECT
       avg(n) series_avg,
       stddev(n) as series_stddev
   FROM
       series
),
zscores AS (
   SELECT
       n,
       (n - series_avg) / series_stddev AS zscore
   FROM
       series,
       stats
)
SELECT
   *,
   zscore NOT BETWEEN -0.5 AND 0.5 AS is_anomaly_0_5,
   zscore NOT BETWEEN -1 AND 1 AS is_anomaly_1,
   zscore NOT BETWEEN -3 AND 3 AS is_anomaly_3
FROM
   zscores;
```
    
```
 n  |         zscore          | is_anomaly_0_5 | is_anomaly_1 | is_anomaly_3 
----+-------------------------+----------------+--------------+--------------
  2 | -0.75703329861022517346 | t              | f            | f
  3 | -0.43259045634870009448 | f              | f            | f
  5 |  0.21629522817435006346 | f              | f            | f
  2 | -0.75703329861022517346 | t              | f            | f
  3 | -0.43259045634870009448 | f              | f            | f
 12 |      2.4873951240050256 | t              | t            | f
  5 |  0.21629522817435006346 | f              | f            | f
  3 | -0.43259045634870009448 | f              | f            | f
  4 | -0.10814761408717501551 | f              | f            | f
(9 rows)
```

- Let’s see what we got:
    - When we decreased the z-score threshold to 0.5, we identified the value 2 as an anomaly in addition to the value 12
    - When we increased the z-score threshold to 3, we did not identify any anomaly
The quality of our results is directly related to the parameters we set for the query 
    - Later we will see how using backtesting can help us identify ideal values
