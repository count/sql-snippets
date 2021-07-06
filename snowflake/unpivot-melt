# Unpivot / melt
Explore this snippet with some demo data [here](https://count.co/n/l1c3J7nME8a?vm=e).


# Description
The act of unpivoting (or melting, if you're a `pandas` user) is to convert columns to rows. Snowflake has added the [UNPIVOT](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#unpivot_operator) operator to help.
The query template is

```sql
SELECT
  <columns_to_keep>,
  metric,
  value
FROM <table> UNPIVOT(value FOR metric IN (<columns_to_unpivot>))
order by metric, value
```
where
- `columns_to_keep` - all the columns to be preserved
- `columns_to_unpivot` - all the columns to be converted to rows
- `table` - the table to pull the columns from
(The output column names metric and value can also be changed.)
# Examples
In the examples below, we use a table with columns `A`, `B`, `C`. We take the columns `B` and `C`, and turn their values into columns called `metric` (containing either the string 'B' or 'C') and `value` (the value from either the column `B` or `C`). The column `A` is preserved.

```sql
-- A
-- Define some dummy data
with a as (
  select * from(
    select 'a' as A, 1 as B, 2 as C  union all 
    select  'b' as A, 3 as B, 4 as C union all
    select  'c' as A, 5 as B, 6 as C 
  )
)

SELECT
  A,
  metric,
  value
FROM a UNPIVOT(value FOR metric IN (B, C))
order by metric, value
```
| A | metric | value |
| --- | ----- | ------ |
| a | B | 1 |
| b | B | 3 |
| c | B | 5 |
| a | C | 2 |
| b | C | 4 |
| c | C | 6 |
