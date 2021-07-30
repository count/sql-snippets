# Greatest/Least value in an array
Interactive version [here](https://count.co/n/tgXkwcSxl2W?b=L2LtcTSMIQS).

# Description
GREATEST and LEAST are helpful way to find the greatest value in a row but to do that for an array is trickier. 
This snippet lets you return the max/min value from an array while maintaining your data shape. 
**NOTE: This will exclude NULLS.**

```sql
SELECT 
   <COLUMN1>...<COLUMNN>,
   MIN(value)
FROM
   <TABLE>
CROSS JOIN UNNEST(<ARRAY_COLUMN>) as value,
group by <COLUMN1>...<COLUMNN>
```
where:
- `<COLUMN1>...<COLUMNN>` is a list of all the other rows you wish to select,

- `<ARRAY_COLUMN>` is the column that contains the array from which you want to select the greatest/least value

# Example: 
```sql
with demo_data as (
  select 1 rownumber, [0,1,NULL,3] arr
  union all select 2 rownumber, [1,2,3,4] arr
)
select 
  rownumber,
  min(value) least,
  max(value) greatest
from demo_data 
cross join unnest(arr) as value
group by rownumber
```
|rownumber|least | greatest |
|---- |---- |----|
|1 | 0 |3|
|2 | 1 |4|
