# tables/top_level_geographies

The `top_level_geographies` table returns information regarding the individual `top-level geographies` which are identified via an `id`.

The following `JOIN` queries can be carried out:

- `geography_type_id` on the [geography_groupings](geography_groupings.md) table using `geography_type_id`.
- `top_level_geography_id` on the [geography_areas](geography_areas.md) table using `id`.

## What are top-level geographies?

A `top-level geography` refers to the first (highest) geography level selectable. This is usually chosen at the start of a search by an end-user. Below is a table of all the available top level geographies.

|top_level_geography_id|description|
|-|-|
|1|United Kingdom|
|2|Great Britain|
|3|England and Wales|
|4|England|
|5|Northern Ireland|
|6|Scotland|
|7|Wales|


## Example use

Let's say you want to list out all of the `geography_areas` that have a `top_level_geography_id` of `6` (Scotland). YOu could perform the following join on the [geography_areas](geography_areas.md) table to get the data:

```sql
SELECT geography_grouping_id,
       c2011_meta.geography_areas.description,
       top_level_geography_id,
       c2011_meta.top_level_geographies.description AS top_level_description
  FROM c2011_meta.geography_areas
       LEFT JOIN c2011_meta.top_level_geographies 
       ON c2011_meta.geography_areas.top_level_geography_id = c2011_meta.top_level_geographies.id 
 WHERE c2011_meta.geography_areas.top_level_geography_id = 6;
```

Results:

|geography_grouping_id|description|top_level_geography_id|
|-|-|-|
|2003|Scotland|6|
|2006|Clackmannanshire|6|
|2006|Dumfries & Galloway|6|
|2006|East Ayrshire|6|
|2006|East Lothian|6|
|...|...|...|

## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key. This also acts as a foreign key for `geography_areas` and `geography_groupings`.|
|geography_type_id|int4|Foreign key for `geography_groupings`.|
|description|varchar(255)|User-readable name to describe a geography.|
|geography_code|varchar(50)|A unique identifier for a given geography.|
|hidden_from_ui|bool|Whether a geography should be visible to the end user.|


## Sample query

```sql
SELECT id, 
       geography_type_id, 
       description, 
       geography_code, 
       hidden_from_ui 
  FROM top_level_geographies;
```

Will return the following:

|id|geography_type_id|description|geography_code|hidden_from_ui|
|-|-|-|-|-|
|1|2,000|United Kingdom|K02000001|false
|2|2,001|Great Britain|K03000001|true
|3|2,002|England and Wales|K04000001|true
|4|2,003|England|E92000001|false
|5|2,003|Northern Ireland|N92000002|false
|6|2,003|Scotland|S92000003|false
|7|2,003|Wales|W92000004|false