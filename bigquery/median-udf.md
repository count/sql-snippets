# MEDIAN UDF
Explore the function with some demo data [here](https://count.co/n/RHmVhHzZpIp?vm=e).

# Description
BigQuery does not have an explicit `MEDIAN` function. The following snippet will show you how to create a custom function for `MEDIAN` in your BigQuery instance. 


# UDF (User Defined Function)
Use the following snippet to create your median function in the BigQuery console: 

```sql
CREATE OR REPLACE FUNCTION <DATASET>.median(arr ANY TYPE) AS ((
 SELECT IF (
    MOD(ARRAY_LENGTH(arr), 2) = 0,
    (arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2) - 1)] + arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))]) / 2,
    arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))]
  )
 FROM
 (SELECT ARRAY_AGG(x ORDER BY x) AS arr FROM UNNEST(arr) AS x)
));
```


# Usage
To call the `MEDIAN` function defined above, use the following: 

```sql
SELECT <DATASET>.median(ARRAY_AGG(<COLUMN_NAME>)) as median
FROM <PROJECT.DATASET.TABLE>
```
### Example
Find the median number of followers for an artist on Spotify:

```sql
SELECT
  spotify.median(array_agg(followers)) as median_followers
FROM
  spotify.spotify_artists
```
| median_followers  |
| ---  |
| 1358146.5 |
