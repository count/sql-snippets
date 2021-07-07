# Replace NULLs

Explore this snippet [here](https://count.co/n/hpKKsv0U2Rv?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat NULL values. The Postgres function [COALESCE](https://www.postgresql.org/docs/current/functions-conditional.html) can help with replacing NULL values with something else:

```sql
with data as (
  select * from (values
    (1,    'one'),
    (null, 'two'),
    (3,    null)
  ) as data (num, str)
)

select
  coalesce(num, -1) num,
  coalesce(str, 'I AM NULL') str
from data
```

| num | str       |
| --- | --------- |
| 1   | a         |
| -1  | b         |
| 3   | I AM NULL |