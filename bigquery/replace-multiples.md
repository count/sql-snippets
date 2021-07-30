#  Replace or Strip out multiple different string matches from some text.

## Description

If you need to remove to replace multiple difference string conditions it can be quite code heavy unless you make use REGEXP_REPLACE
Use | to alternative matches
(?i) allows case of the matches to be ignored

```
select REGEXP_REPLACE('Some text1 or Text2 etc', r'(?i)text1|text2|text3|text4','NewValue')
```

## Example

```sql
with pet as
(select ['Marley (hcp)','Marley (HCP)','Marley(hcp)','Marley (hcP)','Marley (CC)','Marley (cc)','Marley(cc)','Marley (rehomed)','Marley (re-homed)','Marley (stray)','Marley**','Marley        **'] as pets
), pet_list as
(
select pet_name
  from pet
  left join unnest(pets) pet_name)
select
  pet_name, 
  TRIM(REGEXP_REPLACE(pet_name, r'(?i)\(hcp\)|\(cc\)|\(re-homed\)|\(foster\)|\(stray\)|\(rehomed\)|\*\*','')) as clean_pet_name
from pet_list
```

```
+-----------------+--------------+
|pet_name         |clean_pet_name|
+-----------------+--------------+
|Marley (hcp)     |Marley        |
|Marley (HCP)     |Marley        |
|Marley(hcp)      |Marley        |
|Marley (hcP)     |Marley        |
|Marley (CC)      |Marley        |
|Marley (cc)      |Marley        |
|Marley(cc)       |Marley        |
|Marley (rehomed) |Marley        |
|Marley (re-homed)|Marley        |
|Marley (stray)   |Marley        |
|Marley**         |Marley        |
|Marley        ** |Marley        |
+-----------------+--------------+
```
