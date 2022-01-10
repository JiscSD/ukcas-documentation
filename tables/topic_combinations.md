# tables/topic_combinations


## What is topic_combinations?
The `topic_combinations` table lists out the available `topic` combinations for each possible set of `geography_combinations`.

Once a user has selected what locations to filter by, they will then be prompted to select topics alongside corresponding filters. However the data has been setup so that only certain combinations of `variables` can be selected depending on what `geography_combinations` have been chosen. This is to stop users manipulating data to identify individuals using the data.

## Example use
Let's say that you have a topic within the `topics` table and you want to check what topic combinations that topic fits into

```sql
select ID, COMBINATION
from topic_combinations
where ARRAY['AGE'] <@ ARRAY[combination]
```

### Results
|id|combination|geography_conbinations|
|-|-|-|
|106|AGE,COBCON,ECOACT,INDUST,SEX,UNIT|{2003:5}|
|107|AGE,COBCON,ECOACT,OGRPMIN,SEX,UNIT|2003:5|
|108|AGE,COBCON,ECOACT,SEX,UNIT|2003:5|
|147|AGE,DAYPOP,ECOACT,NSSEC,UNIT|2002:3,2003:4,2003:5,2003:7,2004:4,2005:4,2006:4,2006:5,2006:7,2007:5,2008:4,2008:7,2009:5,2011:4,2013:4,2013:7|

With this information we can identify what `topic combinations` that `AGE` is contained within, as well as being able to confirm what `geography_combinations` these `topic_combination` can be queried for.
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
SELECT ID, COMBINATION, GEOGRAPHY_COMBINATIONS, TOP_LEVEL_GEOGRAPHY, TITLE FROM topics WHERE ID 3;
```

This query will return the following table.

|id|abbreviation|name|description|ordinal|top_level_geography|
|-|-|-|-|-|-|
|3|AGE|Age|Age is derived from the date of birth question and is a person's age at their last birthday, at 27 March 2011. Dates of birth that imply an age over 115 are treated as invalid and the person's age is imputed. Infants less than one year old are classified as 0 years of age.|6|{1}|

