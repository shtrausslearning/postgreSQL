
### Weighted mean

- Using a <code>z-score</code> for detecting anomalies
    - An easy way to get started with **anomaly detection** and see results right away
- But this method is not always the best choice
    - If you do not get **good alerts** using this method, there are some improvements and other methods you can try using just SQL

- Our system uses:
    - a <code>mean</code> to determine a reasonable value 
    - **a lookback period** to determine how far back to calculate the mean
    
- In our case, we calculated the <code>mean</code> based on **data from 1 hour ago**
- Using this method of calculating <code>mean</code> gives the **same weight to entries that happened 1 hour ago** and to **entries that just happened** 

- Instead:
    - Give more weight to recent entries at the expense of previous entries
    - The **new weighted mean** should become more sensitive to recent entries, 
    - You may be able to identify anomalies quicker
    - To give more weight to recent entries, you can use a weighted average:

```SQL
SELECT
   status_code,
   avg(entries) as mean,
   sum(
      entries *
      (60 - extract('seconds' from '2020-08-01 17:00 UTC'::timestamptz - period))
   ) / (60 * 61 / 2) as weighted_mean
FROM
   server_log_summary
WHERE
   -- Last 60 periods
   period > '2020-08-01 17:00 UTC'::timestamptz
GROUP BY
   status_code;
```

```
CREATE TABLE
COPY 2892
 status_code |          mean          |   weighted_mean   
-------------+------------------------+-------------------
         500 | 0.15000000000000000000 | 0.295081967213115
         200 |  2779.1000000000000000 |  5467.08196721311
         404 | 0.13333333333333333333 | 0.262295081967213
         400 | 0.73333333333333333333 |  1.44262295081967
(4 rows)
```

- You can see the difference between the mean and the weighted mean for each status code in the results.
- A weighted average is a very common indicator used by stock traders. 
- We used a linear weighted average, but there are also 
    - exponentially weighted averages etc
    
## Mean Absolute Deviation

- **Median absolute deviation** (<code>MAD</code>) is another way of finding anomalies in a series
- MAD is considered better than the z-score for real-life data
- MAD is calculated by finding the median of the deviations from the series median. 
- Just for comparison, the standard deviation is the root square of the average square distance from the mean

## Different measures 

- We used the number of entries per minute as an indicator
- However, depending on the use case, there might be other things you can measure that can yield better results. For example:
    - To identify DOS (Denial-of-Service) attacks, you can monitor the ratio between unique IP addresses to HTTP requests
- To reduce the number of false positives, you can normalize the responses to the proportion of the total responses
-  This way, for example, if you are using a flaky remote service that fails once after every certain amount of requests, using the proportion may not trigger an alert when the increase in errors correlates with an increase in overall traffic

## Conclusion

- The methods you learned in this course are straightforward methods to detect anomalies and produce actionable alerts that can save you much grief
- There are many tools out there that provide similar functionality, but they require tight integration or money
- The main appeal of this approach is that you can get started with tools you probably already have, some SQL, and a scheduled task!