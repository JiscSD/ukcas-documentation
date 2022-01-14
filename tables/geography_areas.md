# tables/geography_areas

The `geography_areas` table returns information regarding geographies within `top_level_geographies` and are identified via an `id`.

The following `JOIN` queries can be carried out:

- `id` on the [top_level_geographies](top_level_geographies.md) table using `top_level_geography_id`.
- `id` on the [geography_grouping](geography_grouping.md) table using `geography_grouping_id`.

## What are geography areas?

A geography area relates to geography within `top_level_geographies`, for instance the geography area of `Cumbria` is located within the top-level geography of `England`.

## Example use

Searching within a top-level geography of `Wales`, you would find the geography area of `Gwynedd`, which can be seen through the following `JOIN` on `top_level_geographies`:

```sql
SELECT ga.id, tlg.description AS top_level_geography, ga.geography_grouping_id, ga.description, ga.geography_code 
  FROM geography_areas ga 
       LEFT JOIN top_level_geographies tlg 
       ON ga.top_level_geography_id = tlg.id 
       WHERE ga.id = 435;
```

|id|top_level_geography|geography_grouping_id|description|geography_code|
|-|-|-|-|-|
|435|Wales|2,006|Gwynedd|W06000002|

From this you could then continue to search for the number of people by `AGE` and `HIQUAL` (highest level of qualification) within `Gwynedd`.

## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key.|
|top_level_geography_id|int4|Foreign key for `top_level_geographies`.|
|geography_grouping_id|int4|Relates to a geographical group in `geography_groupings`, eg. `Gwynedd` has a value of `2,006` which relates to the grouping of `Local Authorities`.|
|description|varchar(255)|An end-user friendly description for the geography.|
|geography_code|varchar(50)|A unique identifier for a geography.|

## Sample query

```sql
SELECT id, top_level_geography_id, geography_grouping_id, description, geography_code 
  FROM geography_areas 
 WHERE id = 17;
```

Returns the following:

|id|top_level_geography_id|geography_grouping_id|description|geography_code|
|-|-|-|-|-|
|17|4|2,005|Buckinghamshire|E10000002|
