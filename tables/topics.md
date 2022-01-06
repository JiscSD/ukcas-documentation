# topics

The `topics` table details the different topics that are available for querying the data on. each topic then has a number of children variables that are referenced in the `variables` table (link) in a one to many relation.

The columns that make up the table consist of:

- `ID` The foreign key that relates the `topics` table to the `variables` table with a one to many relationship
- `abbreviation` The shorthand way of referencing the topics `name`
- `name` the full length name for the selected topic
- `description` A full description describing what the topic represents
- `ordinal` A value that represents an arbitrary numerical order for the data
- `top level geography` an array of what geographies from the `top_level_geographies` table these topics are available for

```sql
SELECT ID, ABBREVIATION, NAME, DESCRIPTION, ORDINAL, TOP_LEVEL_GEOGRAPHY FROM topics WHERE ID 3;
```

This query will return the following table.

|id|abbreviation|name|description|ordinal|top_level_geography|
|-|-|-|-|-|-|
|3|AGE|Age|Age is derived from the date of birth question and is a person's age at their last birthday, at 27 March 2011. Dates of birth that imply an age over 115 are treated as invalid and the person's age is imputed. Infants less than one year old are classified as 0 years of age.|6|{1}|

