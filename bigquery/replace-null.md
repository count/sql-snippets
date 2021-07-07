# Replace NULLs

Explore this snippet [here](https://count.co/n/HrbhKQX4JfT?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat NULL values. The BigQuery function [IFNULL](https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#ifnull) can help with replacing NULL values with something else:

```sql
with data as (
  select * from unnest([
    struct(1    as num, 'a'  as str),
    struct(null as num, 'b'  as str),
    struct(3    as num, null as str)
  ])
)

select
  ifnull(num, -1) num,
  ifnull(str, 'I AM NULL') str,
from data
```

| num | str       |
| --- | --------- |
| 1   | a         |
| -1  | b         |
| 3   | I AM NULL |