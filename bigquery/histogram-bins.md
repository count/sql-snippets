# Histogram Bins
Test out this snippet with some demo data [here](https://count.co/n/J7s46HSTsrm).

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
To use, adjust the bin-size until you can see the 'shape' of the data. The following example finds the percentage of whiskies with in $5 bin groupings: 

```sql
SELECT 
  count(*) / (select count(1) from whisky.scotch_reviews) percentage_of_results,
  floor(scotch_reviews.review_point / 5) * 5 bin
from whisky.scotch_reviews
group by bin
```
