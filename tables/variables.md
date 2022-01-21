# tables/variables

The `variables` table displays information concerning the various different `variables` that are stored on the database.
The following `JOIN` queries can be carried out:

- `id` on the [topics](topics.md) table using `topic_id`.

## What are variables?

`variables` represent filters for a corresponding `topic` in a many to one relationship. These `variables` alongside their parent `topics` give context to the data e.g. a `variable` of `16 to 24` is a child of the `topic` `AGE`.

## Example use

I want to identify every `variable` for the `topic`:`SEX`. We will start by identidying the `id` for the chosen `topic` of `SEX`.

```sql
SELECT id, 
       abbreviation, 
       description  
  FROM c2011_meta.topics 
 WHERE abbreviation = 'SEX';
```

|id|abbreviation|description|
|-|-|-|
|64|SEX|The classification of a person as either male or female.|

Now that we know the ID of the desired `topic` is `64` we can do a JOIN with the `variables` table in order to find out the corresponding `variables`.

```sql
SELECT topic_id, 
       c2011_meta.variables.description 
  FROM c2011_meta.variables 
       LEFT JOIN c2011_meta.topics
       ON c2011_meta.variables.topic_id = c2011_meta.topics.id
 WHERE c2011_meta.topics.id = 64
```

|topic_id|description|
|-|-|
|64|Total: Sex|
|64|Males|
|64|Females|
|64|Male|
|64|Female|

The above table represents the possible `variables` for the topic `SEX`.
## Schema

|column|type|use|
|-|-|-|
|topic_id|int4|Foreign key that relates the variables table to the corresponding `topics` table|
|id|int4|Primary key.|
|description|varchar(500)|A description of the variable.|
|ordinal|int4|An arbitrary numerical order for the data.|
|parent_code|int4|Lists the `id` of any other variables that have a parent relationship to the variable|
|full_description|text|A longer version of the description field|


## Sample query

```sql
SELECT topic_id, 
       id, 
       description, 
       ordinal, 
       parent_code, 
       full_description 
  FROM c2011_meta.variables;
```

Will return the following:

|topic_id|id|description|ordinal|parent_code|full_description|
|-|-|-|-|-|-|
|1|2|Total: Accommodation type|1|NULL|Total\ Accommodation type|
|1|3|Unshared dwelling|2|NULL|Unshared dwelling|
|1|4|Whole house or bungalow|3|3|Unshared dwelling\ Whole house or bungalow|
|1|5|Detached|4|4|Unshared dwelling\ Whole house or bungalow\ Detached|
|1|6|Semi-detached|5|4|Unshared dwelling\ Whole house or bungalow\ Semi-detached|
|1|7|Terraced(including end-terrace)|6|4|Unshared dwelling\ Whole house or bungalow\ Terraced (including end-terrace)|
