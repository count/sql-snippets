# Cumulative distribution functions

Explore this snippet with some demo data [here](https://count.co/n/sL7bEFcg11Z?vm=e).

# Description
Cumulative distribution functions (CDF) are a method for analysing the distribution of a quantity, similar to histograms. They show, for each value of a quantity, what fraction of rows are smaller or greater.
One method for calculating a CDF is as follows:

```sql
select
  -- Use a row_number window function to get the position of this row
  (row_number() over (order by <quantity> asc)) / (select count(*) from <table>) cdf,
  <quantity>,
from <table>
```
where
- `quantity` - the column containing the metric of interest
- `table` - the table name
# Example

Using total Spotify streams as an example data source, let's identify:
- `table` - this is called `raw`
- `quantity` - this is the column `streams`

then the query becomes:

```sql
select
  (row_number() over (order by streams asc)) / (select count(*) from raw) frac,
  streams,
from raw
```
| frac      | streams |
| ----------- | ----------- |
| 0      | 374359       |
| 0   | 375308        |
| ...   | ...        |
| 1   | 7778950        |
