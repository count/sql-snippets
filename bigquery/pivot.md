# Pivot in BigQuery
View interactive notebook [here](https://count.co/n/MqDQcqT18qS?vm=e)

# Description
Pivoting data is a helpful process of any analysis. Recently BigQuery announced a new [PIVOT](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#pivot_operator) operator that will make this easier than ever. 

```sql
SELECT 
   <COLS>
FROM
   <TABLE>
PIVOT(<AGG_FN>(<AGG_COL>) FOR <PIVOT_COL> IN (<val1> [as alias], <val2> [as alias], ... <valn> [as alias]) [as alias]
```
where:
- anything in brackets is opional
- `<COLS>` is the list of columns returned in the query
- `<TABLE>` is the table you a pivoting
  - may not produce a value able or be a subquery using `SELECT AS STRUCT`
- `<AGG_FN>` is an aggregate function that aggregates all input rows across the values of the `<PIVOT_COL>`
  - may reference columns in `<TABLE>` and correlated columns, but none defined by the `PIVOT` clause itself
  - must have only one argument
  - Except for `COUNT`, you can only use aggregate functions that ignore `NULL` inputs
  - if using `COUNT`, you can use `*` as an argument
- `<AGG_COL>` is the column that will be aggregated across the values of `<PIVOT_COL>`
- `<PIVOT_COL>` is the column that will have its values pivoted
  - values must be valid column names (so no spaces or special characters)
  - use the aliases to ensure they are the right format
# Example
In this case we'll see how many matches some popular tennis players have won in the Grand Slams. 

```sql
--tennis_data
select 
  winner_name,
  count(1) wins, 
  initcap(tourney_name) tournament
from 
  tennis.matches_partitioned
where winner_name in ('Novak Djokovic','Roger Federer','Rafael Nadal','Serena Williams')
and tourney_level = 'G'
group by winner_name, tournament
```
| winner_name | wins | tournament|
| ----------- | ---- | ---------|
| Roger Federer | 91 | Us Open |
| Roger Federer | 103 | Australian Open |
| Roger Federer | 102 | Wimbledon |
| Roger Federer | 70 | Roland Garros |

```sql
select * 
from 
  tennis_data 
  pivot(sum(wins) for tournament in ('Wimbledon','Us Open' as US_Open,'Roland Garros' as French_Open,'Australian Open' as Aussie_Open ))
```

| winner_name | Wimbledon | US_Open | French_Open | Aussie_Open |
| ----------- | ---- | ---------| ---------| ---------|
| Roger Federer | 102 | 91 | 70 | 103 |

# Other links
- https://towardsdatascience.com/pivot-in-bigquery-4eefde28b3be
