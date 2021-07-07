# Count NULLs

Explore this snippet [here](https://count.co/n/vGNUC7SOJei?vm=e).

# Description

Part of the data cleaning process involves understanding the quality of your data. NULL values are usually best avoided, so counting their occurrences is a common operation.
There are several methods that can be used here:
- `sum(if(<column> is null, 1, 0)` - use the [`IF`](https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#if) function to return 1 or 0 if a value is NULL or not respectively, then aggregate.
- `count(*) - count(<column>)` - use the different forms of the [`count()`](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#count) aggregation which include and exclude NULLs.
- `sum(case when x is null then 1 else 0 end)` - similar to the IF method, but using a [`CASE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#case_expr) statement instead.

```sql
with data as (
  select * from unnest([1, 2, null, null, 5]) as x
)

select
  sum(if(x is null, 1, 0)) with_if,
  count(*) - count(x) with_count,
  sum(case when x is null then 1 else 0 end) with_case
from data
```

| with_if | with_count | with_case |
| ------- | ---------- | --------- |
| 2       | 2          | 2         |