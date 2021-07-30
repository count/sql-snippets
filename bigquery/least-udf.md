# UDF to find the least non-null value in an array
View interactive version [here](https://count.co/n/5KMhS4l4xgR?vm=e).

# Description
LEAST and GREATEST will include NULLS in BigQuery, so to find the LEAST excluding nulls across a row of values can be done using this UDF: 

```sql
CREATE or replace FUNCTION <dataset>.myLeast(x ARRAY<INT64>) AS
((SELECT MIN(y) FROM UNNEST(x) AS y));
```
Credit to this [Stackoverflow post](https://stackoverflow.com/questions/43796213/correctly-migrate-postgres-least-behavior-to-bigquery). 

# Example: 
```sql
with demo_data as (
  select 1 rownumber, 0 a, null b, -1 c, 1 d union all 
  select 2 rownumber, 5 a, 0 b, null c, 8 d
)
select rownumber, spotify.myLeast([a,b,c,d]) least_non_null, least(a,b,c,d) least_old_way
from demo_data
```
|rownumber|least_not_null|least_old_way|
|--- | ---- | ----- |
| 1 | -1 | NULL |
|2 | 0 | NULL |
