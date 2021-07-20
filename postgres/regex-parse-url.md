# RegEx: Parse URL String
View an interactive version of this snippet [here](https://count.co/n/rpoUsuw0s3r?vm=e).


# Description
URL strings are a very valuable source of information, but trying to get your RegEx exactly right is a pain. 
This re-usable snippet can be applied to any URL to extract the exact part you're after. 

# Snippet ✂️

```sql
SELECT
  SUBSTRING(my_url, '^([a-zA-Z]+)://') AS protocol,
  SUBSTRING(my_url, '(?:[a-zA-Z]+:\/\/)?([a-zA-Z0-9.]+)\/?') AS host,
  SUBSTRING(my_url, '(?:[a-zA-Z]+://)?(?:[a-zA-Z0-9.]+)/{1}([a-zA-Z0-9./]+)') AS path,
  SUBSTRING(my_url, '\?(.*)') AS query,
  SUBSTRING(my_url, '#(.*)') AS ref
FROM <schema.tablename>
```
where:
- `<my_url>` is your URL string (e.g. `"https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#Ref1"`
- `protocol` is how the data is transferred between the host and server (e.g. `https`)
- `host` is the domain name (e.g. `yoursite.com`)
- `path` is the page path on the website (e.g. `pricing/details`)
- `query` is the string of query parameters in the url (e.g. `myparam1=123`)
- `ref` is the reference or where the user came from before visiting the url (e.g. `#newsfeed`)

# Usage
To use the snippet, just copy out the RegEx for whatever part of the URL you need - or capture them all as in the example below: 

```sql
WITH data as 
    (SELECT 'https://www.yoursite.com/pricing/details?myparam1=123&myparam2=abc#newsfeed' AS my_url)

SELECT
  SUBSTRING(my_url, '^([a-zA-Z]+)://') AS protocol,
  SUBSTRING(my_url, '(?:[a-zA-Z]+:\/\/)?([a-zA-Z0-9.]+)\/?') AS host,
  SUBSTRING(my_url, '(?:[a-zA-Z]+://)?(?:[a-zA-Z0-9.]+)/{1}([a-zA-Z0-9./]+)') AS path,
  SUBSTRING(my_url, '\?(.*)') AS query,
  SUBSTRING(my_url, '#(.*)') AS ref
FROM data
```
| protocol | host | path | query | ref |
| --- | ------ | ----- | --- | ------|
| https | www.yoursite.com | pricing/details | myparam1=123&myparam2=abc#newsfeed | newsfeed |

# References Helpful Links
- [The Anatomy of a Full Path URL](https://zvelo.com/anatomy-of-full-path-url-hostname-protocol-path-more/)
