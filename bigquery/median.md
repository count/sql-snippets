# MEDIAN


# Description
BigQuery does not have an explicit `MEDIAN` function. The following snippet will show you how to calculate the median of any column. 


# Snippet ✂️
Use the following snippet to create your median function in the BigQuery console: 

```sql
PERCENTILE_CONT(<COLUMN>, 0.5) OVER() as median
```


# Usage
Use the `MEDIAN` syntax in a query like:

```sql
SELECT
  PERCENTILE_CONT(<COLUMN>, 0.5) OVER() as median
FROM
  <PROJECT.SCHEMA.TABLE>
```