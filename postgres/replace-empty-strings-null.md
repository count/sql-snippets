# Replace empty strings with NULLs

Explore this snippet [here](https://count.co/n/Fzyv9i4SP2B?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat missing values. The [`CASE`](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-CASE) statement can help with replacing empty values with something else:

```sql
WITH data AS (
  SELECT *
  FROM (VALUES ('a'), ('b'), (''), ('d')) AS data (str)
)

SELECT
  CASE WHEN LENGTH(str) != 0 THEN str END AS str
FROM data;
```

| str  |
| ---- |
| a    |
| b    |
| NULL |
| d    |