# Replace empty strings with NULLs

Explore this snippet [here](https://count.co/n/Fzyv9i4SP2B?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat missing values. The [`CASE`](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-CASE) statement can help with replacing empty values with something else:

```sql
with data as (
  select * from (values ('a'), ('b'), (''), ('d')) as data (str)
)

select
  iff(length(str) = 0, null, str) str
from data
```

| STR  |
| ---- |
| a    |
| b    |
| NULL |
| d    |