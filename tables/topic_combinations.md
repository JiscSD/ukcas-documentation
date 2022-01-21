# tables/topic_combinations
The `topic_combinations` table lists out the available `topic` combinations for each possible set of `geography_combinations`.

## What are topic_combinations?
A `topic_combination` is a selection of different `topics` sorted in an array. The data has been setup so that only certain combinations of `topics` can be selected depending on what [geography_combinations](geography_combinations.md)  have been chosen. This is to stop users manipulating data to identify individuals in the census data by being overly specific.

## Example use
Let's say that you have a topic within the [topics](topics.md) table and you want to check what topic combinations that topic fits into

```sql
SELECT id, 
       combination,
       geography_combinations
  FROM c2011_meta.topic_combinations
 WHERE ARRAY['AGE'] <@ ARRAY[combination]
```

|id|combination|geography_combinations|
|-|-|-|
|106|AGE,COBCON,ECOACT,INDUST,SEX,UNIT|{2003:5}|
|107|AGE,COBCON,ECOACT,OGRPMIN,SEX,UNIT|2003:5|
|108|AGE,COBCON,ECOACT,SEX,UNIT|2003:5|
|147|AGE,DAYPOP,ECOACT,NSSEC,UNIT|2002:3,2003:4,2003:5,2003:7,2004:4,2005:4,2006:4,2006:5,2006:7,2007:5,2008:4,2008:7,2009:5,2011:4,2013:4,2013:7|

With this information we can identify what `topic combinations` that `AGE` is contained within, as well as being able to confirm what `geography_combinations` these `topic_combinations` can be queried for.
This query is particularly useful after the user has selected their first (`primary`) topic in the UI as it allows us to display the available topic groups that the user can query are data on based on the initial selection.

## Schema

|column|type|use|
|-|-|-|
|id|int4|primary key|
|variable_combination_count|int4|The total number of possible combinations of the `topics` children `variables` (see notes)|
|ordinal|int4|An arbitrary numerical order for the values|
|combination|_text|An array of different `topics` which forms a topic combination|
|geography_combinations|_text|A list of all the geographical locations that are available for the selected `topic_combination`|
|geography_groupings|_int4|a list of all unique `geography_groupings` ids that are listed in `geography_combinations`|
|top_level_geographies|_int4|a list of all unique `top_level_geographies` ids that are listed in `geography_combinations`|
|title|varchar(255)|A text description of the selected combination|
|universe|varchar(255)|lists the values that are present in all of the combinations, and so can be ignored for filtering purposes.|
|units|_int4|units are `variables` that are guaranteed to be present as part of the selected topic combination.|


## Sample query

```sql
SELECT id, 
       variable_combination_count, 
       combination, 
       geography_combinations, 
       title 
  FROM c2011_meta.topic_combinations
```

This query will return the following table:

|id|variable_combination_count|combination|geography_combinations|title|
|-|-|-|-|-|
|106|513{AGE,COBCON,ECOACT,INDUST,SEX,UNIT}|{2003:5}|Country of birth (condensed for Northern Ireland) by Industry by Sex 2011|
|107|945|{AGE,COBCON,ECOACT,OGRPMIN,SEX,UNIT}|{2003:5}|Country of birth (condensed for Northern Ireland) by Economic activity by Occupation (minor groups) by Sex 2011|
|108|1890|{AGE,COBCON,ECOACT,SEX,UNIT}|{2003:5}|Age by Country of birth (condensed) by Economic activity by Sex 2011|
|147|97|{AGE,DAYPOP,ECOACT,NSSEC,UNIT}|{2002:3,2003:4,2003:5,2003:7,2004:4,2005:4,2006:4,2006:5,2006:7,2007:5,2008:4,2008:7,2009:5,2011:4,2013:4,2013:7}|NS-SeC (National Statistics Socio-economic Classification) (Workplace Population) 2011|
|99|994{AGE,COB,MNLANNI,UNIT}|{2003:5,2006:5}|Country of birth by Main language (Northern Ireland)|2011|
