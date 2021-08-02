# Formatting Dates and Timestamps

# Description
Formatting dates in Postgres requries memorizing a long list of [patterns](https://www.postgresql.org/docs/9.1/functions-formatting.html). These snippets contain the most common formatts for quick access.

# For Dates:
```sql
with dates as (SELECT day::DATE
FROM generate_series('2021-01-01'::DATE, '2021-01-04'::DATE, '1 day') AS day)

select 
  day,
  to_char(day,'D/ID/day/dy') day_of_week, --[D]Sunday = 1 / [ID]Monday = 1; can change caps for day to be DAY or Day
  to_char(day, 'Month/MM/mon') all_month_formats, -- mon can be MON, or Mon depending on how you want the results capitalized
  to_char(day, 'YYYY/YY') year_formats, -- formatting years
  to_char(day,'DDD/IDDD') day_of_year, -- day of year / IDDD: day of ISO 8601 week-numbering year (001-371; day 1 of the year is Monday of the first ISO week)
  to_char(day,'J') as julian_day, -- days since November 24, 4714 BC at midnight
  to_char(day, 'DD') day_of_month, -- day of the month
  to_char(day,'WW/IW') week_of_year, -- week of year; IW: week number of ISO 8601 week-numbering year (01-53; the first Thursday of the year is in week 1)
  to_char(day,'W') week_of_month, -- week of month (1-4),
  to_char(day,'Q') quarter,
  to_char(day,'Mon DD YYYY') as american_date, -- Month/Day/Year format
  to_char(day,'DD Mon YYYY') as international_date -- Day/Month/Year
from dates
```
|day|day_of_week|all_month_formats|year_formats|day_of_year|julian_day|day_of_month| week_of_year| week_of_month| quarter| american_date| international_date|
|---|-----------|-----------------|------------|-----------|----------|------------|-------------|--------------|--------|--------------|-------------------|
|01/01/01|6/5/friday /fri|January /01/jan|2021/21|001/369|2459216|01|01/53|1|1|Jan 01 2021|01 Jan 2021|
|02/01/01|7/6/saturday /sat|January /01/jan|2021/21|002/370|2459217|02|01/53|1|1|Jan 02 2021|02 Jan 2021|
|03/01/01|1/7/sunday /sun|January /01/jan|2021/21|003/371|2459218|03|01/53|1|1|Jan 03 2021|03 Jan 2021|
|04/01/01|2/1/monday /mon|January /01/jan|2021/21|004/001|2459219|04|01/01|1|1|Jan 04 2021|04 Jan 2021|


# For Timestamps:
```sql
with timestamps as (
  SELECT timeseries_hour
FROM generate_series(
    timestamp with time zone '2021-01-01 00:00:00+02',
    timestamp with time zone '2021-01-01 15:00:00+02',
    '90 minute') AS timeseries_hour
)

select 
  timeseries_hour,
  to_char(timeseries_hour,'HH/HH24') as hour, --HH: 01-12; H24:0-24
  to_char(timeseries_hour,'MI') as minute,
  to_char(timeseries_hour,'SS') as seconds,
  to_char(timeseries_hour,'MS') as milliseconds,
  to_char(timeseries_hour,'US') as microseconds,
  to_char(timeseries_hour,'SSSS') as seconds_after_midnight,
  to_char(timeseries_hour,'AM') as am_pm,
  to_char(timeseries_hour,'TZ') as timezone,
  to_char(timeseries_hour,'HH:MI AM') as local_time
from timestamps
```
|timeseries_hour|hour|minute|seconds|milliseconds|microseconds|seconds_after_midnight|am_pm|timezone|local_time|
|---------------|----|------|-------|------------|------------|----------------------|-----|--------|----------|
|2020-12-31T22:00:00.000Z|10/22|00|00|000|000000|79200|PM|UTC|10:00 PM|
|2020-12-31T23:30:00.000Z|11/23|30|00|000|000000|84600|PM|UTC|11:30 PM|
|2021-01-01T01:00:00.000Z|01/01|00|00|000|000000|3600|AM|UTC|01:00 AM|
|2021-01-01T02:30:00.000Z|02/02|30|00|000|000000|9000|AM|UTC|02:30 AM|
|2021-01-01T04:00:00.000Z|04/04|00|00|000|000000|14400|AM|UTC|04:00 AM|
|2021-01-01T05:30:00.000Z|05/05|30|00|000|000000|19800|AM|UTC|05:30 AM|
|2021-01-01T07:00:00.000Z|07/07|00|00|000|000000|25200|AM|UTC|07:00 AM|
|2021-01-01T08:30:00.000Z|08/08|30|00|000|000000|30600|AM|UTC|08:30 AM|
|2021-01-01T10:00:00.000Z|10/10|00|00|000|000000|36000|AM|UTC|10:00 AM|
|2021-01-01T11:30:00.000Z|11/11|30|00|000|000000|41400|AM|UTC|11:30 AM|
|2021-01-01T13:00:00.000Z|01/13|00|00|000|000000|46800|PM|UTC|01:00 PM|
