# Get the last element of an array


# Description
You can use [ARRAY_SLICE](https://docs.snowflake.com/en/sql-reference/functions/array_slice.html) and [ARRAY_SIZE](https://docs.snowflake.com/en/sql-reference/functions/array_size.html) to find the last element in an Array in Snowflake: 

```sql
select 
  array_slice(nums,-1,array_size(nums)) last_element
from 
  (
    select array_construct(1, 2, 3, 4) as nums
  )
```
| LAST_ELEMENT |
| --- |
| [4] |
