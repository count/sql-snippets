# Extract Repeating Words from String


# Description
If I wanted to extract all Workorder numbers from the string: 

```sql
F1 - wo-12, some other text WOM-1235674,WOM-2345,WOM-3456
```
Where the workorder numbers always follow `WOM-`,
and I wanted to make sure each workorder number got it's own row so I could join to another table, I could do: 

```sql
SELECT 
   product_id, 
   wo_number
FROM 
   <TABLE>
CROSS JOIN 
   UNNEST(regexp_extract_all(wo_text,r'WOM-([1-9]+)')) as wo_number
```
where `wo_text` is the column of text that contains the workorder info. 
> Inspired by: https://www.reddit.com/r/bigquery/comments/oizgm6/extracting_word_from_string/
# Example: 
```sql
with data as (
  select 123 product_id, 'F1 - wo-12, some other text WOM-1235674,WOM-2345,WOM-3456' as wo_text
)
select product_id, wo_number
from data
cross join unnest(regexp_extract_all(wo_text,r'WOM-([1-9]+)')) as wo_number
--This will intentionally not extract 'wo-12' as it doesn't match WOM-[numbers] pattern
```
|product_id | wo_number|
|---- | ----- |
| 123 | 1235674 |
|123 | 2345 |
|123 | 3456 |
