#  Concatinate text handling nulls

## Description

If you concatenate values together and one is null then the while result is set to null

```sql
SELECT CONCAT('word', ',', 'word2', ',', 'word3'), CONCAT('word', ',', NULL, ',', 'word3');
```
```
+----------------+----+
|f0_             |f1_ |
+----------------+----+
|word,word2,word3|NULL|
+----------------+----+
```

If you want to ignore nulls you can use arrays and ARRAY_TO_STRING

```
SELECT ARRAY_TO_STRING(['word', 'word2', 'word3'], ','), ARRAY_TO_STRING(['word', NULL, 'word3'], ',');
```
```
+----------------+----------+
|f0_             |f1_       |
+----------------+----------+
|word,word2,word3|word,word3|
+----------------+----------+
```
