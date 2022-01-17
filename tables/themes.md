
# tables/themes

Note: Currently the `themes` table is a WIP so this information will need to be updated when the table is updated.


## What are themes?
The `themes` table exists to help categorise topics into different parent themes that are represented with an id. This will help end users find common topics that they are interested in querying. It is still a WIP but eventually there will be something like `theme_id` in the `topics` table that will relate the two in a one to many relationship.

## Example use
The following `JOIN` queries could be carried out if the [topics](topics.md) table had a `theme_id` column:

`theme_id` on the [topics](topics.md) table using `id`.

The below query would in theory, display all of the topics that are part of the parent theme `People`.
```sql
SELECT theme,
       theme_id,
       c2011_meta.themes.description,
       name
       c2011_meta.topics.description AS topic_description
  FROM c2011_meta.themes
       LEFT JOIN c2011_meta.topics 
       ON c2011_meta.topics.theme_id = themes.id 
 WHERE themes.id = 1;
 ```

|theme|theme_id|description|name|topic_description|
|-|-|-|-|-|
|people|1|This Theme contains topics which describe people|Age|Age is derived from the date of birth question and is a person's age at their last birthday, at 27 March 2011. Dates of birth that imply an age over 115 are treated as invalid and the person's age is imputed. Infants less than one year old are classified as 0 years of age.|
|...|...|...|...|...|

## Schema

|column|type|use|
|-|-|-|
|id|int4|primary key|
|theme|varchar(255)|A short description outlying what topics a user could expect to find related to this theme|
|description|varchar(255)|A longer description of the selected theme|

## Sample query

```sql
SELECT id, 
       theme, 
       description 
  FROM THEMES;
```

Below is some of the data that this query will return.

|ID|theme|description|
|-|-|-|
|1|People|This theme contains topics which describe people|
|2|Demographic and social information about everybody|Theme description needs adding|
