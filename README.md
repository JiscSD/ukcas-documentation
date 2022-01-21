# UKCAS Project Documentation

UKCAS is a data exploration project that aims to provide current and previous years Census data in an easily accessible format.


## Contents

- [Tables](tables/index.md)
- [Usage Examples](usage_examples.md)

## Downloading the development database

We provide a smaller version of the census data for anyone who wants to experiment or build their own data explorer in the form of an sql dump.

[Download minimized census data](https://ukcas-dev-data.s3.eu-west-1.amazonaws.com/UKCAS_SQL_dump.zip)

The SQL dump should allow for a minimized version of the database to be establised from there read through the documentation to help understand the structure of the db.

## Understanding the data

The census data is split up into many seperate tables with their own respective schemas. every census years data consists of two schemas: 
a `meta data` schema which contains information regarding the context of the data. full descriptions of which can be found at [Tables](tables/index.md).
and a `data` schema which contains the actual numerical data. In order to query this data you have to proceed through the tables, gathering the meta data that is of interest and process it so that you get the desired result. For an example on how to do this see: [Usage Examples](usage_examples.md).

## Understanding the geography metadata

In the tables the geography data is described in 3 different ways (corresponding tables linked in brackets):
- `Top level geographies` ([top_level_geographies](tables/top_level_geographies.md))
- `Geography groups` ([geography_groupings](tables/geography_groupings.md))
- `Geography area` ([geography_areas](tables/geography_areas.md))

### Top level geographies

A `top level geography` refers to the first (highest) geography level selectable. This is usually chosen at the start of a search by an end-user e.g. Wales (7) Below is a table of all the available top level geographies. It is referenced in the tables: [geography_areas](tables/geography_areas.md), 

|top_level_geography_id|description|
|-|-|
|1|United Kingdom|
|2|Great Britain|
|3|England and Wales|
|4|England|
|5|Northern Ireland|
|6|Scotland|
|7|Wales|

### Geography groups

The geography_grouping defines how granular a particular area is based on one of 14 different classifications ranging from as broad as the entire UK (`geography_grouping_id` = 2000), all the way down to workplace zone layers (`geography_grouping_id` = 2013).See below for a list of the available geography_groupings.

|id|name|geography_area_count|
|-|-|-|
|2000|United Kingdom|1|
|2001|Great Britain|1|
|2002|England and Wales|1|
|2003|Countries and Groupings|4|
|2004|Regions|9|
|2005|Counties|35|
|2006|Local Authorities|404|
|2007|Wards and Electoral Divisions|9481|
|2008|Middle Super Output Areas and Intermediate Zones|8436|
|2009|Lower Super Output Areas and Data Zones|42143|
|2010|Output Areas and Small Areas|232296|
|2011|Merging Local Authorities|4|
|2012|Merging Wards and Electoral Divisions|43|
|2013|Workplace Zone Layer|53578|

### Geography areas

If you combine the two values then you get the resulting `geography_area`. This is stored with the format of: `{geography_grouping_id}:{top_level_geography_id}`. So for example a geography area of `2005:6` would represent the counties of Northern Ireland.

## Topics and Variables

In the data the `geography_areas` are linked to `topics`, which in turn have their own sets of children that we refer to in this documentation as `variables`. Topics represent high level categories of `variables` e.g. the Topic `AGE` has a set of variables such as: `16 to 24`, `24 to 30` etc.

These `Topics` allow users to filter the data down and refine it to get the results they desire. The data however is grouped in a way that prevents users from refining it down to identify individuals. for example you might only be interested in querying the topic `AGE` for a specific region, but you may have to search the topic_combination: `AGE`, and `Country of Birth` in order to get some results due to the restrictions.

Variables represent the fine grained filters that match with the topics. So for example the topic `Country of Birth` might has the variables: `England`, `Germany`, `India` etc.

```sql
SELECT id, 
       name, 
       geography_area_count, 
       description  
  FROM c2011_meta.geography_groupings
```