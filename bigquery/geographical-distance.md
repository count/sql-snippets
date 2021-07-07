# Distance between longitude/latitude pairs

Explore this snippet [here](https://count.co/n/BcHM84o8uYb?vm=e).

# Description

It is a surprisingly difficult problem to calculate an accurate distance between two points on the Earth, both because of the spherical geometry and the uneven shape of the ground.
Fortunately BigQuery has robust support for geographical primitives - the [`ST_DISTANCE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions#st_distance) function can calculate the distance in metres between two [`ST_GEOGPOINT`](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions#st_geogpoint)s:

```sql
with points as (
  -- Longitudes and latitudes are in degrees
  select 10 as from_long, 12 as from_lat, 15 as to_long, 17 as to_lat
)

select
  st_distance(
    st_geogpoint(from_long, from_lat),
    st_geogpoint(to_long,   to_lat)
  ) as dist_in_metres
from points
```

| dist_in_metres |
| -------------- |
| 773697.017019  |