
# Latest Row (deduplicate)
View interactive snippet [here](https://count.co/n/zFoUJuIYOrC?vm=e).

## Query to return only the latest row based on a column or columns

With BigQuery it is common to have duplicates in your tables especially datalake ones so we need to remove duplicates and often want the latest version of each row. We can make use of the ROW_NUMBER window/analytical function to get a count of each record with the same key and then using QUALIFY to only return the 1st row order by the latest first.

Here we have some rows with duplicates. I've added the ROW_NUMBER so we can see the values it returns.

```sql
WITH
  sample_data AS (
  SELECT 1 AS product_id,'widgit' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 2 AS product_id,'borble' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 3 AS product_id,'connector' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 1 AS product_id,'widgit' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-02-01' AS modified_date
  UNION ALL
  SELECT 2 AS product_id,'borble' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-03-01' AS modified_date )
SELECT
  *, ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY modified_date DESC, created_date DESC) as row_no
FROM
  sample_data
```

We can use QUALIFY to only return a single row. Also if we use a named subquery/cte we can work with the deduped as if it was just a table.
The product_id can be replaced with a list of columns to specify the unique rows.
```sql
WITH
  sample_data AS (
  SELECT 1 AS product_id,'widgit' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 2 AS product_id,'borble' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 3 AS product_id,'connector' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-01-01' AS modified_date
  UNION ALL
  SELECT 1 AS product_id,'widgit' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-02-01' AS modified_date
  UNION ALL
  SELECT 2 AS product_id,'borble' AS description,'Active' AS status,'2019-01-01' AS created_date,'2021-03-01' AS modified_date 
  ),
data_deduped as (
SELECT
  *
FROM
  sample_data
WHERE
  1=1 QUALIFY ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY modified_date DESC, created_date DESC)=1
  )
SELECT
  *
FROM
  data_deduped
```


https://cloud.google.com/bigquery/docs/reference/standard-sql/analytic-function-concepts
https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#with_clause
