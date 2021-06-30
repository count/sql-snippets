# Array to Column
Explore this snippet with some demo data [here](https://count.co/n/pQ914qtFlKo).

# Description
Taking a BigQuery ARRAY and creating a column for each value is commonly used when working with 'wide' data. The following snippet will let you quickly take an array column and create a row for each value.

```sql
SELECT 
   select <columns to keep>, array_value
FROM
   `project.schema.table`
CROSS JOIN UNNEST(<array_column>) as array_value
```
where:
- `<columns to keep>` is a list of the columns from the original table you want to keep in your query results
- `<array_column>` is the name of the ARRAY column


# Usage
The following example shows how to 'explode' 2 rows with 3 array elements each into a separate row for each array entry: 

```sql
WITH demo_data as (
  select 1 row_index,[1,3,4] array_col union all 
  select 2 row_index, [3,6,9] array_col 
)
select demo_data.row_index, array_value
  FROM demo_data
  cross join UNNEST(array_col) as array_value
```
| row_index | array_col |
| --- | ----------- |
| 1 | 1 |
| 1 | 3 |
| 1 | 4 |
| 2 | 3 |
| 2 | 6 |
| 2 | 9 |

# References & Helpful Links: 
- [UNNEST in BigQuery](https://count.co/sql-resources/bigquery-standard-sql/unnest)
- [Arrays in Standard SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays)
