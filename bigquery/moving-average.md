# Moving average

Explore this snippet with some demo data [here](https://count.co/n/X2EmYjUGwwO?vm=e).

# Description
Moving averages are relatively simple to calculate in BigQuery, using the `avg` window function. The template for the query is

```sql
select
  avg(<value>) over (partition by <fields> order by <ordering> asc rows between <x> preceding and current row) mov_av,
  <fields>,
  <ordering>
from <table>
```
where
- `value` - this is the numeric quantity to calculate a moving average for
- `fields` - these are zero or more columns to split the moving averages by
- `ordering` - this is the column which determines the order of the moving average, most commonly temporal
- `x` - how many rows to include in the moving average
- `table` - where to pull these columns from
# Example
Using total Spotify streams as an example data source, let's identify:
- `value` - this is `streams`
- `fields` - this is just `artist`
- `ordering` - this is the `day` column
- `x` - choose 7 for a weekly average
- `table` - this is called `raw` 

then the query is:

```sql
select
  avg(streams) over (partition by artist order by day asc rows between 7 preceding and current row) mov_av,
  artist,
  day, streams
from raw
```