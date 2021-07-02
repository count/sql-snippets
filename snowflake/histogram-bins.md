# Histogram Bins
Explore this snippet with some demo data [here](https://count.co/n/DlXMLRZqo6n?vm=e).


# Description
Creating histograms using SQL can be tricky. This snippet will let you quickly create the bins needed to create a histogram:

```sql
SELECT 
   COUNT(1) / (SELECT COUNT(1) FROM '<project.schema.table>' ) percentage_of_results
   FLOOR(<column>/ <bin-size>) * <bin-size> as bin
FROM
   '<project.schema.table>'
GROUP BY 
   bin
```
where:
- `<column>` is your column you want to turn into a histogram
- `<bin-size>` is the width of each bin. You can adjust this to get the desired level of detail for the histogram.


# Usage
To use, adjust the bin-size until you can see the 'shape' of the data. The following example finds the percentage of days in London by 5-degree temperature groupings. 

```sql
SELECT 
  count(*) / (select count(1) from PUBLIC.LONDON_WEATHER) percentage_of_results,
  floor(LONDON_WEATHER.TEMP / 5) * 5 bin
from PUBLIC.LONDON_WEATHER
group by bin
order by bin
```
| PERCENTAGE_OF_RESULTS | BIN |
| ---- | ---- |
| 0.001 | 20 |
| 0.001 | 25 |
| ... | ... |
| 0.014 | 75 |
| 0.001 | 80 |
