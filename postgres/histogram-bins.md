# Histogram Bins
View an interactive version of this snippet [here](https://count.co/n/27wc3pvc5R0?vm=e).

# Description
Creating histograms using SQL can be tricky. This template will let you quickly create the bins needed to create a histogram:

```sql
SELECT 
   COUNT(1) / (SELECT COUNT(1) FROM <schema.table> ) AS percentage_of_results
   FLOOR(<column>/ <bin-size>) * <bin-size> AS bin
FROM
   <schema.table>
GROUP BY 
   bin
```
where:
- `<column>` is your column you want to turn into a histogram
- `<bin-size>` is the width of each bin. You can adjust this to get the desired level of detail for the histogram.

# Example
Adjust the bin-size until you can see the 'shape' of the data. The following example finds the percentage of fruits with 5-point review groupings. The reviews are on a 0-100 scale: 

```sql
WITH data AS (
    SELECT CASE
        WHEN RANDOM() <= 0.25 THEN 'tennessee'
        WHEN RANDOM() <= 0.5 THEN 'colonel'
        WHEN RANDOM() <= 0.75 THEN 'pikesville'
      ELSE 'michters'
    END AS whiskey,
    CASE
        WHEN RANDOM() <= 0.20 THEN 1
        WHEN RANDOM() <= 0.40 THEN 2
        WHEN RANDOM() <= 0.60 THEN 3
        WHEN RANDOM() <= 0.80 THEN 4
      ELSE 5
    END AS star_rating,
    review_date
    FROM generate_series('2021-01-01'::TIMESTAMP,'2021-01-31'::TIMESTAMP,  '3 DAY') AS review_date
)

SELECT 
  COUNT(*) / (SELECT COUNT(1)::FLOAT FROM data) AS percentage_of_results,
  FLOOR(data.star_rating / 1) * 1 bin
FROM data
GROUP BY bin
```
| percentage_of_results | bin |
| ----- | ----- |
| 0.2727272727272727 | 3 |
| 0.09090909090909091 | 5 |
| 0.18181818181818182 | 1 |
| 0.45454545454545453 | 2 |

