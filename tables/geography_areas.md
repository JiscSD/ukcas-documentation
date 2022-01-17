# tables/geography_areas

The `geography_areas` table returns information regarding geographical locations that are available within the census data.

The following `JOIN` queries can be carried out:

- `id` on the [top_level_geographies](top_level_geographies.md) table using `top_level_geography_id`.
- `id` on the [geography_groupings](geography_groupings.md) table using `geography_grouping_id`.

## What are geography areas?

A geography area represents a specific region within the UK that can vary in size/breadth of coverage based on it's relation with the [top_level_geographies](top_level_geographies.md) table and the [geography_groupings](geography_groupings.md) table.

The relation to the [top_level_geographies](top_level_geographies.md) table defines the overarching region that the data is contained within. e.g. a `top_level_geography_id` = 7 means that the data is categorised as being from `Wales`. Below is a list of all the `top_level_geographies`.

|top_level_geography_id|description
|-|-|
|1|United Kingdom|
|2|Great Britain|
|3|England and Wales|
|4|England|
|5|Northern Ireland|
|6|Scotland|
|7|Wales|

The other table that this table relates to is [geography_groupings](geography_groupings.md). The geography_grouping defines how granular a particular area is based on one of 14 different classifications ranging from as broad as the entire UK (`geography_grouping_id` = 2000), all the way down to workplace zone layers (`geography_grouping_id` = 2013).See below for a list of the available geography_groupings.

|geography_grouping_id|abbreviation|name|
|-|-|-|
|2000|UK|United Kingdom|
|2001|GB|Great Britain|
|2002|EW|England and Wales|
|2003|CTRY|Countries and Groupings|
|2004|RGN|Regions|
|2005|CNTY|Counties|
|2006|LA|Local Authorities|
|2007|WED|Wards and Electoral Divisions|
|2008|MSOAIZ|Middle Super Output Areas and Intermediate Zones|
|2009|LSOADZ|Lower Super Output Areas and Data Zones|
|2010|OASA|Output Areas and Small Areas|
|2011|MLA|Merging Local Authorities|
|2012|MWED|Merging Wards and Electoral Divisions|
|2013|WZLYR|Workplace Zone Layer|

If you combine the two values then you get the resulting `geography_area` which is referenced in the tables [topic_combinations](topic_combinations.md) as `geography_combinations` and [variable_combinations](variable_combinations.md) as `geography_combination`.The data is stored with the format of: ${geography_grouping_id}:${top_level_geography_id} e.g. 2006:4 (which in this case represents the isle of Wight local authority). 
## Example use

Searching within a top-level geography of `Wales`, you would find the `geography_area` of `Gwynedd`, which can be seen through the following `JOIN` on `top_level_geographies`:

```sql
SELECT geography_areas.id,
       top_level_geographies.description,
       geography_areas.geography_grouping_id,
       geography_areas.description,
       geography_areas.geography_code
  FROM c2011_meta.geography_areas
       LEFT JOIN c2011_meta.top_level_geographies 
       ON c2011_meta.geography_areas.top_level_geography_id = top_level_geographies.id 
 WHERE geography_areas.id = 435;
```

|id|top_level_geography|geography_grouping_id|description|geography_code|
|-|-|-|-|-|
|435|Wales|2,006|Gwynedd|W06000002|

## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key.|
|top_level_geography_id|int4|Foreign key for [top_level_geographies](top_level_geographies.md) (see the `What are geography areas` section for how this relatest to external tables).|
|geography_grouping_id|int4|Relates to a geographical group in `geography_groupings`, eg. `Gwynedd` has a value of `2,006` which relates to the grouping of `Local Authorities`.|
|description|varchar(255)|An end-user friendly description for the geography.|
|geography_code|varchar(50)|foregin key for [geography_groupings](geography_groupings.md) (see the `What are geography areas` section for how this relatest to external tables)|

## Sample query

```sql
SELECT id, 
       top_level_geography_id, 
       geography_grouping_id, 
       description, 
       geography_code 
  FROM c2011_meta.geography_areas 
 WHERE id = 17;
```

Returns the following:

|id|top_level_geography_id|geography_grouping_id|description|geography_code|
|-|-|-|-|-|
|17|4|2,005|Buckinghamshire|E10000002|
