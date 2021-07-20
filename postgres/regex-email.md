# RegEx: Validate Email Addresses

# Description
The only real way to validate an email address is to send a request to it and observe the response, but for the purposes of analytics it's often useful to strip out rows including malformed email addresses.
Using the `SUBSTRING` function, the email validation RegEx comes courtesy of [Evan Carroll](https://dba.stackexchange.com/questions/68266/what-is-the-best-way-to-store-an-email-address-in-postgresql/165923#165923):

# Snippet ✂️

```sql
WITH data AS (
    SELECT UNNEST(ARRAY[
    'a',
    'hello@count.c',
    'hello@count.co',
    '@count.co',
    'a@a.a'
    ]) AS emails
)

SELECT
  emails,
  CASE WHEN
    SUBSTRING(
        LOWER(emails),
        '^[a-zA-Z0-9.!#$%&''''*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$'
    ) IS NOT NULL
    THEN TRUE
   ELSE FALSE
  END AS is_valid
FROM data
```

| emails | is_valid |
| ----- | ----- |
| a | false |
| hello@count.c | true |
| hello@count.co | true |
| @count.co | false |
| a@a.a | true |
