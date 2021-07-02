# Get the last element of an array

Explore this snippet [here](https://count.co/n/0loHJW60YO8?vm=e).

# Description
Use the `array_length` function to access the final element of an array. This function takes two arguments, an array and a dimension index. For common one-dimensional arrays, the dimension index is always  1.

```sql
with data as (select array[1,2,3,4] as nums)

select nums[array_length(nums, 1)] from data
```