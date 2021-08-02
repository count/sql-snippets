# SQL Snippets
 
A curated collection of helpful SQL queries and functions, maintained by [Count](https://count.co). These snippets are also mirrored at a [community forum](https://sql-snippets.count.co).

All [contributions](./CONTRIBUTING.md) are very welcome - let's get useful SQL snippets out of Slack channels and Notion pages, and collect them as a community resource.

### [💬 Join the discussion](https://sql-snippets.count.co)
### [🙋‍♀️ Request a snippet](https://github.com/count/sql-snippets/issues/new?assignees=&labels=help+wanted&template=snippet-request.md&title=%5BSNIPPET+REQUEST%5D+)
### [⁉️ Report a bug](https://github.com/count/sql-snippets/issues/new?assignees=&labels=bug&template=bug_report.md&title=%5BBUG%5D+)
### [↣ Submit a pull request](https://github.com/count/sql-snippets/compare)
### [💌 Get weekly updates](https://sqlsnippets.substack.com/p/coming-soon?r=itwes&utm_campaign=post&utm_medium=web&utm_source=copy)

<!--
Database badges

The colour of the badge is taken from the dominant colour of the database logo.

![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)
![Postgres](https://img.shields.io/badge/Postgres-336791)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)
![SQL Server](https://img.shields.io/badge/SQL%20Server-A91D22)
-->

| Snippet | Databases |
| ------- | --------- |
| <h3>Analytics</h3> | |
| Cohort analysis |  [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/cohort-analysis.md) |
| Cumulative distribution functions | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/cdf.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/cume_dist.md) |
| Compound growth rates | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/compound-growth-rates.md) |
| Generate a binomial distribution | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/binomial-distribution.md) |
| Generate a uniform distribution | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/uniform-distribution.md) |
| Histogram bins | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/histogram-bins.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/histogram-bins.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/histogram-bins.md) |
| Median | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/median.md) |
| Median UDF | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/median-udf.md) |
| Outlier detection: MAD method | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/outliers-mad.md) |
| Outlier detection: Standard Deviation method | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/outliers-stdev.md) |
| Outlier detection: Z-score method | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/outliers-z.md) |
| Random Between | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/rand_between.md) |
| Random sampling | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/random-sampling.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/random-sampling.md) |
| Ranking | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/rank.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/rank.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/rank.md) |
| TF-IDF | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/tf-idf.md) |
| <h3>Arrays</h3>| |
| Arrays to columns | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/array-to-columns.md) |
| Filtering arrays | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/filtering-arrays.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/filtering-arrays.md) |
| Generating arrays | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/generating-arrays.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/generate-arrays.md) |
| Get last array element | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/get-last-array-element.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/get-last-array-element.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/last-array-element.md) |
| Reverse array | [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/array-reverse.md) |
| Transforming arrays | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/transforming-arrays.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/transforming-arrays.md) |
| UDF: Least non-null value in array |  [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/least-udf.md) |
| Greatest/Least value in array |  [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/least-array.md) |
| <h3>Charts</h3>| |
| Bar chart | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/bar-chart.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/bar-chart.md) |
| Horizontal bar chart | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/horizontal-bar.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/horizontal-bar.md) |
| Time series | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/timeseries.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/timeseries.md) |
| 100% stacked bar chart | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/100-stacked-bar.md ) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/100-stacked-bar.md) |
| Heat map | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/heatmap.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/heatmap.md)  |
| Stacked bar and line chart | [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/stacked-bar-line.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/stacked-bar-line.md)  |
| <h3>Dates and times</h3>| |
| Converting from strings | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/convert-string-datetimes.md) |
| Converting to strings | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/convert-datetimes-string.md) |
| Local Timezone to UTC | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/localtz-to-utc.md) |
| Converting epoch/unix to timestamp | [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/convert-epoch-to-timestamp.md) |
| Generate timeseries | [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/generate-timeseries.md) |
| Last day of the month | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/last-day.md) |
| <h3>Geography</h3>| |
| Calculating the distance between two points | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/geographical-distance.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/geographical-distance.md) |
| <h3>JSON</h3>| |
| Parsing JSON strings | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/json-strings.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/json-strings.md) |
| <h3>Regular & String expressions</h3>| |
| Parsing URLs | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/regex-parse-url.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/parse-url.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/regex-parse-url.md) |
| Validating emails | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/regex-email.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/regex-email.md) |
| Extract repeating words |  [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/repeating-words.md) |
| Split on Uppercase characters | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/split-uppercase.md) |
| Replace multple conditions | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/replace-multiples.md) |
| CONCAT with NULLS |  [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/concat-nulls.md.md) |
| <h3>Searching</h3>| |
| Search for text in Stored Procedures | [![SQL Server](https://img.shields.io/badge/SQL%20Server-A91D22)](./mssql/search-stored-procedures.md) |
| <h3>Transformation</h3>| |
| Pivot | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/pivot.md) |
| Unpivoting / melting | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/unpivot-melt.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/unpivot-melt.md) | 
| Replace NULLs | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/replace-null.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/replace-null.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/replace-null.md) |
| Replace empty string with NULL | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/replace-empty-strings-null.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/replace-empty-strings-null.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/replace-empty-strings-null.md) |
| Counting NULLs | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/count-nulls.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/count-nulls.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/count-nulls.md) |
| Splitting single columns into rows | [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/split-column-to-rows.md) |
| Latest Row (deduplicate) | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/latest-row.md) |
| <h3>Window functions</h3>| |
| Moving averages | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/moving-average.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/moving-average.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/moving-average.md) |
| Running totals | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/running-total.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/running-total.md) |
| Year-on-year changes | [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/yoy.md) [![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)](./snowflake/yoy.md) |
| Creating equal-sized buckets | [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/ntile.md) |

## Other resources
- [bigquery-utils](https://github.com/GoogleCloudPlatform/bigquery-utils) - a BigQuery-specific collection of scripts
