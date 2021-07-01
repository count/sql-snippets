# Regex: Validate email address

Explore this snippet [here](https://count.co/n/aAHVEQ74TYd?vm=e).

# Description
The only real way to validate an email address is to send an email to it, but for the purposes of analytics it's often useful to strip out rows including malformed email address.
Using the `REGEXP_CONTAINS` function, this example comes courtesy of Reddit user [chrisgoddard](https://www.reddit.com/r/bigquery/comments/dshge0/udf_for_email_validation/f6r7rpt?utm_source=share&utm_medium=web2x&context=3):

```sql
with emails as (
  select * from unnest([
    'a',
    'hello@count.c',
    'hello@count.co',
    '@count.co',
    'a@a.a'
  ]) as email
)

select
  email,
  regexp_contains(
    lower(email),
    "^[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])$"
  ) valid
from emails
```