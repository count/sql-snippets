# Bar Chart
Explore this snippet with some demo data [here](https://count.co/n/KCUlCzlJDHK?vm=e).


# Description
Bar charts are perhaps the most common charts used in data analysis. They have one categorical or temporal axis, and one numerical axis. 
You just need a simple GROUP BY to get all the elements you need for a bar chart: 

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <CATEGORICAL_COLUMN> as cat
FROM 
   `PROJECT.SCHEMA.TABLE>
GROUP BY
   cat
```
where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column.
- `CATEGORICAL_COLUMN` is the group you want to show on the x-axis. Make sure this is a date, datetime, timestamp, or time column (not a number)
# Usage
In this example with some spotify data, we'll look at the average daily streams by month of year:

```sql

select avg(streams) avg_daily_streams, format_date('%m - %b',day) month from (
select sum(streams) streams, 
day 
from spotify.spotify_daily_tracks
group by day)
group by month
```
<img width="832" alt="bar" src="https://user-images.githubusercontent.com/42146708/124673190-424bd680-de6d-11eb-91c3-3675a848804c.png">
