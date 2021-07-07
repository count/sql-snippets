# Running total

Explore this snippet with some demo data [here](https://count.co/n/9oKm47UpLz2?vm=e).

# Description

Running totals are relatively simple to calculate in Snowflake, using the `sum` window function.
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
-- RAW
select streams, day, artist from PUBLIC.SPOTIFY_DAILY_TRACKS
```
| STREAMS | DAY        | ARTIST |
|---------|------------|--------|
| 628338  | 2020-10-25 | CJ     |
| 720233  | 2020-10-26 | CJ     |
| 777880  | 2020-10-27 | CJ     |
| 811130  | 2020-10-28 | CJ     |

```sql
select
  sum(streams) over (partition by artist order by day asc) running_total,
  artist,
  day
from RAW
order by 2,3
```
| RUNNING_TOTAL | DAY        | ARTIST |
|---------------|------------|--------|
| 628338        | 2020-10-25 | CJ     |
| 1348571       | 2020-10-26 | CJ     |
| 2126451       | 2020-10-27 | CJ     |
| 2937581       | 2020-10-28 | CJ     |
