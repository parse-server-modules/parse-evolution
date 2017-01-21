# Feature name

* Proposal: 0002-within-polygon
* Authors: [Mads Bjerre](https://github.com/madsb)
* Review Manager: TBD
* Status: **Awaiting review**

## Introduction
## Motivation

Parse currently supports query constraints `withinGeoBox`, `withinKilometers`, `withinMiles` and `withinRadians`. Assume we want to find objects within a polygon.

## Proposed solution

Introducing a `withinPolygon` constraint that takes an array of lat/lon coordinates, or even nested arrays of coordinates for MultiPolygon support.

## Detailed design

Extending the SDK with a `withinPolygon` query constraint.

MongoDB 2.4 [added support](https://docs.mongodb.com/manual/applications/geospatial-indexes/#geojson-objects) for GeoJSON storage and queries. MongoDB 2.6 added support for additional GeoJSON types: MultiPoint, MultiLineString, MultiPolygon, GeometryCollection.

The docs for MongoDB geospatial query operations are [here](https://docs.mongodb.com/manual/applications/geospatial-indexes/#query-operations).

We would probably need to look specifically at the [$geoWithin](https://docs.mongodb.com/manual/reference/operator/query/geoWithin/#op._S_geoWithin) operator.

## Alternatives considered

There are currently no alternative ways to do this with the current Parse SDK.