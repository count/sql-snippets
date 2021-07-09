# Local Timezone to UTC
Explore this snippet with some demo data [here](https://count.co/n/448Ip23n0Ef?vm=e).


# Description
[UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (Coordinated Universal Time) is a universal standard time and can be a great way to compare data across many timezones. 
Often we're given time in a local timezone and need to convert it to UTC. To do that in BigQuery we can just use the [TIMESTAMP](https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions#timestamp) function: 

```sql
SELECT 
   TIMESTAMP(<Local datetime>, <timezone>) as utc_timestamp
```
where: 
- The local datetime can be in the form of a string expression, datetime object or date object.
- The timezone should be one of BigQuery's [time zones](https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions#timezone_definitions).
# Usage
We're given Euro match data in Pacific time and want to convert it to UTC:

```sql
-- match_data
select '2021-06-11 12:00:00' start_time, 'Turkey' Home, 'Italy' Away union all 
select '2021-06-12 06:00:00' start_time, 'Wales' Home, 'Switzerland' Away union all 
select'2021-06-12 09:00:00' start_time, 'Denmark' Home, 'Finland' Away union all 
```
|start_date| Home | Away |
|-------| ----- | ------ |
|2021-06-11 12:00:00| Turkey | Italy |
|2021-06-12 06:00:00| Wales | Switzerland |
|2021-06-12 09:00:00| Denmark | Finland |

```sql
-- tidied
Select 
  timestamp(match_data.start_time,'America/Los_Angeles') utc_start,
  Home,
  Away
from match_data
```
|utc_start| Home | Away |
|-------| ----- | ------ |
|2021-06-11T19:00:00.000Z| Turkey | Italy |
|2021-06-12T13:00:00.000Z| Wales | Switzerland |
|2021-06-12T16:00:00.000Z| Denmark | Finland |
