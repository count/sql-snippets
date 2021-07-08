# Generate a uniform distribution

Explore this snippet [here](https://count.co/n/s7XKhGzLxX6?vm=e)

# Description

Generating a uniform distribution of (pseudo-) random numbers is often useful when performing statistical tests on data.
This snippet demonstrates a method for generating a deterministic sample from a uniform distribution:
1. Create a column of row indices - here the [`GENERATE_ARRAY`](https://cloud.google.com/bigquery/docs/reference/standard-sql/array_functions#generate_array) function is used, though the [`ROW_NUMBER`](https://cloud.google.com/bigquery/docs/reference/standard-sql/numbering_functions#row_number) window function may also be useful.
2. Convert the row indices into pseudo-random numbers using the [`FARM_FINGERPRINT`](https://cloud.google.com/bigquery/docs/reference/standard-sql/hash_functions#farm_fingerprint) hashing function.
3. Map the hashes into the correct range

```sql
with params as (
  select
    10000 as num_samples, -- How many samples to generate
    1000000 as precision, -- How much precision to maintain (e.g. 6 decimal places)
    2 as min,             -- Lower limit of distribution
    5 as max,             -- Upper limit of distribution
),
samples as (
  select
    -- Create hashes (between 0 and precision)
    -- Map [0, precision] -> [min, max]
    mod(abs(farm_fingerprint(cast(indices as string))),  precision) / precision * (max - min) + min as samples
  from
    params,
    unnest(generate_array(1, num_samples)) as indices
)

select * from samples
```

| samples |
| ------- |
| 3.55    |
| 3.316   |
| 2.146   |
| ...     |