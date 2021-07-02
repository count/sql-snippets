# Generating arrays
Explore this snippet with some demo data [here](https://count.co/n/PeGMQjXNBBq?vm=e).



# Description
Snowflake supports the ARRAY column type - here are a few methods for generating arrays.
## Literals
Arrays can contain most types, and they don't have to have the same supertype. 

```sql
select
  array_construct(1, 2, 3) as num_array,
  array_construct('a', 'b', 'c') as str_array,
  array_construct(null, 'hello', 3::double, 4, 5) mixed_array,
  array_construct(current_timestamp(), current_timestamp()) as timestamp_array
```
| NUM_ARRAY | STR_ARRAY | MIXED_ARRAY | TIMESTAMP_ARRAY |
| --------- | --------- | ----------- | --------------- |
| [1,2,3] | ["a","b","c"] | [null,"hello",3,4,5] | ["2021-07-02 11:37:36.205 -0400","2021-07-02 11:37:36.205 -0400"] |

## GENERATE_* functions
Analogous to `numpy.linspace()`.
See this Snowflake [article](https://community.snowflake.com/s/article/Generate-gap-free-sequences-of-numbers-and-dates) for more details.


For generating a sequence of dates without gaps, you can use [GENERATOR](https://docs.snowflake.com/en/sql-reference/functions/generator.html) in combination with [SEQ4](https://docs.snowflake.com/en/sql-reference/functions/seq1.html): 

```sql
select row_number() over (order by seq4()) consecutive_nums
from table(generator(rowcount => 10));
```
| CONSECUTIVE_NUMS |
| ---------------- |
| 1 |
| 2 |
| 3 |
| 4 |
| 5 |
| 6 |
| 7 |
| 8 |
| 9 |
| 10 |

Or to generate a sequence of consecutive dates, you can add [TIMEADD](https://docs.snowflake.com/en/sql-reference/functions/timeadd.html): 

```sql
SELECT TIMEADD('day', row_number() over (order by 1), '2020-08-30')::date date 
FROM TABLE(GENERATOR(ROWCOUNT => 10));
```
| DATE |
| ---------------- |
| 2020-08-30 |
| 2020-09-01 |
| 2020-09-02 |
| 2020-09-03 |
| 2020-09-04 |
| 2020-09-05 |
| 2020-09-06 |
| 2020-09-07 |
| 2020-09-08 |
| 2020-09-09 |

## ARRAY_AGG
Aggregate a column into a single row element.

```sql
SELECT ARRAY_AGG(num)
FROM (SELECT 1 num UNION ALL SELECT 2 num UNION ALL SELECT 3 num)
```
| ARRAY_AGG(NUM) |
| ---------------- |
| [1,2,3] |
