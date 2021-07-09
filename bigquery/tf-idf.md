# TF-IDF
To explore an interactive version of this snippet, go [here](https://count.co/n/0NnhvqUjFuZ?vm=e).

# Description
TF-IDF (Term Frequency - Inverse Document Frequency) is a way of extracting keywords that are used more in certain documents than in the corpus of documents as a whole. 
It's a powerful NLP tool and is surprisingly easy to do in SQL. 
This snippet comes from Felipe Hoffa on [stackoverflow](https://stackoverflow.com/questions/47028576/how-can-i-compute-tf-idf-with-sql-bigquery):

```sql
#standardSQL
WITH words_by_post AS (
  SELECT CONCAT(link_id, '/', id) id, REGEXP_EXTRACT_ALL(
    REGEXP_REPLACE(REGEXP_REPLACE(LOWER(body), '&amp;', '&'), r'&[a-z]{2,4};', '*')
      , r'[a-z]{2,20}\'?[a-z]+') words
  , COUNT(*) OVER() docs_n
  FROM `fh-bigquery.reddit_comments.2017_07`  
  WHERE body NOT IN ('[deleted]', '[removed]')
  AND subreddit = 'movies'
  AND score > 100
), words_tf AS (
  SELECT id, word, COUNT(*) / ARRAY_LENGTH(ANY_VALUE(words)) tf, ARRAY_LENGTH(ANY_VALUE(words)) words_in_doc
    , ANY_VALUE(docs_n) docs_n
  FROM words_by_post, UNNEST(words) word
  GROUP BY id, word
  HAVING words_in_doc>30
), docs_idf AS (
  SELECT tf.id, word, tf.tf, ARRAY_LENGTH(tfs) docs_with_word, LOG(docs_n/ARRAY_LENGTH(tfs)) idf
  FROM (
    SELECT word, ARRAY_AGG(STRUCT(tf, id, words_in_doc)) tfs, ANY_VALUE(docs_n) docs_n
    FROM words_tf
    GROUP BY 1
  ), UNNEST(tfs) tf
)    


SELECT *, tf*idf tfidf
FROM docs_idf
WHERE docs_with_word > 1
ORDER BY tfidf DESC
LIMIT 1000
```


# Usage

```sql
-- ex_data
select 1 id, 'England will win it' body union all 
select 2 id, 'France will win it' body union all 
select 3 id, 'Its coming home' body 
```
|id | body |
|--| ---- |
|1 | 'England will win it' |
|2 | 'France will win it' |
|3 | 'Its coming home'|
```sql
-- words_by_post
SELECT 
  id, REGEXP_EXTRACT_ALL(
    REGEXP_REPLACE(REGEXP_REPLACE(LOWER(body), '&amp;', '&'), r'&[a-z]{2,4};', '*')
      , r'[a-z]{2,20}\'?[a-z]+') words
  , COUNT(*) OVER() docs_n
 FROM ex_data  
```
|id | body |
|--| ---- |
|1 | ['england','will','win'] |
|2 | ['france','will','win'] |
|3 | ['its','coming','home']|
```sql
-- words_tf
SELECT id, word, COUNT(*) / ARRAY_LENGTH(ANY_VALUE(words)) tf, ARRAY_LENGTH(ANY_VALUE(words)) words_in_doc
    , ANY_VALUE(docs_n) docs_n
FROM words_by_post, UNNEST(words) word
GROUP BY id, word
```
|id | word | tf | words_in_doc| docs_n|
|--| ---- | ---- | ---- | ---- |
|1 | 'england'|0.33|3 |3|
|1 | 'will' |0.33|3 |3|
|1| 'win'|0.33|3 |3|
|...| ...|...|... |...|

```sql
-- docs_idf
SELECT tf.id, word, tf.tf, ARRAY_LENGTH(tfs) docs_with_word, LOG(docs_n/ARRAY_LENGTH(tfs)) idf
  FROM (
    SELECT word, ARRAY_AGG(STRUCT(tf, id, words_in_doc)) tfs, ANY_VALUE(docs_n) docs_n
    FROM words_tf
    GROUP BY 1
  ), UNNEST(tfs) tf
```
|id | word | tf | docs_with_word| idf|
|--| ---- | ---- | ---- | ---- |
|1 | 'england'|0.33|1 |1.099|
|1 | 'will' |0.33|2 |.405|
|1| 'win'|0.33|2 |.405|
|...| ...|...|... |...|
```sql
-- tf_idf
SELECT *, tf*idf tfidf
FROM docs_idf
WHERE docs_with_word > 1
ORDER BY tfidf DESC
```
|id | word | tf | docs_with_word| idf|tfidf|
|--| ---- | ---- | ---- | ---- |---- |
|1 | 'england'|0.33|1 |1.099|.135|
|1 | 'will' |0.33|2 |.405|.135|
|1| 'win'|0.33|2 |.405|.135|
|...| ...|...|... |...|...|
