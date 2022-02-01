# tables/geography_area_relations

The `geography_area_relations` table provides a record of child/parent relationships between `geography_areas` as well as their `geography_groupings`.

The following `JOIN` queries can be carried out:

- `id` on the [geography_areas](geography_areas.md) table using `parent_id` or using `child_id`.
- `id` on the [geography_grouping](geography_grouping.md) table using `child_grouping_id`.

## What are geography area relations?

A geography area relation is a parent/child relationship between [geography_areas](geography_areas.md), and [geography_groupings](geography_grouping.md). This helps provide further context to a geographic location. The data is entirely dependant on these relationships, and on it's own does not contain much context.

## Example use

Let's say we want to see all of the areas that are related to Scotland which has a `top_level_geography` id of `6`.
We can use the below query to identify all of the areas that have Scotland as a parent relation by doing a join between `geography_area_relations` and `geography_areas`.


```sql
SELECT parent_id, 
       child_id, 
       active, 
       c2011_meta.geography_areas.description  
  FROM c2011_meta.geography_area_relations
       LEFT JOIN c2011_meta.geography_areas
       ON c2011_meta.geography_area_relations.child_id = c2011_meta.geography_areas.id
 WHERE parent_id = 6
```

|parent_id|child_id|active|description|
|-|-|-|-|
|6|402|true|Clackmannanshire|
|6|403|true|Dumfries & Galloway|
|6|404|true|East Ayrshire|
|6|405|true|East Lothian|
|6|406|true|East Renfrewshire|
|...|...|...|...|


## Schema

|column|type|use|
|-|-|-|
|parent_id|int4|The parent `id` of the `geography_area` that relates to the `geography_area_relation`.|
|child_id|int4|The child `id` of the `geography_area` that relates to the `geography_area_relation`.|
|parent_grouping_id|int4|The parent `id` of the `geography_grouping` that relates to the `geography_area_relation`.|
|child_grouping_id|int4|The child `id` of the `geography_grouping` that relates to the `geography_area_relation`.|
|active|boolean|Whether a given relation is active and queryable.|

## Sample query

```sql
SELECT parent_id, 
       child_id, 
       parent_grouping_id, 
       child_grouping_id,
       active
  FROM c2011_meta.geography_area_relations
```

Returns the following:
|parent_id|child_id|parent_grouping_id|child_grouping_id|active|
|-|-|-|-|-|
|6|249747|2003|2010|true|
|6|249748|2003|2010|true|
|6|249749|2003|2010|true|
|6|249750|2003|2010|true|
|6|249751|2003|2010|true|
|...|...|...|...|...|

