# Running total

Explore this snippet with some demo data [here](https://count.co/n/xFHD89xBRAU?vm=e).

# Description
Running totals are relatively simple to calculate in BigQuery, using the `sum` window function.
The template for the query is

```sql
select
  sum(<value>) over (partition by <fields> order by <ordering> asc) running_total,
  <fields>,
  <ordering>
from <table>
```
where 
- `value` - this is the numeric quantity to calculate a running total for
- `fields` - these are zero or more columns to split the running totals by
- `ordering` - this is the column which determines the order of the running total, most commonly temporal
- `table` - where to pull these columns from
# Example
Using total Spotify streams as an example data source, let's identify:
- `value` - this is `streams`
- `fields` - this is just `artist`
- `ordering` - this is the `day` column
- `table` - this is called `raw`

then the query is:

```sql
select
  sum(streams) over (partition by artist order by day asc) running_total,
  artist,
  day
from raw
```