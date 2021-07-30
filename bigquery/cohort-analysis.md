# Cohort Analysis
View interactive notebook [here](https://count.co/n/INCbY1QUomH?vm=e).


# Description
A cohort analysis groups users based on certain traits and compares their behavior across groups and over time. 
The most common way of grouping users is by when they first joined. This definition will change for each company and even use case but it's generally taken to be the day or week that a user joined. All users who joined at the same time are grouped together. Again, this can be made more complex if needed. 
The simplest way to start comparing cohorts is seeing if they returned to your site/product in the weeks after they joined. You can continue to refine what patterns of behavior you're looking for.
But for this example, we'll do the following:
- Assign users a cohort based on the week they created their account
- For each cohort determine the % of users that returned for the 10 weeks after joining
## The Data
- The way your data is structured will greatly determine how you do your analysis. Most commonly, there's a transaction table that has timestamped events such as 'create account' or 'purchase' for each user. This is what we'll use for our example. 
- The data comes from [BigQuery's Example Google Analytics dataset](https://console.cloud.google.com/bigquery). 
## The Code
- In this example I've gone step by step, but to do it all in one query you could try: 

```sql
WITH cohorts as 
(
   SELECT 
      fullVisitorId,
      date_trunc(first_value(date) over (partition by fullVisitorId order by date asc),week) cohort
FROM data
),
activity as 
(
   SELECT
      cohorts.cohort,
      date_diff(date,cohort,week) weeks_since_joining,
      count(distinct data.fullVisitorId) visitors
   FROM
      cohorts
   LEFT JOIN data ON cohorts.fullVisitorId = data.fullVisitorId
   WHERE 
      date_diff(date,cohort,week) between 1 and 10
   GROUP BY cohort, weeks_since_joining
),
cohorts_with_weeks as 
(
   SELECT distinct
      cohort,
      count(distinct fullVisitorId) cohort_size,
      weeks_since_joining
   FROM cohorts
   CROSS JOIN
   UNNEST(generate_array(1,10)) weeks_since_joining
   GROUP BY cohort, weeks_since_joining
),
SELECT
   cohorts_with_weeks.cohort,
   cohorts_with_weeks.weeks_since_joining,
   cohorts_with_weeks.cohort_size,
   coalesce(activity.visitors,0)/cohorts_with_weeks.cohort_size per_active,
FROM
   cohorts_with_weeks
LEFT JOIN activity 
   ON cohorts_with_weeks.cohort = activity.cohort 
   AND cohorts_with_weeks.weeks_since_joining = activity.weeks_since_joining
```

#### Data Preview
| fullVisitorId | visitNumber | visitId | date | ... | channelGrouping |
| ---- | ----- |----- |---- | ----- | ----- |
|6453870986532490896 | 1 | 1473801936 | 2016-09-13 | ... | Display | 
|2287637838474850444 | 5 | 1473815616 | 2016-09-13 | ... | Direct | 

### 1. Assign each user a cohort based on the week they first appeared
```sql
--cohorts
select 
  fullVisitorId,
  date_trunc(first_value(date) over (partition by fullVisitorId order by date asc),week) cohort
from data
```
|fullVisitorId | cohort |
| ---- | ---- |
| 0002871498069867123 | 21/08/2016 |
| 0020502892485275044 | 11/09/2016 |
|.... | .... |


### 2. For each cohort find the number of users that have returned for weeks 1-10
```sql
--activity
select 
  cohorts.cohort,
  date_diff(date,cohort,week) weeks_since_joining,
  count(distinct data.fullVisitorId) visitors
from 
  cohorts 
left join data on cohorts.fullVisitorId = data.fullVisitorId
where date_diff(date,cohort,week) between 1 and 10
group by
  cohort, weeks_since_joining
```

|cohort | weeks_since_joining | visitors|
|----| --- | ---- |
|2016-07-31| 2 | 7 |
|2016-07-31| 1 | 6 |
|2016-07-31| 9 | 1 |
|2016-07-31| 5 | 1 |
|.... | .... | .... | 


### 3. Turn that into a percentage of users each week (being sure to include weeks where no users returned)
```sql
--cohorts_with_weeks
select distinct 
  cohort, 
  count(distinct fullVisitorId) cohort_size, 
  weeks_since_joining 
from cohorts 
cross join 
  unnest(generate_array(1,10)) weeks_since_joining
group by cohort, weeks_since_joining
```
|cohort| cohort_size | weeks_since_joining | 
|----- |----- |----- |
| 2016-08-21| 263 | 1 |
|2016-08-21| 263 | 2 |
|2016-08-21| 263 | 3 |
|2016-08-21| 263 | 4 |
|2016-08-21| 263 | 5 |
|2016-08-21| 263 | 6 |
|2016-08-21| 263 | 7 |
|2016-08-21| 263 | 8 |
|2016-08-21| 263 | 9 |
|2016-08-21| 263 | 10 |
|...| ... | ... |

```sql
--cohort_table
Select 
  cohorts_with_weeks.cohort,
  cohorts_with_weeks.cohort_size,
  cohorts_with_weeks.weeks_since_joining,
  coalesce(activity.visitors,0)/cohorts_with_weeks.cohort_size per_active,
from 
  cohorts_with_weeks
left join activity on cohorts_with_weeks.cohort = activity.cohort and cohorts_with_weeks.weeks_since_joining = activity.weeks_since_joining
```
|cohort| cohort_size| weeks_since_joining | per_active |
| ---- |---- | ---- | ---- | 
| 2016-08-21| 263 | 1 | 0.015 |
|2016-08-21| 263 | 2 | 0.019 |
|2016-08-21| 263 | 3 | 0.023 |
| ... | ... | ... | .... |

Now we can see our cohort table. 

<img width="754" alt="Screenshot 2021-07-30 at 5 53 02 pm" src="https://user-images.githubusercontent.com/42146708/127686174-9c6597af-a020-486d-9fee-2acd7b005c16.png">
