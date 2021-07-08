# Stacked Bar & Line Chart

Explore this snippet with some demo data [here](https://count.co/n/ze9Y5H2r4mu?vm=e).

# Description

Bar and line charts can communicate a lot of info on a single chart. They require all of the elements of a bar chart: 
- 2 categorical or temporal columns - 1 for the x-axis and 1 for stacked bars
- 2 numeric columns - 1 for the bar and 1 for the line

```sql
SELECT 
   AGG_FN(<COLUMN>) OVER (PARTITION BY <CAT_COLUMN1>, <CAT_COLUMN2>) as bar_metric,
   <CAT_COLUMN1> as x_axis,
   <CAT_COLUMN2> as cat,
   AGG_FN(<COLUMN2>) OVER (PARTITION BY <CAT_COLUMN1>) as line
FROM 
   <TABLE>
```

where: 
- `AGG_FN` is an aggregation function like `SUM`, `AVG`, `COUNT`, `MAX`, etc.
- `COLUMN` is the column you want to aggregate to get your bar chart metric. Make sure this is a numeric column. 
- `COLUMN2` is the column you want to aggregate to get your line chart metric. Make sure this is a numeric column. 
- `CAT_COLUMN1` and `CAT_COLUMN2` are the groups you want to compare. One will go on the x axis and the other on the colors of the bar chart. Make sure these are categorical or temporal columns and not numerical fields. 

# Usage

In this example with some example data we'll look at some sample data of users that were successful and unsuccessful at a certain task over a few days. We'll show the count of users by their outcome, then show the overall success rate for each day. 

```sql
with sample_data as (
   select date('2021-01-01') registered_at, 'successful' category, abs(random()/1000000) user_count union all 
   select date('2021-01-01') registered_at, 'unsuccessful' category, abs(random()/1000000) user_count union all 
   select date('2021-01-02') registered_at, 'successful' category, abs(random()/1000000) user_count union all 
   select date('2021-01-02') registered_at, 'unsuccessful' category, abs(random()/1000000) user_count union all 
   select date('2021-01-03') registered_at, 'successful' category, abs(random()/1000000) user_count union all 
   select date('2021-01-03') registered_at, 'unsuccessful' category, abs(random()/1000000) user_count
)


select 
  registered_at, 
  category, 
  user_count, 
  sum(case when category = 'successful' then user_count end) over (partition by registered_at) / sum(user_count) over (partition by registered_at) success_rate
from sample_data
```
<img width="941" alt="stackedbar-line" src="https://user-images.githubusercontent.com/42146708/124850076-dcd31500-df54-11eb-942f-f2454149d7f7.png">
