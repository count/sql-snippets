#  Last day of the month

## Description

In some SQL dialects there is a LAST_DAY function, but there isn’t one in BigQuery.
We can achieve the same by taking the day before the first day of the following month.

1. `DATE_ADD(<date>, INTERVAL 1 MONTH)` sets the reference date to the following month.
2. `DATE_TRUNC(<date>, MONTH)` gets the first day of that month.
3. `DATE_SUB(<date>, INTERVAL 1 DAY)` gets the day before. `<date>-1` can be used as well.

## Example: Last day of the current month (local time)

```
SELECT
DATE_TRUNC(DATE_ADD(CURRENT_DATE('Europe/Madrid'), INTERVAL 1 MONTH), MONTH) - 1
``` 
If no time zone is specified as an argument to CURRENT_DATE, the default time zone, UTC, is used.
