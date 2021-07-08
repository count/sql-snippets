# 100% Bar Chart

Explore this snippet with some demo data [here](https://count.co/n/C6BCLP94YLs?vm=e).

# Description

100% bar charts compare the proportions of a metric across different groups.
To build one you'll need: 
- A numerical column as the metric you want to compare. This should be represented as a percentage. 
- 2 categorical columns. One for the x-axis and one for the coloring of the bars. 

```sql
SELECT 
   AGG_FN(<COLUMN>) as metric,
   <XAXIS_COLUMN> as x,
   <COLOR_COLUMN> as color,
FROM 
   <TABLE>
GROUP BY
   x, color
```

where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column. Must add up to 1 for each bar.
- `XAXIS_COLUMN` is the group you want to show on the x-axis. Make sure this is a date, datetime, timestamp, or time column (not a number)
- `COLOR_COLUMN` is the group you want to show as different colors of your bars. 

# Usage

In this example with some London weather data, we calculate the percentage of days in each month that are of each weather type (e.g. rain, sun).

```sql
-- a
select
  count(DATE) days,
  month(date) month, 
  WEATHER 
from PUBLIC.LONDON_WEATHER
group by month, WEATHER
order by month
```

| DAYS | MONTH | WEATHER |
| ---- | ----- | ------- |
| 33   | 1     | sunny   |
| 16   | 1     | fog     |
| 74   | 1     | rain    |
| ...  | ...   | ...     |

```sql
select
  count(DATE) days,
  month(date) month
from PUBLIC.LONDON_WEATHER
group by month
order by month
```
| DAYS | MONTH |
| ---- | ----- |
| 123  | 1     |
| 113  | 2     |
| 123  | 3     |
| 120  | 4     | 
| ...  | ...   |

```sql
-- c
select
  "a".WEATHER,
  "a".MONTH,
  "a".DAYS / "b".DAYS per_days
from
  "a" left join "b" on "a".MONTH = "b".MONTH
```

<img width="280" alt="stacked-bar-2" src="https://user-images.githubusercontent.com/42146708/124853380-a26c7680-df5a-11eb-9a32-f9276b8e7368.png">
