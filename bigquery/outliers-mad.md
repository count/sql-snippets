# Identify Outliers
View interactive notebook [here](https://count.co/n/lgTo572j3We?vm=e).

# Description
Outlier detection is a key step to any analysis. It allows you to quickly spot errors in data collection, or which points you may need to remove before you do any statistical modeling.
There are a number of ways to find outliers, this snippet focuses on the MAD method.
# The Data
For this example, we'll be looking at some data that shows the total number of daily streams for the Top 200 Tracks on Spotify every day since Jan 1 2017:

Data:
|streams | day |
| ------ | --- |
|234109876 | 25/10/2020 |
|... | .... |


# Method 1: MAD (Median Absolute Deviation)
[MAD](https://en.wikipedia.org/wiki/Median_absolute_deviation) describes how far away a point is from the median. This method can be preferable to other methods, like the z-score method because you don't have to assume the data is normally distributed.

![image](https://user-images.githubusercontent.com/42146708/127674882-abeb638b-5b58-4274-9112-34b52a84c1fd.png)

> "A critical value for the Median Absolute Deviation of 5 has been suggested by workers in Nonparametric statistics, as a Median Absolute Deviation value of just under 1.5 is equivalent to one standard deviation, so MAD = 5 is approximately equal to 3 standard deviations."
> - [Using SQL to detect outliers](https://towardsdatascience.com/using-sql-to-detect-outliers-aff676bb2c1a) by Robert de Graaf

```sql
SELECT 
   diff_to_med.*,
   percentile_cont(diff_to_med.difference, 0.5) over() median_of_diff,
   abs(difference/percentile_cont(difference, 0.5) over()) mad,
   case
      when abs(difference/percentile_cont(difference, 0.5) over()) > <MAD_THRESHOLD> then 'outlier'
      else 'not outlier'
  end label
FROM (
   select 
      <OTHER_COLUMNS> 
      <OUTLIER_COLUMN>, 
      med.median ,
      abs(data.<OUTLIER_COLUMN> - med.median) difference
   from 
      <TABLE> as data
   cross join 
      (
         select distinct 
            percentile_cont(<OUTLIER_COLUMN>,.5) over() median 
         from <TABLE> as data) as med
      ) as diff_to_med
order by mad desc
```
where:
- `<OTHER_COLUMNS>`: Columns selected other than the outlier column
- `<OUTLIER_COLUMN>`: Column name for outlier
- `<MAD_TRESHOLD>`: The outlier threshold. 5 = 3 standard deviations

#### MAD Method Outliers
```sql
select diff_to_med.*, 
  percentile_cont(diff_to_med.difference, 0.5) over() median_of_diff, 
  abs(difference/percentile_cont(difference, 0.5) over()) mad, 
  case 
    when abs(difference/percentile_cont(difference, 0.5) over()) > 5 then 'outlier' 
    else 'not outlier' 
  end label
from (
  select data.day, data.streams, med.median ,abs(data.streams - med.median) difference
from data
cross join (select distinct percentile_cont(streams,.5) over() median from data) as med
) as diff_to_med
order by mad desc
```
| day | streams | median | difference | median_to_diff | mad | label |
| --- | ------- | ------ | ---------- | -------------- | ---- | ----- |
| 24/12/2020 | 555245855 | 235341012.5 | 319904842.5 | 15798261 |20.0249 | outlier |
| 08/01/2017 | 162831491 | 235341012.5 | 72509521.5 | 15798261 |4.59 | not outlier |
