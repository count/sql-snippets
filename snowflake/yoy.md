# Year-on-year percentage difference

To explore this example with some demo data, head [here](https://count.co/n/WDGC9JhS6dE?vm=e).

# Description

Calculating the year-on-year change of a quantity typically requires a few steps:
1. Aggregate the quantity by year (and by any other fields)
2. Add a column for last years quantity (this will be null for the first year in the dataset)
3. Calculate the change from last year to this year
A generic query template may then look like

```sql
select
  *,
  -- Calculate percentage change
  (<year_total> - last_year_total) / last_year_total * 100 as perc_diff
from (
  select
    *,
    -- Use a window function to get last year's total
    lag(<year_total>) over (partition by <fields> order by <year_column> asc) last_year_total
  from <table>
  order by <fields>, <year_column> desc
)
```
where
- `table` - your source data table
- `fields` - one or more columns to split the year-on-year differences by
- `year_column` - the column containing the year of the aggregation
- `year_total` - the column containing the aggregated data

# Example

Using total Spotify streams as an example data source, let's identify:
- `table` - this is called `RAW`
- `fields` - this is just the single column `ARTIST`
- `year_column` - this is called `YEAR`
- `year_total` - this is `SUM_STREAMS`

Then the query looks like:

```sql
-- RAW
select
  date_trunc(year,day) year,
  sum(streams) sum_streams,
  artist
from PUBLIC.SPOTIFY_DAILY_TRACKS
group by 1,3
```
| year       | sum_streams | artist |
| ---------- | ----------- | ------ |
| 2020-01-01 | 115372043   | CJ     |
| 2021-01-01 | 179284925   | CJ     |
| ...        | ...         | ...    |

```sql
select
  *,
  (sum_streams - last_year_total) / last_year_total * 100 as perc_diff
from (
  select
    *,
    lag(sum_streams) over (partition by artist order by year asc) last_year_total
  from RAW

  order by artist, year desc
)
```
| year       | sum_streams | artist | last_year_total | perc_diff | 
| ---------- | ----------- | ------ | --------------- | --------- |
| 2020-01-01 | 115372043   | CJ     | NULL            | NULL      |
| 2021-01-01 | 179284925   | CJ     | 115372043       | 55.397200 | 
| ...        | ...         | ...    | ...             | ...       |
