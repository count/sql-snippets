# Replace empty strings with NULLs

Explore this snippet [here](https://count.co/n/g0Q8V8oSnB8?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat missing values. The BigQuery function [IF](https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#if) can help with replacing empty values with something else:

```sql
with data as (
  select * from unnest(['a', 'b', '', 'd']) as str
)

select
  if(length(str) = 0, null, str) str
from data
```

| str  |
| ---- |
| a    |
| b    |
| NULL |
| d    |