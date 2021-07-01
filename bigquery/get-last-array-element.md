# Get the last element of an array

Explore this snippet [here](https://count.co/n/f4moFjZCYNV?vm=e).

# Description
There are two methods to retrieve the last element of an array - calculate the array length, or reverse the array and select the first element:

```sql
select 
  nums[ordinal(array_length(nums))] as method_1,
  array_reverse(nums)[ordinal(1)] as method_2,
from (select [1, 2, 3, 4] as nums)
```