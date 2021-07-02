# Reverse an array

Explore this snippet [here](https://count.co/n/aDl6lXQKQdx?vm=e).

# Description
There is no built-in function for reversing an array, but it is possible to construct one using the `generate_subscripts` function:

```sql
with data as (select array[1, 2, 3, 4] as nums)

select
  array(
    select nums[i]
    from generate_subscripts(nums, 1) as indices(i)
    order by i desc
  ) as reversed
from data
```


## References
PostgreSQL wiki [https://wiki.postgresql.org/wiki/Array_reverse](https://wiki.postgresql.org/wiki/Array_reverse)