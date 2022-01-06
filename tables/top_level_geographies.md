# tables/top_level_geographies

The `top_level_geographies` table returns information regarding the individual `top level geographies` which are identified via an `id`.

The following `JOIN` queries can be carried out:

- `geography_type_id` on the [geography_groupings](geography_groupings.md) table using `geography_type_id`.
- `top_level_geography_id` on the [geography_areas](geography_areas.md) table using `id`.

## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key|
|geography_type_id|int4|Foreign key for `geography_groupings`|
|description|varchar(255)|User-readable name to describe a geography.|
|geography_code|varchar(50)|A unique identifier for a given geography.|
|hidden_from_ui|bool|Whether a geography should be visible to the end user.|


## Sample query

```sql
SELECT id, geography_type_id, description, geography_code, hidden_from_ui FROM top_level_geographies
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