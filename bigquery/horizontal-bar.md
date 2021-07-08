# Horizontal Bar Chart

Explore this snippet with some demo data [here](https://count.co/n/w4I0DgqsPT2?vm=e).

# Description

Horizontal Bar charts are ideal for comparing a particular metric across a group of values. They have bars along the x-axis and group names along the y-axis. 
You just need a simple GROUP BY to get all the elements you need for a horizontal bar chart: 

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <GROUP_COLUMN> as group
FROM 
   <PROJECT.SCHEMA.TABLE>
GROUP BY
   group
```

where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column.
- `GROUP_COLUMN` is the group you want to show on the y-axis. Make sure this is a categorical column (not a number)

# Usage

In this example with some Spotify data, we'll compare the total streams by artist:

```sql
select
  sum(streams) streams, 
  artist 
from spotify.spotify_daily_tracks
group by artist
```
<img width="832" alt="horizontal-bar" src="https://user-images.githubusercontent.com/42146708/124672685-4fb49100-de6c-11eb-9bc6-fc6ce5f340e2.png">
