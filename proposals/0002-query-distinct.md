# Feature name

* Proposal: 0002-query-aggregate
* Authors: [Kulshekhar Kabra](https://github.com/kulshekhar)
* Review Manager: TBD
* Status: **Awaiting review**

## Introduction

Allow users to make queries that use aggregates (distinct, sum, count, etc)

## Motivation

It is an often requested and useful feature which would enhance the querying capability offered by parse server

One [Related issue](https://github.com/ParsePlatform/parse-server/issues/2238)

## Proposed solution

Introduce two additional parameters (similar to the existing `keys`, `order`, `limit`, `skip` and `include` parameters)

- `group`: this will identify the fields and the aggregate functions to be applied to them, if any
- `having`: this would be similar to the `where` clause but would be applicable to aggregated fields. 

## Detailed design

The new parameters will need to be whitelisted in `ClassesRouter.js`. They will also need to go through any sanitization processes that exist to prevent injection attacks.

The `having` parameter without the `group` parameter would be ignored.

The specification for the `group` parameter can be based on the `$group` operator from MongoDB. The operators allowed can differ based on the database adapter (The next section contains a list of operators available in Postgres and MongoDB)

The specification for the `having` parameter can be based on the existing `where` parameter.

The adapters will need to modify their query generation functions to incorporate these changes

There is a direct relation between the new parameters introduced and Postgres's query semantics.

For MongoDB the `group` parameter has a direct implementation in `$group`. To implement `having`, inspiration can be taken from [this PDF](https://rickosborne.org/download/SQL-to-MongoDB.pdf)

If the `group` parameter is present in addition to the `keys` and/or `include` parameters, I'd argue that these (`keys`/`include`) should be ignored as they wouldn't really make sense. Any documentation for `group` should probably include a mention of this decision.

## Aggregate operators 

|Postgres|MongoDB|
|---|---|
||$addToSet|
|array_agg|$push|
|avg|$avg|
|bit_and|
|bit_or|
|bool_and|
|bool_or|
|count|
|every|
||$first
|json_agg|$push
|jsonb_agg|$push
|json_object_agg|
|jsonb_object_agg|
||$last
|max|$max
|min|$min
|string_agg|
|sum|$sum
|xmlagg|
|corr|
|covar_pop|
|covar_samp|
|regr_avgx|
|regr_avgy|
|regr_count|
|regr_intercept|
|regr_r2|
|regr_slope|
|regr_sxx|
|regr_sxy|
|regr_syy|
|stddev|
|stddev_pop|$stdDevPop
|stddev_samp|$stdDevSamp
|variance|
|var_pop|
|var_samp|

## Alternatives considered

None really
