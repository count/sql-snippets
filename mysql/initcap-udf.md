# UDF for capitalizing initial letters

# Description

Most SQL dialects have a way to convert a string from e.g. "tHis STRING" to "This String" where each word starts with a capitalized letter. This is useful for transforming names of people or places (e.g. ‘North Carolina’ or ‘Juan Martin Del Potro’).

Per this [Stackoverflow post](https://stackoverflow.com/questions/12364086/how-can-i-achieve-initcap-functionality-in-mysql), this UDF will give us INITCAP functionality.

```sql
DELIMITER $$

DROP FUNCTION IF EXISTS `test`.`initcap`$$

CREATE FUNCTION `initcap`(x char(30)) RETURNS char(30) CHARSET utf8
READS SQL DATA
DETERMINISTIC
BEGIN
SET @str='';
SET @l_str='';
WHILE x REGEXP ' ' DO
SELECT SUBSTRING_INDEX(x, ' ', 1) INTO @l_str;
SELECT SUBSTRING(x, LOCATE(' ', x)+1) INTO x;
SELECT CONCAT(@str, ' ', CONCAT(UPPER(SUBSTRING(@l_str,1,1)),LOWER(SUBSTRING(@l_str,2)))) INTO @str;
END WHILE;
RETURN LTRIM(CONCAT(@str, ' ', CONCAT(UPPER(SUBSTRING(x,1,1)),LOWER(SUBSTRING(x,2)))));
END$$

DELIMITER ;
```

# Usage

Use like:

```sql
select initcap('This is a test string');
```

# References

- [Stackoverflow](https://stackoverflow.com/questions/12364086/how-can-i-achieve-initcap-functionality-in-mysql)
- [SQL snippets](https://sql-snippets.count.co/t/udf-initcap-in-mysql/207)