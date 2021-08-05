# Check if a column is accessible

Explore this snippet [here](https://count.co/n/xpgYlM1uDAT?vm=e).

# Description
One method to check if the current session has access to a column is to use the [`COL_LENGTH`](https://docs.microsoft.com/en-us/sql/t-sql/functions/col-length-transact-sql) function, which returns the length of a column in bytes.
If the column doesn't exist (or cannot be accessed), COL_LENGTH returns null.

```sql
select
  col_length('Orders', 'Sales') as does_exist,
  col_length('Orders', 'Shmales') as does_not_exist
```

| does_exist | does_not_exist |
| ---------- | -------------- |
| 8          | NULL           |