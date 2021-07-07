# Distance between longitude/latitude pairs

Explore this snippet [here](https://count.co/n/nqZbPem1fKi?vm=e).

# Description

It is a surprisingly difficult problem to calculate an accurate distance between two points on the Earth, both because of the spherical geometry and the uneven shape of the ground.
Fortunately Snowflake has robust support for geographical primitives - the[`ST_DISTANCE`](https://docs.snowflake.com/en/sql-reference/functions/st_distance.html) function can calculate the distance in metres between two [`ST_POINT`](https://docs.snowflake.com/en/sql-reference/functions/st_makepoint.html)s


```sql
with points as (
  -- Longitudes and latitudes are in degrees
  select 10 as from_long, 12 as from_lat, 15 as to_long, 17 as to_lat
)

select
  st_distance(
    st_makepoint(from_long, from_lat),
    st_makepoint(to_long,   to_lat)
  ) as dist_in_metres
from points
```

| DIST_IN_METRES |
| -------------- |
| 773697.017019  |