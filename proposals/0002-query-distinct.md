# Feature name

* Proposal: 0002-query-distinct
* Authors: [Kulshekhar Kabra](https://github.com/kulshekhar)
* Review Manager: TBD
* Status: **Awaiting review**

## Introduction

Allow users to fetch distinct values for a column that matches a given query

## Motivation

It is an often requested and useful feature.

[Related issue](https://github.com/ParsePlatform/parse-server/issues/2238)

## Proposed solution

Introducing an additional parameter `distinct` (similar to the existing `keys`, `order`, `limit`, `skip` and `include` parameters)

## Detailed design

The new parameter will need to be whitelisted in `ClassesRouter.js`. It will also need to go through any sanitization processes that exist to prevent injection attacks.

Additionally, the presence of this parameter should modify the queries as follows:

- In MongoDB, execute the `.distinct()` method instead of the usual `find` query
- In Postgres, use `DISTINCT(fieldname)` instead of the usual list of field names in the `find` query

If the `distinct` parameter is present in addition to the `keys` and/or `include` parameters, I'd argue that these (`keys`/`include`) should be ignored as they wouldn't really make sense. Any documentation for `distinct` should probably include a mention of this decision.

## Alternatives considered

Another alternative that could work would be to allow modifiers with field names in the `keys` parameter. Something like `distinct:field1,field2`.

However, this seems brittle and likely would introduce a point for SQL injection/other issues.
