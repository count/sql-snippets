# Parse URL String

Explore the snippet with some demo data [here](https://count.co/n/LM876dhSHtU?vm=e).

# Description

URL strings are a very valuable source of information, but trying to get your regex exactly right is a pain. 
This re-usable snippet can be applied to any URL to extract the exact part you're after. 
Helpfully, Snowflake has a [PARSE_URL](https://docs.snowflake.com/en/sql-reference/functions/parse_url.html) function that returns a JSON object of URL components. 

```sql
SELECT KEY,VALUE 
FROM
   (
      SELECT parsed from
      (
         SELECT
            parse_url(url) as parsed
         FROM my_url
       )
   ),
LATERAL FLATTEN(input=> parsed)
```
where:
- `<url>` is your URL string (e.g. `"https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#Ref1"`


# Usage

To use, just copy out the regex for whatever part of the URL you need. Or capture them all as in the example below: 

```sql
WITH my_url as (select 'https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#newsfeed' as url)

select KEY,VALUE FROM 
(
  SELECT parsed from 
    (
      select 
        parse_url(url) as parsed
      FROM my_url
    )
), 
LATERAL FLATTEN(input => parsed)
```
| KEY        | VALUE                               |
|------------| ----------------------------------- |
| fragment   | "newsfeed"                          |
| host       | "www.yoursite.com"                  |
| parameters | {"myparam1":"123","myparam2":"abc"} |
| port       | (Empty)                             |
| query      | "myparam1=123&myparam2=abc"         |
| scheme     | "https"                             |

# References
- [The Anatomy of a Full Path URL](https://zvelo.com/anatomy-of-full-path-url-hostname-protocol-path-more/)
