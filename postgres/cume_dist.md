# Cumulative Distribution
View an interactive snippet [here](https://count.co/n/1fymVJoCPVM?vm=e).

# Description
Cumulative Distribution is a method for analysing the distribution of a quantity, similar to histograms. They show, for each value of a quantity, what fraction of rows are smaller or greater.
One method for calculating it is as follows:

```sql
SELECT
  -- If necessary, use a row_number window function to get the position of this row in the dataset
  (ROW_NUMBER() OVER (ORDER BY <quantity>)) / (SELECT COUNT(*) FROM <schema.table>) AS cume_dist,
  <quantity>,
FROM <schema.table>
```
where
- `quantity` - the column containing the metric of interest
- `schema.table` - the table with your data

# Example

```sql
WITH data AS (
    SELECT md5(RANDOM()::TEXT) AS username_hash,
           RANDOM() AS random_number,
           row_number
    FROM GENERATE_SERIES(1, 10) AS row_number
)

SELECT
  --we do not need to use a row_number window function as we have a generate_series in our test data set
   username_hash,
  (row_number::FLOAT / (SELECT COUNT(*) FROM data)) AS frac,
  random_number
FROM data;
```
| username_hash | frac | random_number |
| ----- | ----- | ----- |
| f9689609b388f96ac0846c69d4b916dc | 0.1 | 0.48311887726926983 |
| 68551a63b1eb24c750408dc20c6e4510 | 0.2 | 0.7580949364508456 |
| 86b915387b7842c33c52e71ada5488d2 | 0.3 | 0.9461365697398278 |
| 2f9b8d13471f5f8896c073edc4d712a8 | 0.4 | 0.2723711331889973 |
| 27cd443183f886105001f337d987bdaa | 0.5 | 0.2698584751712403 |
| 370ce4710f9900fa38f99494d50b9275 | 0.6 | 0.011248462980887552 |
| 98d2b23c493e44a395a330ca2d349a47 | 0.7 | 0.7387236527452572 |
| 328ac83d33498f81a6b1e70fb8e0dcbb | 0.8 | 0.5815637247802528 |
| 9426739e865d970e7c932c37714b32f0 | 0.9 | 0.5957734211030683 |
| 452a9d8f21c9773fbd5d832138d0d4d1 | 1 | 0.9090960753850368 |
