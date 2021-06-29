# Compound growth rates


# Description
When a quantity is increasing in time by the same fraction every period, it is said to exhibit **compound growth** - every time period it increases by a larger amount.
It is possible to show that the behaviour of the quantity over time follows the law

```sql
x(t) = x(0) * exp(r * t)
```
where
- `x(t)` is the value of the quantity now
- `x(0)` is the value of the quantity at time t = 0
- `t` is time in units of [time_period]
- `r` is the growth rate in units of 1 / [time_period]


# Calculating growth rates
From the equation above, to calculate a growth rate you need to know:
- The value at the start of a period
- The value at the end of a period
- The duration of the period
Then, following some algebra, the growth rate is given by

```sql
r = ln(x(t) / x(0)) / t
```

For a concrete example, assume a table with columns:
- `num_this_month` - this is `x(t)`
- `num_last_month` - this is `x(0)`
- `this_month` & `last_month` - `t` (in years) = `datetime_diff(this_month, last_month, month) / 12`


#### Inferred growth rates

In the following then, `growth_rate` is the equivalent yearly growth rate for that month:

```sql
-- with_growth_rate
select
  *,
  ln(num_this_month / num_last_month) * 12 / datetime_diff(this_month, last_month, month) growth_rate
from with_previous_month
```


# Using growth rates
To better depict the effects of a given growth rate, it can be converted to a year-on-year growth factor by inserting into the exponential formula above with t = 1 (one year duration).
Or mathematically,

```sql
yoy_growth = x(1) / x(0) = exp(r)
```
It is always sensible to spot-check what a growth rate actually represents to decide whether or not it is a useful metric.

#### Extrapolated year-on-year growth

```sql
-- with_yoy_growth
select
  *,
  exp(growth_rate) as yoy_growth
from with_growth_rate
```