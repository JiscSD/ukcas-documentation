# UKCAS Project Documentation


## Contents

- [Tables](tables/index.md)
- [Usage Examples](usage_examples.md)

## Downloading the development database

We provide a smaller version of the census data for anyone who wants to experiment or build their own data explorer which is available [here](https://ukcas-dev-data.s3.eu-west-1.amazonaws.com/UKCAS_SQL_dump.zip) in the form of an sql dump.

The SQL dump should allow for a minimized version of the database to be establised from there read through the documentation to help understand the structure of the db.

## Understanding the data

The census data is split up into many seperate tables with their own respective folders. every census years data consists primarily of two schemas: 
a `meta data` schema which contains information regarding the context of the data
and a `data` schema which contains the actual numerical data. In order to query this data you have to proceed through the tables, gathering the meta data that is of interest and process it so that you get the desired result. For an example on how to do this see: [Usage Examples](usage_examples.md).

## Understanding the geography metadata

In the tables the geography data is described in 3 different ways:
- `Top level geographies`
- `Geography groups`
- `Geography area`

### Top level geographies

A `top-level geography` refers to the first (highest) geography level selectable. This is usually chosen at the start of a search by an end-user e.g. Wales (7) Below is a table of all the available top level geographies. It is referenced in the tables: [geography_areas](tables/geography_areas.md), 

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

### geography area

If you combine the two values then you get the resulting `geography_area`. This is stored with the format of: ${geography_grouping_id}:${top_level_geography_id}. So for example a geography area of 2005:6 would represent the counties of Northern Ireland.

### Topics and Variables

In the data the `geography_areas` are linked to `topics`, which in turn have their own sets of children that we refere to in this documentation as `variables`.

These `Topics` allow users to filter the data down and refine it to get the results they desire. The data however is grouped in a way that prevents users from refining down to specifically to identify individuals. for example you might only be interested in querying the topic `AGE` for a specific region, but you may have to search the topic_combination: `AGE`, and `Country of Birth` in order to get some results.

Variables represent the fine grained filters that match with the topics. So for example the topic `Country of Birth` might have the variables: `England`, `Germany`, `India` etc.
