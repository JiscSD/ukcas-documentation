# tables/geography_area_relations

The `geography_area_relations` table provides a record of child/parent relationships between `geography_areas` as well as their `geography_groupings`.

The following `JOIN` queries can be carried out:



## What are geography area relations?

A geography area relation is a parent/child relationship between `geography_areas`, or `geography_groupings`. This helps provide further context to a geographic location.

## Example use

Searching for the Electoral Wards within Yorkshire.


```sql

```


## Schema

|column|type|use|
|-|-|-|
|parent_id|int4|The `geography_area.id` of the parent geography area.|
|child_id|int4|The `geography_area.id` of the child geography area.|
|parent_grouping_id|int4|The `geography_grouping_id` for the parent geography area.|
|child_grouping_id|int4|The `geography_grouping_id` for the child geography area.|
|active|boolean|Whether a given relation is active.|

## Sample query

```sql

```

Returns the following:


