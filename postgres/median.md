# Calculating the Median

# Description
Postgres does not have an explicit `MEDIAN` function. The following snippet will show you how to calculate the median of any column.

# Snippet ✂️
Here is a median function with a sample dataset: 

```sql
WITH data AS (
    SELECT CASE
        WHEN RANDOM() <= 0.25 THEN 'apple'
        WHEN RANDOM() <= 0.5 THEN 'banana'
        WHEN RANDOM() <= 0.75 THEN 'pear'
      ELSE 'orange'
    END AS fruit,
    RANDOM()::DOUBLE PRECISION AS weight
    FROM generate_series(1,10)
)

SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY weight) AS median
FROM
  data
```
| median |
| ----------- |
| 0.1358146 |
