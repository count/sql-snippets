# Transforming arrays

Explore this snippet with some demo data [here](https://count.co/n/UPNSCO974GQ?vm=e).

# Description

To transform elements of an array (often known as mapping over an array), it is possible to gain access to array elements using the `FLATTEN` function

```sql
with data as (select [1,2,3] nums, ['a', 'b', 'c'] strs)

select
   array_agg(x.value * 2 + 1) nums_transformed, -- Transformation function
   array_agg(y.value ||'_2') strs_transformed -- Another transformation function
from DATA
cross join table(flatten(input => nums)) as x
left join table(flatten(input => strs)) as y on x.index = y.index
```

| NUMS_TRANSFORMED | STRS_TRANSFORMED     |
|------------------|----------------------|
| [3,5,7]          | ["a_2","b_2","c_2" ] |
