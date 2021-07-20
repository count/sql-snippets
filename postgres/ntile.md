# Creating equal-sized buckets using ntile

# Description
When you want to create equal sized groups (i.e. buckets) of rows, there is a SQL function for that too.
The function [NTILE](https://www.postgresql.org/docs/13/functions-window.html) lets you do exactly that:

```sql
WITH users AS (
    SELECT md5(RANDOM()::TEXT) AS name_hash,
           CASE WHEN RANDOM() <= 0.5 THEN 'male' ELSE 'female' END AS gender,
           CASE WHEN RANDOM() <= 0.33 THEN 'student' WHEN RANDOM() <= 0.66 THEN 'professor' ELSE 'alumni' END AS status
    FROM generate_series(1, 10)
)

SELECT 
       name_hash,
       gender,
       status,
       NTILE(3) OVER(PARTITION BY status ORDER BY status) AS ntile_bucket
FROM
       users;
```

|name_hash|gender|status|ntile_bucket|
|------|------|------|------|
|8c9882e8cc6f748c06b3a20ee7af9787|female|alumni|1|
|032dffdb7cb67a5f1a6b77b713043508|female|alumni|2|
|72559a7e25935004d172329a47cb3db5|male|alumni|3|
|284d1337a3d30433fe17ad2110d27655|male|professor|1|
|3797bb418424d7ced117e8817f398fea|male|professor|2|
|e56d1e6e53f12c6a133c4f47a962754c|female|professor|3|
|8713e6b5a1c359f42e960c05460c8e14|male|student|1|
|af8ea7ea643fbbfefe7c845e77c9e4d3|male|student|1|
|033161ae441c347c4aa43acd46347836|female|student|2|
|a476b8c2cfa8df1c9494c9851cbd4e66|female|student|3|

