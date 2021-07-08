# Time series line chart

Explore this snippet with some demo data [here](https://count.co/n/B3XjM5dxt4f?vm=e).

# Description

Timeseries charts show how individual metric(s) change over time. They have lines along the y-axis and dates along the x-axis. 
You just need a simple GROUP BY to get all the elements you need for a timeseries chart: 

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <DATETIME_COLUMN> as datetime
FROM 
   <PROJECT.SCHEMA.TABLE>
GROUP BY
   datetime
```
where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column.
- `DATETIME_COLUMN` is the group you want to show on the x-axis. Make sure this is a date, datetime, timestamp, or time column (not a number)

# Usage

In this example with some Spotify data, we'll look at the total daily streams over time:

```sql
select
  sum(streams) streams, 
  day 
from spotify.spotify_daily_tracks
group by day
```
<img width="832" alt="timeseries" src="https://user-images.githubusercontent.com/42146708/124672892-b2a62800-de6c-11eb-8240-97e4fa848337.png">
