# Heat maps

Explore this snippet with some demo data [here](https://count.co/n/ee5fRPdUrIQ?vm=e).

# Description

Heatmaps are a great way to visualize the relationship between two variables enumerated across all possible combinations of each variable. To create one, your data must contain
- 2 categorical columns (e.g. not numeric columns)
- 1 numerical column

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <CAT_COLUMN1> as cat1,
   <CAT_COLUMN2> as cag2,
FROM 
   <PROJECT.SCHEMA.TABLE>
GROUP BY
   cat1, cat2
```
where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column. Must add up to 1 for each bar.
- `CAT_COLUMN1` and `CAT_COLUMN2` are the groups you want to compare. One will go on the x axis and the other on the y-axis. Make sure these are categorical or temporal columns and not numerical fields. 

# Usage

In this example with some Spotify data, we'll see the average daily streams across different days of the week and months of the year. 

```sql
-- a
select sum(streams) daily_streams, day from spotify.spotify_daily_tracks group by day
```
| daily_streams | day        |
| ------------- | ---------- |
| 234109876     | 2020-10-25 |
| 243004361     | 2020-10-26 |
| 248233045     | 2020-10-27 |

```sql
select
  avg(a.daily_streams) avg_daily_streams,
  format_date('%m - %b',day) month,
  format_date('%w %A',day) day_of_week
from a
group by day_of_week, month
```
<img width="274" alt="heatmap" src="https://user-images.githubusercontent.com/42146708/124849663-0a6b8e80-df54-11eb-940f-eb1e7daa0663.png">
