# Flatten an Array


# Description
To flatten an ARRAY in other dialects you could use UNNEST. In Snowflake, that functionality is called [FLATTEN](https://docs.snowflake.com/en/sql-reference/functions/flatten.html). To use it you can do: 

```sql
SELECT 
   VALUE
FROM 
   <TABLE>,
LATERAL FLATTEN(INPUT=> <ARRAY_COLUMN>)
```
where: 
- `<TABLE>` is your table w/ the array column
- `<ARRAY_COLUMN>` is the name of the column with the arrays
> Note: You can select more than VALUE; there are a lot of things returned. See the example below to see.

```sql
with demo_arrays as (
  select array_construct(1,4,'a') arr
)

select 
  * --SHOW ALL THE COLUMNS RETURNED W/ FLATTEN. Common to use VALUE. 
from demo_arrays,
lateral flatten(input => arr)
```
|ARR|SEQ|KEY|PATH|INDEX|VALUE|THIS|
|---|---|---|----|-----|-----|----|
|[1,4,"a"]|1|NULL|[0]|0|1|[1,4,"a"]|
|[1,4,"a"]|1|NULL|[1]|1|4|[1,4,"a"]|
|[1,4,"a"]|1|NULL|[2]|2|"a"|[1,4,"a"]|



# Example:
Here's an array with JSON objects within. We can flatten these objects twice to get to the JSON values.

```sql
With demo_data as (
  select  array_construct(
 parse_json('{
    "AveragePosition": 1,
    "Competitor": "test.com.au",
    "EstimatedImpressions": 4583,
    "SearchTerms": 3,
    "ShareOfClicks": "14.25%",
    "ShareOfSpend": "1.09%"
  }'),
  parse_json('{
    "AveragePosition": 1.9,
    "Competitor": "businessname.com.au",
    "EstimatedImpressions": 10118,
    "SearchTerms": 34,
    "ShareOfClicks": "6.77%",
    "ShareOfSpend": "9.16%"
  }'))
 as demo_array
)
 
select 
  value:AveragePosition,
  value:Competitor,
  value:EstimatedImpressions,
  value:SearchTerms,
  value:ShareofClicks,
  value:ShareofSpend
from (
select value from demo_data,
lateral flatten(input => demo_array))
```
|VALUE:AVERAGEPOSITION|VALUE:COMPETITOR|VALUE:ESTIMATEDIMPRESSIONS|VALUE:SEARCHTERMS|VALUE:SHAREOFCLICKS|VALUE:SHAREOFSPEND|
|-----|-----|-----|-----|------|------|
|1|"test.com.au"|4583|3|"14.25"|"1.09%"|
|1.9|"businessname.com.au"|10118|34|"6.77%"|"9.16"|
