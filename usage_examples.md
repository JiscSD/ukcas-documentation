## How many people are in full-time employment in Wales?

First a new user decides what geography they want to query

```sql
SELECT * 
  FROM c2011_meta.top_level_geographies;
```
|geography_type_id|id|description|geography_code|hidden_from_ui|
|-|-|-|-|-|
|2000|1|United Kingdom|K02000001|false|
|2001|2|Great Britain|K03000001|true|
|2002|3|England and Wales|K04000001|true|
|2003|4|England|E92000001|false|
|2003|5|Northern Ireland|N92000002|false|
|2003|6|Scotland|S92000003|false|
|2003|7|Wales|W92000004|false|

The user now selects the entirety of Wales from the above selection of top-level geographies. We then query the `geography_areas` table for the corresponding area data.

```sql
SELECT * 
  FROM c2011_meta.geography_areas
 WHERE c2011_meta.geography_areas.geography_grouping_id = 2003
   AND c2011_meta.geography_areas.top_level_geography_id = 7;
```

|geography_grouping_id|id|description|geography_code|top_level_geography_id|
|-|-|-|-|-|
|2003|7|Wales|W92000004|7|


Now that we have the `geography_grouping_id` and the `top_level_geography_id` for the users location selection: We now get the user to pick what topics they want to search on by listing out the available topics that match the chosen geography.


```sql
SELECT combination 
  FROM c2011_meta.topic_combinations
 WHERE ARRAY['2003:7'] <@ ARRAY[c2011_meta.topic_combinations.geography_combinations];

```
The above query provides us with every topic combination for the selected geography. Below is a small sample:
|combination|
|-|
|{AGE,DAYPOP,ECOACT,NSSEC,UNIT}|
|{AGE,DAYPOP,ECOACT,OGRPMIN,UNIT}|
|{AGE,DAYPOP,ECOACT,PPHALL,UNIT}|
|{AGE,DAYPOP,ECOACT,RELIG,UNIT}|
|{AGE,DAYPOP,ECOACT,TENURE,UNIT,URESPOP}|
|...

We now have a list of all the possible topic combinations for Wales which we display to the user. The user wants to know how many people are in full-time employment for the selected gography. So for this instance they select: `economic activity` (ECOACT).

Now we need to query the table for only topic combinations that contain `economic activity` and match out geography selection.
```sql
SELECT combination 
  FROM c2011_meta.topic_combinations
 WHERE ARRAY['2003:7'] <@ ARRAY[c2011_meta.topic_combinations.geography_combinations]
   AND ARRAY['ECOACT'] <@ ARRAY[c2011_meta.topic_combinations.combination];

``` 
Which produces the following results (we're just showing the first 5 results in this case).

|combination|
|-|
|{AGE,DAYPOP,ECOACT,NSSEC,UNIT}|
|{AGE,DAYPOP,ECOACT,OGRPMIN,UNIT}|
|{AGE,DAYPOP,ECOACT,PPHALL,UNIT}|
|{AGE,DAYPOP,ECOACT,RELIG,UNIT}|
|{AGE,DAYPOP,ECOACT,TENURE,UNIT,URESPOP}|
|...|

Now user selects the `topic` group containing `economic activity` that they desire (we've selected the simplest combination). This means that the results will be grouped by `age` and `economic activity`

`AGE,ECOACT,UNIT`

We now query `variable_combinations` for the relevant `cellnames` and `tablenames` that relate to the data.

```sql
SELECT table_column_name,
       topic_variable_combination, 
       description, 
       table_name
  FROM c2011_meta.variable_combinations
 WHERE ARRAY['AGE','ECOACT','UNIT'] = c2011_meta.variable_combinations.topic_combination;
```

|name|topic_variable_combination|description|table_name|
|-|-|-|-|
|QS601SC0010|{AGE:46,ECOACT:570,UNIT:1962}|Age 16 to 74 // Economically active\ Full-time students // Persons|QS601_0_SC_MRG_RCD_AGG|
|QS601SC0013|{AGE:46,ECOACT:3429,UNIT:1962}|Age 16 to 74 // Persons // Economically inactive\ Student|QS601_0_SC_MRG_RCD_AGG|
|QS601UK0003|{AGE:46,ECOACT:3823,UNIT:1962}|Age 16 to 74 // Persons // Economically active\ Employee\ Part-time (excluding full-time students)|QS601_0_UK_MRG_RCD_AGG|
|QS601UK0004|{AGE:46,ECOACT:3795,UNIT:1962}|Age 16 to 74 // Persons // Economically active\ Employee\ Full-time (excluding full-time students)|QS601_0_UK_MRG_RCD_AGG|


To get the number of citizens each `name` and `table_name` this corresponds to we can do a query like so:

```sql
SELECT geocodeid, 
       QS601SC0010 
  FROM c2011.QS601_0_SC_MRG_RCD_AGG;
```

|geocodeid|QS601SC0010|
|-|-|
|354549|78|
|354550|90|
|354551|81|
|354552|63|
|354553|120|

The `geocodeid` refers back to the `geography_area_relations` `child_id` column.
while the cellname column represents the number of people, for that geography, that match our selection of `topics` and `variables`.