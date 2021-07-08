# 100% bar chart

Explore this snippet with some demo data [here](https://count.co/n/qMn2uMsl0rz?vm=e).

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
   <PROJECT.SCHEMA.TABLE>
GROUP BY
   x, color
```
where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your metric. Make sure this is a numeric column. Must add up to 1 for each bar.
- `XAXIS_COLUMN` is the group you want to show on the x-axis. Make sure this is a date, datetime, timestamp, or time column (not a number)
- `COLOR_COLUMN` is the group you want to show as different colors of your bars. 

# Usage

In this example with some tennis data we'll see what percentage of matches won for the world's top tennis players come on different surfaces. 

```sql
-- a
select
  count(matches_partitioned.match_num) matches_won,
  winner_name player, 
  surface 
from tennis.matches_partitioned
where winner_name in ('Rafael Nadal','Serena Williams', 'Roger Federer','Novak Djokovic')
group by player, surface
```

|matches_won| player| surface|
|-----------|-------|--------|
|95| Novak Djokovic | Grass|
|621| Novak Djokovic | Hard |
|230 | Novak Djokovic | Clay|
|9 | Novak Djokovic | Carpet|
| ... | ... | ... |


```sql
-- b
select
  count(matches_partitioned.match_num) matches_played,
  winner_name player
from tennis.matches_partitioned
where winner_name in ('Rafael Nadal','Serena Williams', 'Roger Federer','Novak Djokovic')
group by player
```
| matches_played | player          |
| -------------- | --------------- |
| 955            | Novak Djokovic  |
| 1250           | Roger Federer   |
| 1016           | Rafael Nadal    |
| 846            | Serena Williams | 

```sql
select
  a.player,
  a.surface,
  a.matches_won / b.matches_played per_matches_won
from
  a left join b on a.player = b.player
```
<img width="832" alt="100-stacked-bar" src="https://user-images.githubusercontent.com/42146708/124782888-74a31580-def9-11eb-900b-a6c36aa1664e.png">
