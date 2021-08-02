# Cumulative distribution functions

Explore this snippet with some demo data [here](https://count.co/n/sL7bEFcg11Z?vm=e).

# Description
Cumulative distribution functions (CDF) are a method for analysing the distribution of a quantity, similar to histograms. They show, for each value of a quantity, what fraction of rows are smaller or greater.
One method for calculating a CDF is as follows:

```sql
select
  -- Use a row_number window function to get the position of this row
  (row_number() over (order by <quantity> asc)) / (select count(*) from <table>) cdf,
  <quantity>,
from <table>
```
where
- `quantity` - the column containing the metric of interest
- `table` - the table name
# Example


```sql
with data as (
  select  1 student_id,  97 score union all 
  select 2 student_id, 93 score union all 
  select 3 student_id, 76 score union all 
  select 4 student_id, 82 score union all 
  select 5 student_id, 77 score
)
select student_id, score, (row_number() over (order by score asc)) / (select count(*) from data) frac
from data
```
| student_id      | score | frac |
| ----------- | ----------- | ---- |
|3 | 76 | 0.2|
| 5| 77 | 0.4 |
|4 | 82 | 0.6|
|2 | 93 | 0.8|
|1|97 | 1|
