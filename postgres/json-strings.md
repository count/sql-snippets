# Extracting values from JSON strings

Explore this snippet [here](https://count.co/n/sL7bEFcg11Z?vm=e).

# Description
Postgres has supported lots of JSON functionality for a while now - see some examples in the [official documentation](https://www.postgresql.org/docs/current/functions-json.html).
## Operators
There are several operators that act as accessors on JSON data types

```sql
with json as (select '{
  "a": 1,
  "b": "bee",
  "c": [
    4,
    5,
    { "d": [6, 7] }
  ]
}' as text)

select
  text::json as root, -- Cast text to JSON
  text::json->'a' as a, -- Extract number as string
  text::json->'b' as b, -- Extract string
  text::json->'c' as c, -- Extract object
  text::json->'c'->0 as c_first_str, -- Extract array element as string
  text::json->'c'->>0 as c_first_num, -- Extract array element as number
  text::json#>'{c, 2}' as c_third, -- Extract deeply-nested object
  text::json#>'{c, 2}'->'d' as d -- Extract field from deeply-nested object
from json
```

| root                                                | a | b     | c                       | c_first_str | c_first_num | c_third         | d      |
| --------------------------------------------------- | - | ----- | ----------------------- | ----------- | ----------- | --------------- | ------ |
| { "a": 1, "b": "bee", "c": [4, 5, { "d": [6, 7] }]} | 1 | "bee" | [4, 5, { "d": [6, 7] }] | 4           | 4           | { "d": [6, 7] } |	[6, 7] |


## JSON_EXTRACT_PATH
As an alternative to the operator approach, the JSON_EXTRACT_PATH function works by specifying the keys of the JSON fields to be extracted:

```sql
select
  json_extract_path('{
  "a": 1,
  "b": "bee",
  "c": [
    4,
    5,
    { "d": [6, 7] }
  ]
}', 'c', '2', 'd', '0')
```

| json_extract_path |
| ----------------- |
| 6                 |