# Converting dates/times to strings

Explore this snippet [here](https://count.co/n/p72zrP81Vye?vm=e).

# Description
It is possible to convert from a `DATE` / `DATETIME` / `TIMESTAMP` / `TIME` data type into a `STRING` using the `CAST` function, and BigQuery will choose a default string format.
For more control over the output format use one of the `FORMAT_*` functions:

```sql
with dt as (
  select
    current_date() as date,
    current_time() as time,
    current_datetime() as datetime,
    current_timestamp() as timestamp
)

select
  cast(timestamp as string) default_format,
  format_date('%Y/%d/%m', datetime) day_first,
  format_datetime('%A', date) weekday,
  format_datetime('%r', datetime) time_of_day,
  format_time('%H', time) hour,
  format_timestamp('%Z', timestamp) timezone
from dt
```