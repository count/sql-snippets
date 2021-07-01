# Filtering arrays

Explore this snippet [here](https://count.co/n/YvwEswZfHtP?vm=e).

# Description
The ARRAY function in BigQuery will accept an entire subquery, which can be used to filter an array:

```sql
select array(
  select x
  from unnest((select [1, 2, 3, 4])) as x
  where x <= 3 -- Filter predicate
) as filtered
```