# Split a string on Uppercase Characters
View interactive version [here](https://count.co/n/Exa1qpLjE8y?vm=e).

# Description
Split a string like 'HelloWorld' into ['Hello','World'] using:

```sql
regexp_extract_all('stringToReplace',r'([A-Z][a-z]*)')
```

# Example:
```sql
select 
regexp_extract_all('HelloWorld',r'([A-Z][a-z]*)') as_array,
array_to_string(regexp_extract_all('HelloWorld',r'([A-Z][a-z]*)'),' ') a_string_again
```
|as_array | a_string_again|
|------- | --------- |
|["Hello","World"] | Hello World |
