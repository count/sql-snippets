# Identify Outliers
View interactive notebook [here](https://count.co/n/lgTo572j3We?vm=e).

# Description
Outlier detection is a key step to any analysis. It allows you to quickly spot errors in data collection, or which points you may need to remove before you do any statistical modeling.
There are a number of ways to find outliers, this snippet focuses on the Z score method.
# The Data
For this example, we'll be looking at some data that shows the total number of daily streams for the Top 200 Tracks on Spotify every day since Jan 1 2017:

Data:
|streams | day |
| ------ | --- |
|234109876 | 25/10/2020 |
|... | .... |


# Z-Score
Similar to Method 2, the z-score method tells us the probability of a point occurring in a normal distribution.

![image](https://user-images.githubusercontent.com/42146708/127680954-ed19dc55-3df8-43a5-9db3-beabd7f37fd0.png)

> Image from simplypsychology.org
To use z-scores to identify outliers, we can set an alpha (significance level) that will determine how 'likely' each point will be in order for it to be classified as an outlier.
This method also assumes the data is normally distributed.

```sql
SELECT 
   find_z.*,
   case 
      when abs(z_score) >=abs(<Z_THRESHOLD>) then 'outlier' else 'not outlier' 
   end label
FROM (
   SELECT
      <OTHER_COLUMNS>,
      <OUTLIER_COLUMN>,
      (<OUTLIER_COLUMN> - avg(<OUTLIER_COLUMN>) over ())
/ (stddev(<OUTLIER_COLUMN>) over ()) z_score
   FROM <TABLE> AS data) find_z
ORDER BY abs(z_score) DESC
```
where:
- `<OTHER_COLUMNS>`: Columns selected other than the outlier column
- `<OUTLIER_COLUMN>`: Column name for outlier
- `<Z_THRESHOLD>`: z-score threshold to identify outliers (suggested: 1.96)


## Example


```sql
select find_z.*,
case when abs(z_score) >=abs(1.96) then 'outlier' else 'not outlier' end label
from (select 
  day, 
  streams, 
  (streams - avg(streams) over ())
     / (stddev(streams) over ()) z_score
from data) find_z
order by abs(z_score) desc
```
| day | streams | z_score | label | 
| --- | ------- | ------ | ---------- | 
| 07/01/2017 | 177567024 | -1.971 |outlier | 
| 02/12/2020 | 294978502 | 1.957 | not outlier | 
