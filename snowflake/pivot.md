# Pivot Tables in Snowflake


# Description
We're all familiar with the power of Pivot tables, but building them in SQL tends to be tricky.
Snowflake has some bespoke features that make this a little easier though. In this context:
> Pivoting a table means turning rows into columns

To pivot in Snowflake, you can use the [PIVOT](https://docs.snowflake.com/en/sql-reference/constructs/pivot.html) keyword: 

```sql
SELECT 
   <COLS>
FROM <TABLE>
   PIVOT ( <aggregate_function> ( <pivot_column> )
            FOR <value_column> IN ( <pivot_value_1> [ , <pivot_value_2> ... ] ) )
```
where: 
- `<COLS>` are all the columns you're selecting
- `<TABLE>` is the table you're pivoting
- `<aggregate_function>` is any of AVG, COUNT, MAX, MIN, or SUM
- `<pivot_column>` is the column that will be aggregated
- `<value_column>` is the column that contains the values from which the column names will be generated
- `<pivot_value_n>` is a list of what will become the pivot columns
# Example:

```sql
with data as (
  select 192.048 HOURS_WATCHED, 'Friends'TITLE, 2018 YEAR union all 
  select 1.869 HOURS_WATCHED, 'Arrested Development' TITLE, 2018 YEAR union all
  select 78.596 HOURS_WATCHED, 'Friends' TITLE, 2019 YEAR union all
  select 2.521 HOURS_WATCHED, 'Arrested Development' TITLE, 2019 YEAR union all
   select 58.134 HOURS_WATCHED, 'Friends' TITLE, 2020 YEAR union all
  select 0.993 HOURS_WATCHED, 'Arrested Development' TITLE, 2020 YEAR 
)
SELECT * FROM data 
PIVOT(sum(hours_watched) for year in (2018, 2019, 2020)) 
```
|TITLE|2018|2019|2020|
|----|----|-----|----|
|Friends|192.048|78.596|59.134|
|Arrested Development|1.869|2.521|0.993|

# References:
- [Pivot Tables in Snowflake ](https://count.co/sql-resources/snowflake/pivot-tables)
