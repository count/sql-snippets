# Generate a timeseries of dates and/or times
View an interactive version of this snippet [here](https://count.co/n/Du9nUudW9MH?vm=e).

# Description

In order to conduct timeseries analysis, we need to have a complete timeseries for the data in question. Often, we cannot be certain that every day/hour etc. occurs naturally in our dataset.
Therefore, we to be able to generate a series of dates and/or times to base our query on. 
The [`GENERATE_SERIES`](https://www.postgresql.org/docs/13/functions-srf.html) function will allow us to do this:

```sql
--generate timeseries in hourly increments
SELECT timeseries_hour
FROM generate_series('2021-01-01 00:00:00'::TIMESTAMP, '2021-01-01 03:00:00'::TIMESTAMP, '1 hour') AS timeseries_hour;
```
| timeseries_hour |
|---|
| 2021-01-01 00:00:00.000000 |
| 2021-01-01 01:00:00.000000 |
| 2021-01-01 02:00:00.000000 |
| 2021-01-01 03:00:00.000000 |

```sql
--generate timeseries in daily increments
SELECT timeseries_day::DATE
FROM generate_series('2021-01-01'::DATE, '2021-01-04'::DATE, '1 day') AS timeseries_day;
```

| timeseries_day |
|---|
| 2021-01-01 |
| 2021-01-02 |
| 2021-01-03 |
| 2021-01-04 |
