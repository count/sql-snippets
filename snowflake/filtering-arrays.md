# Filtering arrays


# Description
To filter arrays in Snowflake you can use a combination of ARRAY_AGG and FLATTEN: 

```sql
-- A
select array_agg(value) as filtered from 
(select 
array_construct(1,2,3,4) as x),
lateral flatten(input=>x)
where value <=3
```
| FILTERED |
| -------- |
| [1,2,3] |