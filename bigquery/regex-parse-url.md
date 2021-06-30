# Regex: Parse URL String
Explore the query with some demo data [here](https://count.co/n/U2RGnRt91pB?vm=e).

# Description
URL strings are a very valuable source of information, but trying to get your regex exactly right is a pain. 
This re-usable snippet can be applied to any URL to extract the exact part you're after. 

```sql
SELECT 
   REGEXP_EXTRACT(<url>, r'(?:[a-zA-Z]+://)?([a-zA-Z0-9-.]+)/?') as host,
   REGEXP_EXTRACT(<url>, r'(?:[a-zA-Z]+://)?(?:[a-zA-Z0-9-.]+)/{1}([a-zA-Z0-9-./]+)') as path,
   REGEXP_EXTRACT(<url>, r'\?(.*)') as query,
   REGEXP_EXTRACT(<url>, r'#(.*)') as ref,
   REGEXP_EXTRACT(<url>, r'^([a-zA-Z]+)://') as protocol
FROM
   '<project.schema.table>'
```
where:
- `<url>` is your URL string (e.g. `"https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#Ref1"`
- `host` is the domain name (e.g. `yoursite.com`)
- `path` is the page path on the website (e.g. `pricing/details`)
- `query` is the string of query parameters in the url (e.g. `myparam1=123`)
- `ref` is the reference or where the user came from before visiting the url (e.g. `#newsfeed`)
- `protocol` is how the data is transferred between the host and server (e.g. `https`)


# Usage
To use, just copy out the regex for whatever part of the URL you need. Or capture them all as in the example below: 

```sql
WITH my_url as (select 'https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#newsfeed' as url)
SELECT
  REGEXP_EXTRACT(url, r'(?:[a-zA-Z]+://)?([a-zA-Z0-9-.]+)/?') as host,
  REGEXP_EXTRACT(url, r'(?:[a-zA-Z]+://)?(?:[a-zA-Z0-9-.]+)/{1}([a-zA-Z0-9-./]+)') as path,
  REGEXP_EXTRACT(url, r'\?(.*)') as query,
  REGEXP_EXTRACT(url, r'#(.*)') as ref,
  REGEXP_EXTRACT(url, r'^([a-zA-Z]+)://') as protocol
FROM my_url
```
| host | path | query | ref | protocol|
| --- | ------ | ----- | --- | ------|
| www.yoursite.com | pricing/details | myparam1=123&myparam2=abc#newsfeed | newsfeed | https |


# References Helpful Links
- [The Anatomy of a Full Path URL](https://zvelo.com/anatomy-of-full-path-url-hostname-protocol-path-more/)
- [GoogleCloudPlatform/bigquery-utils](https://github.com/GoogleCloudPlatform/bigquery-utils/blob/master/udfs/community/url_parse.sql)
