# Generating arrays

Explore this snippet [here](https://count.co/n/2avxdwBFCGz?vm=e).

# Description
BigQuery supports the ARRAY column type - here are a few methods for generating arrays.
## Literals
Arrays can contain most types, though all elements must be derived from the same supertype.

```sql
select
  [1, 2, 3] as num_array,
  ['a', 'b', 'c'] as str_array,
  [current_timestamp(), current_timestamp()] as timestamp_array,
```


## GENERATE_* functions
Analogous to `numpy.linspace()`.

```sql
select
  generate_array(0, 10, 2) AS even_numbers,
  generate_date_array(CAST('2020-01-01' AS DATE), CAST('2020-03-01' AS DATE), INTERVAL 1 MONTH) AS month_starts
```


## ARRAY_AGG
Aggregate a column into a single row element.

```sql
SELECT ARRAY_AGG(num)
FROM (SELECT 1 num UNION ALL SELECT 2 num UNION ALL SELECT 3 num)
```