# Rand Between in BigQuery
Interactive version [here](https://count.co/n/lUHSRbAZqLK?vm=e).


# Description
Generating random numbers is a good way to test out our code or to randomize results. 
In BigQuery there is a [`RAND`](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#rand) function which will generate a number between 0 and 1. 
But sometimes it's helpful to get a random integer within a specified range. This snippet will do that: 

```sql
SELECT 
   ROUND(<min> + RAND() * (<max> - <min>)) rand_between
FROM 
   <TABLE>
```
where: 
- `<min>`, `<max>` is the range between which you want to generate a random number
# Example: 

```sql
with random_number as (select rand() as random)
select 
  random, -- random number between 0 and 1
  round(random*100) rand_1_100, --random integer between 1 and 100
  round(1 + random * (10 - 1)) rand_btw_1_10 -- random integer between 1 and 10
from random_number
```
|random | rand_1_100 | rand_btw_1_10 |
| ----- | ---------- | ------------- |
|0.597 | 60 | 6 |
