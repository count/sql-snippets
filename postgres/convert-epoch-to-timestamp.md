# Replace epoch/unix formatted timestamps with human-readable ones

# Description

Often data sources will include epoch/unix-style timestamps or dates, which are not human-readable. 
Therefore, we want to convert these to a different format, for example to be used in charts or aggregation queries. 
The [`TO_TIMESTAMP`](https://www.postgresql.org/docs/13/functions-formatting.html) function will allow us to do this with little effort:

```sql
WITH data AS (
  SELECT *
  FROM (VALUES (1611176312), (1611176759), (1611176817), (1611176854)) AS data (str)
)

SELECT
  TO_TIMESTAMP(str) AS converted_timestamp
FROM data;
```

| converted_timestamp |
| ---- |
| 2021-01-20 20:58:32.000000 |
| 2021-01-20 21:05:59.000000 |
| 2021-01-20 21:06:57.000000 |
| 2021-01-20 21:07:34.000000 |