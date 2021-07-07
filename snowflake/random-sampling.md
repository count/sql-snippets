# Random sampling

Explore this snippet with some demo data [here](https://count.co/n/UekRUzNjL4V?vm=e).

# Description

Taking a random sample of a table is often useful when the table is very large, and you are still working in an exploratory or investigative mode. A query against a small fraction of a table runs much more quickly, and consumes fewer resources.
Here are some example queries to select a random or pseudo-random subset of rows.

# Using RANDOM
The [RANDOM](https://docs.snowflake.com/en/sql-reference/functions/random.html) function returns a value in [0, 1) (i.e. including zero but not 1). You can use it to sample tables, e.g.:

#### Return 1% of rows

```sql
select * from PUBLIC.SPOTIFY_DAILY_TRACKS QUALIFY PERCENT_RANK() OVER (ORDER BY RANDOM()) <= 0.01
```

#### Return 10 rows

```sql
select * from PUBLIC.SPOTIFY_DAILY_TRACKS order by random() limit 10
```

# Using hashing

A hash is a deterministic mapping from one value to another, and so is not random, but can appear random 'enough' to produce a convincing sample. Use a [supported hash function ](https://docs.snowflake.com/en/sql-reference/functions-hash-scalar.html)in Snowflake to produce a pseudo-random ordering of your table:

#### MD5

```sql
select * from PUBLIC.SPOTIFY_DAILY_TRACKS order by md5(SPOTIFY_DAILY_TRACKS.TRACK_ID) limit 10
```

#### SHA1

```sql
select * from PUBLIC.SPOTIFY_DAILY_TRACKS order by sha1(SPOTIFY_DAILY_TRACKS.TRACK_ID) limit 10
```

# Using TABLESAMPLE
The [TABLESAMPLE](https://docs.snowflake.com/en/sql-reference/constructs/sample.html) has a major benefit - it doesn't require a full table scan, and therefore can be much cheaper and quicker than the methods above. The downside is that the percentage value must be a literal, so this query is less flexible than the ones above.

#### Tablesample

```sql
select * from PUBLIC.SPOTIFY_DAILY_TRACKS tablesample (1) -- 1% of the data
```
