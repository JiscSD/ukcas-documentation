## how many people are in fulltime employment in wales


1st step: user decides what geography they want to query

```sql
Select * from top_level_geographies
```

user now selects the entirety of Wales

```sql
Select * from c2011_meta.geography_areas
where c2011_meta.geography_areas.geography_grouping_id = 2003
and c2011_meta.geography_areas.top_level_geography_id = 7;
```

User now want to know how many people are in fulltime employment in wales

```sql
Select combination from c2011_meta.topic_combinations
where ARRAY['2003:7'] <@ ARRAY[c2011_meta.topic_combinations.geography_combinations];

```

We now have a list of all the possible topic combinations for Wales

Now we need to query the table for only `ECOACT`
```sql
Select combination from c2011_meta.topic_combinations
where ARRAY['2003:7'] <@ ARRAY[c2011_meta.topic_combinations.geography_combinations]
and ARRAY['ECOACT'] <@ ARRAY[c2011_meta.topic_combinations.combination];
``` 

Now user selects the group containing ECOACT that they desire (we've selected the simplest combination). This means that the results will be grouped by age and economic activity

`AGE,ECOACT,UNIT`

We now query the variable_combination table for all valid topic/variable combos.
```sql
Select topic_variable_combination, combination from c2011_meta.variable_combinations
where ARRAY['AGE','ECOACT','UNIT'] = c2011_meta.variable_combinations.topic_combination;

``` 

THhis returns a list of topic_variables_combinations, but in order to present this to the user we need to query what these are using the `combination` field.

so we have a list of arrays,
and we want to join these arrays onto the variables column.
so that we can in turn translate the id into 
```sql
Select topic_variable_combination, unnest(combination), c2011_meta.variables.description from c2011_meta.variable_combinations
cross join unnest(combination) v(id)
left join c2011_meta.variables ON variables.id = v.id
where ARRAY['AGE','ECOACT','UNIT'] = c2011_meta.variable_combinations.topic_combination;
```
^the above returns the descriptions of the variables matched against their corresponding IDs
