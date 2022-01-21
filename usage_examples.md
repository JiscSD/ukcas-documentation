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

The user now selects the `top level geography` Wales from the above selection. We now get the user to select how granular they want the data to be by having them select a `geography_area`. We then query the `geography_areas` table for all the possible results and present them to the end user.

```sql
SELECT id,
       name,
       description,
       geography_area_count,
       top_level_geography_id
  FROM c2011_meta.geography_groupings
 WHERE ARRAY[top_level_geography_id] @> ARRAY[6];
```
|id|name|description|geography_area_count|top_level_geography_id|
|-|-|-|-|-|
|2003|Countries and Groupings|England, Wales, Scotland and Northern Ireland.|4|{4,5,6,7}|
|2006|Local Authorities|Metropolitan Districts, Non-metropolitan District, London Boroughs and Unitary Authorities in England; Local Government Districts in Northern Ireland; Council Areas in Scotland; Unitary Authorities in Wales|404|{4,5,6,7}|
|2007|Wards and Electoral Divisions|	Census Wards in England; Census Electoral Divisions in England;  Census Merged Wards in England; Census Wards in Northern Ireland; Census Wards in Scotland; Census Electoral Divisions in Wales; Census Merged Wards in Wales|9481|{4,5,6,7}|
|2008|Middle Super Output Areas and Intermediate Zones|Middle Super Output Area Set in England; Census Intermediate areas in Scotland; Middle Super Output Area Set in Wales|8436|{4,6,7}|
|2009|Lower Super Output Areas and Data Zones|Lower Super Output Areas in England; Super Output Areas in Northern Ireland; Census Datazones in Scotland; Lower Super Output Areas in Wales|42143|{4,5,6,7}|
|2010|Output Areas and Small Areas|Output Areas in England; Output Areas in Scotland; Output Areas in Wales; Small Areas in Northern Ireland|232296|{4,5,6,7}|

From this output the end user then selects the id `2003`. We can now query the `geography_areas` table for a complete list of possible geography selections
```sql
SELECT * 
  FROM c2011_meta.geography_areas
 WHERE c2011_meta.geography_areas.geography_grouping_id = 2003
   AND c2011_meta.geography_areas.top_level_geography_id = 7;
```

|geography_grouping_id|id|description|geography_code|top_level_geography_id|
|-|-|-|-|-|
|2003|7|Wales|W92000004|7|

In this case the only possible selection is `Wales` and so the end user selects this geography for their query. Now that we have a selection we take the `geography_grouping_id` and the `top_level_geography_id` for the users geography selection. We now get the user to pick what topics they want to search on by listing out the available topics that match the chosen geography. The topics are grouped together in the database in `topic_combinations` so users will usually have to select more than one topic in order to query the data.


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

We now have a list of all the possible `topic_combinations` for `Wales` which we display to the user. The user wants to know how many people are in full-time employment for the selected gography. So they select: `economic activity` (ECOACT).

Now we need to query the table for only `topic_combinations` that contain `economic activity` and match the users chosen geography_area.

```sql
SELECT combination,
       id
  FROM c2011_meta.topic_combinations
 WHERE ARRAY['2003:7'] <@ ARRAY[c2011_meta.topic_combinations.geography_combinations]
   AND ARRAY['ECOACT'] <@ ARRAY[c2011_meta.topic_combinations.combination];
```

Which produces the following results (we're just showing the first 5 results).

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

Note that `UNIT` is a universal topic that is contained in every topic combination.

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
|...|...|...|...|

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

The `geocodeid` refers back to the `geography_areas` `id` column.
while the cellname column represents the number of people, for that geography, that match our selection of `topics` and `variables`.

We can go on to query the `geography_areas` table to identify the names of the found `geography_areas`

```sql
SELECT id, 
       description 
  FROM c2011_meta.geography_areas
 WHERE ARRAY[354549, 354550, 354551, 354552, 354553] @> ARRAY[id]
```

which returns:

|id|description|
|-|-|
|354549|Merryton and Meadowhill|
|354550|Larkhall Central, Raploch, Millheugh and Burnhead|
|354551|Hareleeshill|
|354552|Strutherhill|
|354553|Stonehouse|

combined with the previous table showing the variable_combination we can now present the end result:

|id|geography_description|variable_combination|population
|-|-|-|-|
|354549|Merryton and Meadowhill|Age 16 to 74 // Economically active\ Full-time students // Persons|78|
|354550|Larkhall Central, Raploch, Millheugh and Burnhead|Age 16 to 74 // Economically active\ Full-time students // Persons|90|
|354551|Hareleeshill|Age 16 to 74 // Economically active\ Full-time students // Persons|81|
|354552|Strutherhill|Age 16 to 74 // Economically active\ Full-time students // Persons|63|
|354553|Stonehouse|Age 16 to 74 // Economically active\ Full-time students // Persons|120|