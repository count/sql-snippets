# Transforming arrays

Explore this snippet [here](https://count.co/n/CLAQ94HaRvO?vm=e).

# Description
To transform elements of an array (often known as mapping over an array), it is possible to gain access to array elements using the `UNNEST` function:

```sql
with data as (select [1,2,3] nums, ['a', 'b', 'c'] strs)

select

  array(
    select
      x * 2 + 1 -- Transformation function
    from unnest(nums) as x
  ) as nums_transformed,

  array(
    select
      x || '_2' -- Transformation function
    from unnest(strs) as x
  ) as strs_transformed
  
from data
```