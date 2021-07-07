# Count NULLs

Explore this snippet [here](https://count.co/n/E0yQhZaKdmD?vm=e).

# Description
Part of the data cleaning process involves understanding the quality of your data. NULL values are usually best avoided, so counting their occurrences is a common operation.
There are several methods that can be used here:
- `sum(if(<column> is null, 1, 0)` - use the [`IFF`](https://docs.snowflake.com/en/sql-reference/functions/iff.html) function to return 1 or 0 if a value is NULL or not respectively, then aggregate.
- `count(*) - count(<column>)` - use the different forms of the [`count()`](https://docs.snowflake.com/en/sql-reference/functions/count.html) aggregation which include and exclude NULLs.
- `sum(case when x is null then 1 else 0 end)` - similar to the IFF method, but using a [`CASE`](https://docs.snowflake.com/en/sql-reference/functions/case.html) statement instead.

```sql
with data as (
  select * from (values (1), (2), (null), (null), (5)) as data (x)
)

select
  sum(iff(x is null, 1, 0)) with_iff,
  count(*) - count(x) with_count,
  sum(case when x is null then 1 else 0 end) with_case
from data
```

| WITH_IFF | WITH_COUNT | WITH_CASE |
| -------- | ---------- | --------- |
| 2        | 2          | 2         |