# Converting strings to dates/times

Explore this snippet [here](https://count.co/n/wvs19LPsUrQ?vm=e).

# Description
BigQuery supports the `DATE` / `DATETIME` / `TIMESTAMP` / `TIME` data types, all of which have different `STRING` representations.

## CAST
If a string is formatted correctly, the `CAST` function will convert it into the appropriate type:

```sql
select
  cast('2017-06-04 14:44:00' AS datetime) AS datetime,
  cast('2017-06-04 14:44:00 Europe/Berlin' AS timestamp) AS timestamp,
  cast('2017-06-04' AS date) AS date,
  cast( '14:44:00' as time) time
```


## PARSE_*
Alternatively, if the format of the string is non-standard but consistent, it is possible to define a custom string format using one of the `PARSE_DATE` / `PARSE_DATETIME` etc. functions:

```sql
select
  parse_datetime(
    '%A %B %d, %Y %H:%M:%S',
    'Thursday January 7, 2021 12:04:33'
  ) as parsed_datetime
```