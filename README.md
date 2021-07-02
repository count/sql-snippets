# SQL Snippets
 
A curated collection of helpful SQL queries and functions, maintained by [Count](https://count.co).

All [contributions](./CONTRIBUTING.md) are very welcome - let's get useful SQL snippets out of Slack channels and Notion pages, and collect them as a community resource.

### [🙋‍♀️ Request a snippet](https://github.com/count/sql-snippets/issues/new?assignees=&labels=help+wanted&template=snippet-request.md&title=%5BSNIPPET+REQUEST%5D+)
### [⁉️ Report a bug](https://github.com/count/sql-snippets/issues/new?assignees=&labels=bug&template=bug_report.md&title=%5BBUG%5D+)
### [↣ Submit a pull request](https://github.com/count/sql-snippets/compare)

## Categories

<!--
Database badges

The colour of the badge is taken from the dominant colour of the database logo.

![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)
![Postgres](https://img.shields.io/badge/Postgres-336791)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8)
![SQL Server](https://img.shields.io/badge/SQL%20Server-A91D22)
-->

### Arrays
- Arrays to columns [![BigQuery](https://img.shields.io/badge/BigQuery-4387FB)](./bigquery/array-to-columns.md)
- Filtering arrays [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/filtering-arrays.md)
- Generating arrays [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/generating-arrays.md) 
- Get last array element [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/get-last-array-element.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/get-last-array-element.md)
- Reverse array [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/array-reverse.md)
- Transforming arrays [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/transforming-arrays.md) 

### Dates and times
- Converting from strings [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/convert-string-datetimes.md)
- Converting to strings [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/convert-datetimes-string.md)

### JSON
- Parsing JSON strings [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/json-strings.md) [![Postgres](https://img.shields.io/badge/Postgres-336791)](./postgres/json-strings.md)


### Regular expressions
- Parsing URLs [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/regex-parse-url.md)
- Validating emails [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/regex-email.md)

### Searching
- Search for text in Stored Procedures [![SQL Server](https://img.shields.io/badge/SQL%20Server-A91D22)](./mssql/search-stored-procedures.md) 

### Window functions
- Moving averages [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/moving-average.md)
- Running totals [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/running-total.md)
- Year-on-year changes [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/yoy.md)

### Uncategorised
- Cumulative distribution functions [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/cdf.md)
- Compound growth rates [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/compound-growth-rates.md)
- Histogram bins [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/histogram-bins.md)
- Median UDF [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/median-udf.md)
- Median [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/median.md)
- Random sampling [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/random-sampling.md)
- Ranking [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/rank.md)
- Unpivoting / melting [![BigQuery](https://img.shields.io/badge/BigQuery-4387fb)](./bigquery/unpivot-melt.md)


## Other resources
- [bigquery-utils](https://github.com/GoogleCloudPlatform/bigquery-utils) - a BigQuery-specific collection of scripts
