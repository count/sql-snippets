# Count NULLs

Explore this snippet [here](https://count.co/n/YJCjwADi4VY?vm=e).

# Description

Part of the data cleaning process involves understanding the quality of your data. NULL values are usually best avoided, so counting their occurrences is a common operation.
There are several methods that can be used here:
- `count(*) - count(<column>)` - use the different forms of the [`count()`](https://www.postgresql.org/docs/current/functions-aggregate.html) aggregation which include and exclude NULLs.
- `sum(case when x is null then 1 else 0 end)` - create a new column which contains 1 or 0 depending on whether the value is NULL, then aggregate.

```sql
with data as (
  select * from (values (1), (2), (null), (null), (5)) as data (x)
)

select
  count(*) - count(x) with_count,
  sum(case when x is null then 1 else 0 end) with_case
from data
```

| with_count | with_case |
| ---------- | --------- |
| 2          | 2         |
