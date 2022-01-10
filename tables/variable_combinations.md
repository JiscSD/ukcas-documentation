# tables/variable_combinations

the `variable_combinations` table lists out for a given variable combination what topics they belong to, what `geography_combinations` these are available for, and most importantly gives the `tablename` and row `name` that the raw data is avaiable at. 
## What are variable_combinations?

`variable_combinations` is the table that brings together the data in the other tables that can then be used to reference the actual raw data in the other schema. the `table_name` and `name` fields are used to achieve this. in order to query this though one has to know the following information:
- what `topic_variable_combination` combination they are querying
- what `geography_combination` they are querying
With this information you can get the `name` and `table_names` for a particular entry.
e.g. 
```sql
SELECT name, geography_combination, combination, table_name FROM c2011_meta.variable_combinations
WHERE ARRAY['AGE:50','ECOACT:565','SEX:1933','UNIT:1962'] <@ ARRAY[topic_variable_combination]
```
output:
|name|geography_combination|topic_variable_combination|table_name|
|-|-|-|-|
|DC6107EW0610|2002:3,2003:4,2003:7,2004:4,2005:4,2006:4,2006:7,2007:4,2007:7,2008:4,2008:7,2011:4|AGE:50,ECOACT:565,SEX:1933,UNIT:1962|DC6107_0_EW_MRG_RCD_AGG|

using these results we can then query the data table for the results using:

```sql
select DC6107EW0610, geocodeid from c2011.DC6107_0_EW_MRG_RCD_AGG;
```
which results:
|dc6107ew0610|geocodeid|
|-|-|
|52|18333|
|32|18334|
|...|...

## Example use


## Schema

## Sample query