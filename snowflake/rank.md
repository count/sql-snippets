# Rank
Explore this snippet with some demo data [here](https://count.co/n/9MW21RBKUus).



# Description
A common requirement is the need to rank a particular row in a group by some quantity - for example, 'rank stores by total sales in each region'. Fortunately BigQuery supports the `rank` window function to help with this.
The query template is

```sql
select
  rank() over (partition by <fields> order by <rank_by> desc) rank
from <table>
```
where
- `fields` - zero or more columns to split the results by - a rank will be calculated for each unique combination of values in these columns
- `rank_by` - a column to decide the ranking
- `table` - where to pull these columns from
# Example
Using total Spotify streams by day and artist as an example data source, let's calculate, by artist, the rank for total streams by day (e.g. rank = 1 indicates that day was their most-streamed day).
- `fields` - this is just `artist`
- `rank_by` - this is `streams`
- `table` - this is called `raw`
then the query is:

```sql
-- RAW
select streams, artist, day from PUBLIC.SPOTIFY_DAILY_TRACKS
```
|DAY | STREAMS | ARTIST|
|---|---|----|
|2017-06-01| 509093 | 2 Chainz|
|2017-06-03| 562412 | 2 Chainz|
|2017-06-04| 480350 | 2 Chainz|
```sql
-- WITH_RANK
select
  *,
  rank() over (partition by artist order by streams desc) rank
from RAW
```
|RANK|DAY | STREAMS | ARTIST|
|---|---|---|----|
|247|2017-06-01| 509093 | 2 Chainz|
|205|2017-06-03| 562412 | 2 Chainz|
|262|2017-06-04| 480350 | 2 Chainz|
