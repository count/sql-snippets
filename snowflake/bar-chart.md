# Bar Chart

Explore this snippet with some demo data [here](https://count.co/n/CmSsFFvSJfz?vm=e).

# Description

Bar charts are perhaps the most common charts used in data analysis. They have one categorical or temporal axis, and one numerical axis. 
You just need a simple GROUP BY to get all the elements you need for a bar chart: 

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <CATEGORICAL_COLUMN> as cat
FROM 
   <TABLE>
GROUP BY
   cat
```

where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column.
- `CATEGORICAL_COLUMN` is the group you want to show on the x-axis. Make sure this is a date, datetime, timestamp, or time column (not a number)

# Usage

In this example with some Spotify data, we'll look at the average daily streams by month of year:

```sql
select
  avg(streams) avg_daily_streams,
  MONTH(day) month
from (
  select sum(streams) streams, 
  day 
  from PUBLIC.SPOTIFY_DAILY_TRACKS
  group by day
)
group by month
```
<img width="832" alt="bar" src="https://user-images.githubusercontent.com/42146708/124851863-31c45a80-df58-11eb-91ae-3f5961f844f8.png">
