# tables/geography_groupings

The `geography_groupings` table returns grouping categories for `geography_areas` which are identified via an `id`.

The following `JOIN` queries can be carried out:

- `geography_grouping_id` on the [geography_areas](geography_areas.md) table using `id`.

## What are geography groupings?

A `geography_groupings` refer to named collection/groupings of `geography_areas`, so if we look at the following query:

```sql
select ga.id, ga.description, ga.geography_code, ga.geography_grouping_id, gg.name as geography_grouping_name from geography_areas ga left join geography_groupings gg on gg.id = ga.geography_grouping_id where ga.id = 65;
```

We can see that `Down` has a geographic grouping of `Local Authorities`:

|id|description|geography_code|geography_grouping_id|geography_grouping_name|
|-|-|-|-|-|
|65|Down|95NN|2,006|Local Authorities|

## Example use

You would like to retrieve the number of Local Authorities (`LA`) grouped by `top_level_geographies` along with the top-level `geography_code`:

```sql
select tlg.geography_code, tlg.description, count(ga.id) as local_authorities
from geography_areas ga
left join geography_groupings gg on ga.geography_grouping_id = gg.id
left join top_level_geographies tlg on ga.top_level_geography_id = tlg.id
where gg.abbreviation = 'LA' group by tlg.description, tlg.geography_code;

```

This results in the following:

|geography_code|description|local_authorities|
|--------------|-----------|-----------------|
|E92000001|England|324|
|N92000002|Northern Ireland|26|
|S92000003|Scotland|32|
|W92000004|Wales|22|


## Schema

|column|type|use|
|-|-|-|
|id|int4|Primary key.|
|abbreviation|varchar(100)|Short code/abbreviation for a grouping, such as `CNTY` for `County`.|
|name|varchar(100)|An end-user friendly name for a grouping, such as `Wards and Electoral Divisions`.|
|description|text|An end-user friendly description for a grouping.|
|top_level_geography_id|int4|The top-level geography for a given grouping, for example `Regions` has a top-level geography of `England` as the concept of a region exists in England only.|
|geography_area_count|int4|The number of `geography_areas` within the grouping.|
|description_extended|text|An extended end-user friendly description for a grouping.|

## Sample query

```sql
select id, abbreviation, name, description, top_level_geography_id, geography_area_count from geography_groupings;
```

Will return the following (**Note**: This excludes `description_extended` for ease of viewing):

|id|abbreviation|name|description|top_level_geography_id|geography_area_count|
|--|------------|----|-----------|----------------------|--------------------|
|2,000|UK|United Kingdom|United Kingdom|1|1|
|2,001|GB|Great Britain|England, Scotland  and Wales as a combined single entity|2|1|
|2,002|EW|England and Wales|England and Wales as a combined single entity|3|1|
|2,003|CTRY|Countries and Groupings|England, Wales, Scotland and Northern Ireland.|1|4|
|2,004|RGN|Regions|The Region area list contains nine areas for English Regions, and provides complete coverage of England only.|4|9|
|2,005|CNTY|Counties|Counties, Metropolitan Counties and Inner and Outher London.|4|35|
|2,006|LA|Local Authorities|Metropolitan Districts, Non-metropolitan District, London Boroughs and Unitary Authorities in England; Local Government Districts in Northern Ireland; Council Areas in Scotland; Unitary Authorities in Wales |1|404|
|2,007|WED|Wards and Electoral Divisions|Census Wards in England; Census Electoral Divisions in England;  Census Merged Wards in England; Census Wards in Northern Ireland; Census Wards in Scotland; Census Electoral Divisions in Wales; Census Merged Wards in Wales|1|9,481|
|2,008|MSOAIZ|Middle Super Output Areas and Intermediate Zones|Middle Super Output Area Set in England; Census Intermediate areas in Scotland; Middle Super Output Area Set in Wales|1|8,436|
|2,009|LSOADZ|Lower Super Output Areas and Data Zones|Lower Super Output Areas in England; Super Output Areas in Northern Ireland; Census Datazones in Scotland; Lower Super Output Areas in Wales|1|42,143|
|2,010|OASA|Output Areas and Small Areas|Output Areas in England; Output Areas in Scotland; Output Areas in Wales; Small Areas in Northern Ireland|1|232,296|
|2,011|MLA|Merging Local Authorities|Census Districts subject to merging for release of Detailed Characteristics.|4|4|
|2,012|MWED|Merging Wards and Electoral Divisions|Census Wards subject to merging for release of Detailed Characteristics.|4|43|
|2,013|WZLYR|Workplace Zone Layer|Workplace Zones|3|53,578|

