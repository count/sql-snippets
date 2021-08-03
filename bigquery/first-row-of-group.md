# Find the first row of each group

Explore this snippet [here](https://count.co/n/1KZI8Y4aAX5?vm=e).

# Description

Finding the first value in a column partitioned by some other column is simple in SQL, using the GROUP BY clause. Returning the rest of the columns corresponding to that value is a little trickier. Here's one method, which relies on the RANK window function:

```sql
with and_ranking as (
  select
    *,
    rank() over (partition by <partition> order by <ordering>) ranking
  from <table>
)
select * from and_ranking where ranking = 1
```
where
- `partition` - the column(s) to partition by
- `ordering` - the column(s) which determine the ordering defining 'first'
- `table` - the source table

The CTE `and_ranking` adds a column to the original table called `ranking`, which is the order of that row within the groups defined by `partition`. Filtering for `ranking = 1` picks out those 'first' rows.

# Example

Using total Spotify streams as an example data source, we can pick the rows of the table corresponding to the most-streamed song per day. Filling in the template above:
- `partition` - this is just `day`
- `ordering` - this is `streams desc`
- `table` - this is `spotify.spotify_daily_tracks`

```sql
with and_ranking as (
  select
    *,
    rank() over (partition by day order by streams desc) ranking
  from spotify.spotify_daily_tracks
)
select day, title, artist, streams from and_ranking where ranking = 1
```