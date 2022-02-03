# tables/variable_combinations

The `variable_combinations` tables lists out the variable combinations available for the topic combinations, the `geography_combinations` they are available for, and most importantly gives the `table_name` and row `name` that the raw data is avaiable at. 

## What are variable_combinations?

A `variable_combination` consists of multiple unique `variables` from different parent `topics`. The id of a `variable_combination` is a unique topic/varriable combination, i.e "Ages 16 - 75" and "Full time employment". This variable combination can be used to find a corresponding `cellname` and `table` from the `data` schema.

## Example use

In order to query this though, you need the following information:
- what `topic_variable_combination` combination they are querying
- what `geography_combination` they are querying.

With this information you can get the `name` and `table_names` for a particular entry: 
```sql
SELECT name, 
       geography_combination, 
       combination, 
       table_name, 
       description  
  FROM c2011_meta.variable_combinations
 WHERE ARRAY['AGE:50','ECOACT:565','SEX:1933','UNIT:1962'] <@ ARRAY[topic_variable_combination]
```
|name|geography_combination|topic_variable_combination|table_name|description
|-|-|-|-|-|
|DC6107EW0610|2002:3,2003:4,2003:7,2004:4,2005:4,2006:4,2006:7,2007:4,2007:7,2008:4,2008:7,2011:4|AGE:50,ECOACT:565,SEX:1933,UNIT:1962|DC6107_0_EW_MRG_RCD_AGG|Age 65 and over // Economically active // Females // Persons|

Using these results we can then query the data table for the results using:

```sql
SELECT DC6107EW0610, 
       geocodeid 
  FROM c2011.DC6107_0_EW_MRG_RCD_AGG;
```
which results:
|dc6107ew0610|geocodeid|
|-|-|
|52|18333|
|32|18334|
|...|...|

We've now obtained results that represent `women`, `aged 65 and over`, `who are economically active`  for a range of different locations.

## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key|
|name|varchar(50)|`Cellname` of selected data, which corresponds to an entry in `table_name`.|
|combination|_int4|A list of the `ID` fields for all the corresponding `variables`.|
|topic_combination|_text|An array of IDs that list out all of the different `topics` that this `variable_combination` contain.|
|topic_variable_combination|_text|Lists out all of the entries `topics` with the corresponding child `variables`.|
|geography_combination|_text|Lists all of the relevant `geography combinations` for that `variable_combination`|
|geography_groupings|_text|Lists all of the `geography groupings` for that `variable_combination`|
|description|varchar|a description of what the selected `variable combination` represents|
|table_name|varchar(50)|the name of a table stored in the data schema of the database.|
|top_level_geography|_int4|A list of all the `top level geography` entries for a parrticular `variable_combination`|

## Sample query
See example use