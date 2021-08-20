# Cumulative distribution functions

Explore this snippet with some demo data [here](https://count.co/n/ANpwFTvhnWT?vm=e).

# Description

Cumulative distribution functions (CDF) are a method for analysing the distribution of a quantity, similar to histograms. They show, for each value of a quantity, what fraction of rows are smaller or greater.
One method for calculating a CDF is as follows:

```
select 
--  Use a row_number window function to get the position of this row and CAST to convert row_number and count from integers to decimal
CAST(row_number() over (order by <quantity> asc) AS DECIMAL)/CAST((select count(*) from <table>) AS DECIMAL) AS cdf
from <table>
```

where

* `quantity` - the column containing the metric of interest
* `table` - the table name

**Note:** CAST is used to convert the numerator and denominator of the fraction into decimals before they are divided. 

# Example

Using some student test scores as an example data source, let’s identify:

* `table` - this is called `Example_data`
* `quantity` - this is the `column score`

then the query becomes:

```
select 
student_id, 
score, 
CAST(row_number() over (order by score asc) AS DECIMAL)/CAST((select count(*) from Example_data) AS DECIMAL) AS frac
from Example_data
```

|student_id|score|frac|
|---|---|---|
|3|76|0.2|
|5|77|0.2|
|4|82|0.6|
|...|...|...|
|1|97|1|
