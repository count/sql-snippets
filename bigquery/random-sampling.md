# Random sampling

Explore this snippet with some demo data [here](https://count.co/n/cSN4gyht6Vd?vm=e).

# Description
Taking a random sample of a table is often useful when the table is very large, and you are still working in an exploratory or investigative mode. A query against a small fraction of a table runs much more quickly, and consumes fewer resources.
Here are some example queries to select a random or pseudo-random subset of rows.


# Using rand()
The [RAND](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#rand) function returns a value in [0, 1) (i.e. including zero but not 1). You can use it to sample tables, e.g.:

#### Return 1% of rows

```sql
-- one_percent
select * from spotify.spotify_daily_tracks where rand() < 0.01
```

#### Return 10 rows

```sql
-- ten_rows
select * from spotify.spotify_daily_tracks order by rand() limit 10
```

#### Return approximately 10 rows (discouraged)

```sql
-- ten_rows_approx
select
  *
from spotify.spotify_daily_tracks
where rand() < 10 / (select count(*) from spotify.spotify_daily_tracks)
```


# Using hashing
A hash is a deterministic mapping from one value to another, and so is not random, but can appear random 'enough' to produce a convincing sample. Use a [supported hash function](https://cloud.google.com/bigquery/docs/reference/standard-sql/hash_functions) in BigQuery to produce a pseudo-random ordering of your table:

#### Farm fingerprint

```sql
-- farm_fingerprint
select * from spotify.spotify_artists order by farm_fingerprint(spotify_artists.artist_id) limit 10
```

#### MD5

```sql
-- md5
select * from spotify.spotify_artists order by md5(spotify_artists.artist_id) limit 10
```

#### SHA1

```sql
-- SHA1
select * from spotify.spotify_artists order by sha1(spotify_artists.artist_id) limit 10
```

#### SHA256

```sql
-- SHA256
select * from spotify.spotify_artists order by sha256(spotify_artists.artist_id) limit 10
```

#### SHA512

```sql
-- SHA512
select * from spotify.spotify_artists order by sha512(spotify_artists.artist_id) limit 10
```


# Using TABLESAMPLE
The [TABLESAMPLE](https://cloud.google.com/bigquery/docs/table-sampling) clause is currently in preview status, but it has a major benefit - it doesn't require a full table scan, and therefore can be much cheaper and quicker than the methods above. The downside is that the percentage value must be a literal, so this query is less flexible than the ones above.

#### Tablesample

```sql
-- tablesample
select * from spotify.spotify_daily_tracks tablesample system (10 percent)
```