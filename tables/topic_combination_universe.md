# tables/topic_combination_universe
The `topic_combination_universe` table lists out the available `topic` combinations for each possible set of `universe`.

## What are universes?
A `universe` is the account of the values that are present in all of the combinations. E.g. Residents aged 16 and over in employment.

## Schema

|column|type|use|
|-|-|-|
|id|int4|primary key|
|topic_combination_id|int4|Foreign key to link the record with the `topic_combinations`|
|table_name|varchar(10)|The original table name from the casweb table, used as a reference only|
|universe|varchar(255)|lists the values that are present in all of the combinations, and so can be ignored for filtering purposes.|


## Sample query

```sql
SELECT id, 
       topic_combination_id, 
       table_name, 
       universe
  FROM c1981_meta.topic_combination_universes
```

This query will return the following table:

|id|topic_combination_id|table_name|universe|
|-|-|-|-|
|7|7|S32|Residents in such households|
|15|13|S30|Residents with different address one year before Census|
|17|13|S32|Residents in such households|

