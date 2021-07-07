# Moving average

Explore this snippet with some demo data [here](https://count.co/n/vwTzk68QCZ7?vm=e).

# Description
Moving averages are relatively simple to calculate in Snowflake, using the `avg` window function. The template for the query is

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
-- RAW
select day, sum(streams) streams, artist from PUBLIC.SPOTIFY_DAILY_TRACKS group by 1, 3
```
| DAY      | STREAMS | ARTIST  |
|----------|---------|---------|
|2017-06-01| 509093  | 2 Chainz|
|2017-06-03| 562412  | 2 Chainz|
|2017-06-04| 480350  | 2 Chainz|

```sql
select
  avg(streams) over (partition by artist order by day asc rows between 7 preceding and current row) mov_av,
  artist,
  day,
  streams
from RAW
```
| MOV_AVG  | DAY        | STREAMS | ARTIST   |
|----------|------------|---------|----------|
| 509093   | 2017-06-01 | 509093  | 2 Chainz |
| 535752.5 | 2017-06-03 | 562412  | 2 Chainz |
| 517285   | 2017-06-04 | 480350  | 2 Chainz |
