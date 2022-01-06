# topics


## What are Topics?
The `topics` table details the different topics that are available for querying the data on. each topic then has a number of children variables that are referenced in the [variables](variables.md) table in a one to many relation.

## Example use
The following are some examples of JOIN queries
topic_id on the [variables](variables.md) table using id.

## Schema

|column|type|use|
|-|-|-|
|id|int4|primary key, and a foreign key used by `variables` table|
|abbreviation|varchar(255)|The shorthand way of referencing the topics `name`|
|name|varchar(255)|the full length name for the selected topic|
|description|text|A full description describing what the topic represents|
|ordinal|int4|A value that represents an arbitrary numerical order for the data|
|top level geography|_int4|an array of what geographies from the `top_level_geographies` table these topics are available for|


## Sample query

```sql
SELECT ID, ABBREVIATION, NAME, DESCRIPTION, ORDINAL, TOP_LEVEL_GEOGRAPHY FROM topics WHERE ID 3;
```

This query will return the following table.

|id|abbreviation|name|description|ordinal|top_level_geography|
|-|-|-|-|-|-|
|3|AGE|Age|Age is derived from the date of birth question and is a person's age at their last birthday, at 27 March 2011. Dates of birth that imply an age over 115 are treated as invalid and the person's age is imputed. Infants less than one year old are classified as 0 years of age.|6|{1}|

