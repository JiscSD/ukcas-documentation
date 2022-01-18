# tables/topics


## What are topics?
The `topics` table details different topics that can be selected and when combined with subsequent `variables` allow users to filter down the data. Some examples of `topics` are: `Age`, `Country of Birth`, `Economic Activity`. 

Each topic then has a number of children variables that are stored in the [variables](variables.md) table in a one to many relation.
e.g. the Topic `AGE` has a `variable` of `Age 0 - 4`, `Age 5 to 7` etc

## Example use
The following are some examples of JOIN queries

topic_id on the [variables](variables.md) table using id.

```sql
SELECT c2011_meta.topics.id, 
       c2011_meta.topics.abbreviation, 
       c2011_meta.topics.name, 
       c2011_meta.variables.description 
  FROM c2011_meta.topics 
       LEFT JOIN c2011_meta.variables
       ON c2011_meta.variables.topic_id = c2011_meta.topics.id 
 WHERE c2011_meta.topics.ID = 3 
 LIMIT 5;
```

|id|abbreviation|name|description|
|-|-|-|-|
|3|AGE|Age|Total: Age|
|3|AGE|Age|Age 0 to 4|
|3|AGE|Age|Age 5 to 7|
|3|AGE|Age|Age 8 to 9|
|3|AGE|Age|Age 10 to 14|
## Schema

|column|type|use|
|-|-|-|
|id|int4|primary key, and a foreign key used by `variables` table|
|abbreviation|varchar(255)|The shorthand way of referencing the topics `name`|
|name|varchar(255)|the full length name for the selected topic|
|description|text|A full description describing what the topic represents|
|ordinal|int4|A value that represents an arbitrary numerical order for the data|
|top_level_geography_coverage|_int4|an array of what geographies from the `top_level_geographies` table these topics are available for|


## Sample query

```sql
SELECT id, 
       abbreviation, 
       name, 
       description, 
       ordinal, 
       top_level_geography_coverage 
  FROM c2011_meta.topics
 WHERE id = 3;
```

This query will return the following table.

|id|abbreviation|name|description|ordinal|top_level_geography_coverage|
|-|-|-|-|-|-|
|3|AGE|Age|Age is derived from the date of birth question and is a person's age at their last birthday, at 27 March 2011. Dates of birth that imply an age over 115 are treated as invalid and the person's age is imputed. Infants less than one year old are classified as 0 years of age.|6|{1}|