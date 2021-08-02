#  UDF: Sort Array

## Description

Javascript UDF to sort an array in ascending order.

```sql
-- Create the UDF.
CREATE OR REPLACE FUNCTION array_sort(a array)
  RETURNS array
  LANGUAGE JAVASCRIPT
AS
$$
  return A.sort();
$$
;

-- Call the UDF with a small array.
SELECT ARRAY_SORT(PARSE_JSON('[2,4,5,3,1]'));
```

## Example 


```sql
SELECT ARRAY_SORT(PARSE_JSON('[2,4,5,3,1]')) as arr_sorted;
```

| ARR_SORTED |
| ------------- |
| [1,2,3,4,5]   |

# References: 
- https://docs.snowflake.com/en/sql-reference/user-defined-functions.html
- https://docs.snowflake.com/en/developer-guide/udf/javascript/udf-javascript-introduction.html#introductory-example
