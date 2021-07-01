# Extracting values from JSON strings

Explore this snippet [here](https://count.co/n/P88ejfuK7iv?vm=e).

# Description
BigQuery has a rich set of functionality for working with JSON - see any function with [JSON in the name](https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions). 
Most of the BigQuery JSON functions require the use of a [JSONPath](https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions#JSONPath_format) parameter, which defines how to access the part of the JSON object that is required.
## JSON_QUERY
This function simply returns the JSON-string representation of the part of the object that is requested:

```sql
with json as (select '''{
  "a": 1,
  "b": "bee",
  "c": [
    4,
    5,
    { "d": [6, 7] }
  ]
}''' as text)

select
  -- Note - all of these result columns are strings, and are formatted as JSON
  json_query(text, '$') as root,
  json_query(text, '$.a') as a,
  json_query(text, '$.b') as b,
  json_query(text, '$.c') as c,
  json_query(text, '$.c[0]') as c_first,
  json_query(text, '$.c[2]') as c_third,
  json_query(text, '$.c[2].d') as d,
from json
```


## JSON_VALUE
This function is similar to `JSON_QUERY`, but tries to convert some values to native data types - namely strings, integers and booleans. If the accessed part of the JSON object is not a simple data type, `JSON_VALUE` returns null:

```sql
with json as (select '''{
  "a": 1,
  "b": "bee",
  "c": [
    4,
    5,
    { "d": [6, 7] }
  ]
}''' as text)

select
  json_value(text, '$') as root, -- = null
  json_value(text, '$.a') as a,
  json_value(text, '$.b') as b,
  json_value(text, '$.c') as c, -- = null
  json_value(text, '$.c[0]') as c_first,
  json_value(text, '$.c[2]') as c_third, -- = null
  json_value(text, '$.c[2].d') as d, -- = null
from json
```