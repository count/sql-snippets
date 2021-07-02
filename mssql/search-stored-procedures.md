# SEARCH FOR TEXT WITHIN STORED PROCEDURES

# Description
Often you'll want to know which stored procedure is performing an action or contains specific text. Query will match on comments.

# Snippet ✂️
```sql
SELECT name
FROM   sys.procedures
WHERE  Object_definition(object_id) LIKE '%strHell%'
```

# Usage

```sql
SELECT name
FROM   sys.procedures
WHERE  Object_definition(object_id) LIKE '%blitz%'
```

| name |
| -------- |
| sp_BlitzFirst| 
| sp_BlitzCache| 
| sp_BlitzWho| 
| sp_Blitz| 
| sp_BlitzBackups| 
| sp_BlitzLock| 
| sp_BlitzIndex| 

# Referenced 
[Stackoverflow](https://stackoverflow.com/questions/14704105/search-text-in-stored-procedure-in-sql-server)