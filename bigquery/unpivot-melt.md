# Unpivot / melt

Explore this snippet with some demo data [here](https://count.co/n/xwGc7kFuAm0?vm=e).

# Description
The act of unpivoting (or melting, if you're a `pandas` user) is to convert columns to rows. BigQuery has recently added the [UNPIVOT](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#unpivot_operator) operator as a preview feature.
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

(The output column names 'metric' and 'value' can also be changed.)
# Examples
In the examples below, we use a table with columns `A`, `B`, `C`. We take the columns `B` and `C`, and turn their values into columns called `metric` (containing either the string 'B' or 'C') and `value` (the value from either the column `B` or `C`). The column `A` is preserved.

```sql
-- Define some dummy data
with a as (
  select * from unnest([
    struct( 'a' as A, 1 as B, 2 as C ),
    struct( 'b' as A, 3 as B, 4 as C ),
    struct( 'c' as A, 5 as B, 6 as C )
  ])
)
SELECT
  A,
  metric,
  value
FROM a UNPIVOT(value FOR metric IN (B, C))
order by metric, value
```


If you would rather not use preview features in BigQuery, there is another generic solution (from [StackOverflow](https://stackoverflow.com/a/47657770)), though it may be much less performant than the built-in `UNPIVOT` implementation.
The query template here is

```sql
select
  <columns_to_keep>,
  metric,
  value
from (
  SELECT
    *,
    REGEXP_REPLACE(SPLIT(pair, ':')[OFFSET(0)], r'^"|"$', '') metric, 
    REGEXP_REPLACE(SPLIT(pair, ':')[OFFSET(1)], r'^"|"$', '') value 
  FROM
    <table>,
    UNNEST(SPLIT(REGEXP_REPLACE(to_json_string(<table>), r'{|}', ''))) pair
)
where metric not in (<column_names_to_keep>)
order by metric, value
```
where
- `column_names_to_keep` - this is a list of the JSON-escaped names of the columns to keep

An annotated example is:

```sql
-- Define some dummy data
with a as (
  select * from unnest([
    struct( 'a' as A, 1 as B, 2 as C ),
    struct( 'b' as A, 3 as B, 4 as C ),
    struct( 'c' as A, 5 as B, 6 as C )
  ])
)

-- See https://stackoverflow.com/a/47657770

select
  A, -- Keep A column
  metric,
  value
from (
  SELECT
    *, -- Original table

    -- Do some manual JSON parsing of a string which looks like '"B": 3'
    REGEXP_REPLACE(SPLIT(pair, ':')[OFFSET(0)], r'^"|"$', '') metric, 
    REGEXP_REPLACE(SPLIT(pair, ':')[OFFSET(1)], r'^"|"$', '') value 
  FROM
    a,
    -- 1. Create a row which contains a string like '{ "A": "a", "B": 1, "C": 2 }'
    -- 2. Remove the outer {} characters
    -- 3. Split the string on , characters
    -- 4. Join the new array onto the original table
    UNNEST(SPLIT(REGEXP_REPLACE(to_json_string(a), r'{|}', ''))) pair
)
where metric not in ('A') -- Filter extra rows that we don't need
order by metric, value
```