# Replace NULLs

Explore this snippet [here](https://count.co/n/yYCaXXdOESx?vm=e).

# Description

An essential part of cleaning a new data source is deciding how to treat NULL values. The Snowflake function [IFNULL](https://docs.snowflake.com/en/sql-reference/functions/ifnull.html) can help with replacing NULL values with something else:

```sql
with data as (
  select * from (values
    (1,    'one'),
    (null, 'two'),
    (3,    null)
  ) as data (num, str)
)

select
  ifnull(num, -1) num,
  ifnull(str, 'I AM NULL') str
from data
```

| NUM | STR       |
| --- | --------- |
| 1   | a         |
| -1  | b         |
| 3   | I AM NULL |