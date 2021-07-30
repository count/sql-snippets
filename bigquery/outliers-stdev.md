# Identify Outliers
View interactive notebook [here](https://count.co/n/lgTo572j3We?vm=e).

# Description
Outlier detection is a key step to any analysis. It allows you to quickly spot errors in data collection, or which points you may need to remove before you do any statistical modeling.
There are a number of ways to find outliers, this snippet focuses on the Standard Deviation method.
# The Data
For this example, we'll be looking at some data that shows the total number of daily streams for the Top 200 Tracks on Spotify every day since Jan 1 2017:

Data:
|streams | day |
| ------ | --- |
|234109876 | 25/10/2020 |
|... | .... |


# Standard Deviation
The most common way outliers are detected is by finding points outside a certain number of sigmas (or standard deviations) from the mean.
If the data is normally distributed, then the following rules apply:

![image](https://user-images.githubusercontent.com/42146708/127679527-731d73ba-2294-4e0e-b7d3-30386d3a1d6f.png)

> Image from anomoly.io
So, identifying points 2 or 3 sigmas away from the mean should lead us to our outliers.

## Example

```sql
SELECT
   data.*,
   case
      when <OUTLIER_COLUMN> between mean - 3 * stdev and mean + 3 * stdev 
         then 'not outlier'
      else 'outlier'
   end label,
   mean - <SIGMAS> * stdev lower_bound,
   mean + <SIGMAS> * stdev upper_bound
FROM data
CROSS JOIN 
   (
    SELECT
      avg(<OUTLIER_COLUMN>) mean,
      stddev_samp(data.streams<OUTLIER_COLUMN>    
    FROM <TABLE> AS data ) mean_sd
ORDER BY label DESC
```
where:
- `<OTHER_COLUMNS>`: Columns selected other than the outlier column
- `<OUTLIER_COLUMN>`: Column name for outlier
- `<SIGMAS>`: Number of standard deviations to identify the outliers (suggested: 3)

#### Standard Deviation Method Outliers
```sql
select 
  data.*, 
  case 
    when streams between mean - 3 * stdev and mean + 3 * stdev then 'not outlier' 
    else 'outlier' 
  end label,
  mean - 3 * stdev lower_bound,
  mean + 3 * stdev upper_bound
from data
cross join (select 
  avg(data.streams) mean, 
  stddev_samp(data.streams) stdev
from data ) mean_sd
order by label desc
```
| day | streams | label | lower_bound | upper_bound | 
| --- | ------- | ------ | ---------- | -------------- | 
| 29/06/2018 | 350167113 | outlier |146828000.339 | 326135386.804 | 
| 08/01/2017 | 162831491 | not outlier | 146828000.339 | 326135386.804 | 
