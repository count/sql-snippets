# Parsing JSON Strings


# Description
Snowflake has a few helpful ways to deal w/ JSON objects. This snippet goes through a few ways to access elements in a JSON object: 

```sql
SELECT 
   <json_column>:<level_1_element>.<level_2_element>.<level_3_element> -- select json element,
   <json_column>:"<level_1_element>"."<level_2_element>", -- select json element if element name has special characters or spaces
   <json_column>:<level_1_element>::<data_type>, -- cast return object to explicit type
   <json_column>:<level_1_array>[array_index], -- access one instance of array
   get(<json_column>:<level_1>,array_size(<json_column>:<level_1>)-1) -- get last element in array
FROM 
<TABLE>
```
where: 
- `<json_column>` is the column in `<TABLE>` with the JSON object
- `<level_n_element>` is the level of nesting in the JSON object
- `<TABLE>` is the table w/ the JSON data. 
# Example: 

```sql
with car_sales
as(
select parse_json(column1) as src
from values
('{
    "date" : "2017-04-28",
    "dealership" : "Valley View Auto Sales",
    "salesperson" : {
      "id": "55",
      "name": "Frank Beasley"
    },
    "customer" : [
      {"name": "Joyce Ridgely", "phone": "16504378889", "address": "San Francisco, CA"}
    ],
    "vehicle" : [
      {"make": "Honda", "model": "Civic", "year": "2017", "price": "20275", "extras":["ext warranty", "paint protection"]}
    ],
    "values":[1,2,3]
}'),
('{
    "date" : "2017-04-28",
    "dealership" : "Tindel Toyota",
    "salesperson" : {
      "id": "274",
      "name": "Greg Northrup"
    },
    "customer" : [
      {"name": "Bradley Greenbloom", "phone": "12127593751", "address": "New York, NY"}
    ],
    "vehicle" : [
      {"make": "Toyota", "model": "Camry", "year": "2017", "price": "23500", "extras":["ext warranty", "rust proofing", "fabric protection"]}
    ],
    "values":[3,2,1]
}') v)

select 
  src:date::date as date , -- select json object
  src:"customer", -- for objects w/ incompatible names (e.g. spaces and special characters) escape w/ quotes
  src:vehicle[0].make::string as make, -- select first item in list
  get(src:values,array_size(src:values)-1) last_element -- last value in list 

from car_sales;
```
|DATE|SRC:"CUSTOMER"|MAKE|LAST_ELEMENT|
|----|------------|----|------|
|28/04/2017|[{"address":"San Francisco, CA","name":"Joyce Ridgely","phone":"16504378889"}]|HONDA|3|
|28/04/2017"|[{"address":"New York, NY","name":"Bradley Greenbloom","phone":"12127593751"}]|TOYOTA|1|
