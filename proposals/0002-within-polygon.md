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

This is how the Javascript SDK could look.

Parse.Query `withinPolygon(key, polygon)` 

Adds a constraint to the query that requires a particular key's coordinates be contained within a given polygon bounding box.

Parameters:
* `key` <String>
The key to be constrained

* `polygon` <Array>
A polygon consists of an array of "linear ring" coordinate arrays. A "linear ring" array consists at least four `Parse.GeoPoint` coordinates, where the first and last coordinates specify the same position.<sup>[1](#footnote1)</sup>

Notes:
The linear ring cannot self-intersect.


### Polygon with a single ring (Javascript example)
```
var myCoordinates = [
  [
    new Parse.Geopoint(55.246249,11.666107),
    new Parse.Geopoint(55.297884,11.780090),
    new Parse.Geopoint(55.228240,11.852875),
    new Parse.Geopoint(55.182788,11.753998)
  ]
];
var query = new Parse.Query;
query.withinPolygon('myKey', myCoordinates);
```

### Polygon with multiple rings (Javascript example)
```
var outerPoints = [
  new Parse.GeoPoint(55.225499,11.684647),
  new Parse.GeoPoint(55.273640,11.758804),
  new Parse.GeoPoint(55.213356,11.839828),
  new Parse.GeoPoint(55.187493,11.743011)
];
var innerPoints = [
  new Parse.GeoPoint(55.242726,11.748505),
  new Parse.GeoPoint(55.224324,11.725159),
  new Parse.GeoPoint(55.210222,11.751938),
  new Parse.GeoPoint(55.227848,11.775970)
];
var myCoordinates = [outerPoints,innerPoints];
var query = new Parse.Query;
query.withinPolygon('myKey', myCoordinates);
```

Extending the SDK with a `withinPolygon` query constraint.

MongoDB 2.4 [added support](https://docs.mongodb.com/manual/applications/geospatial-indexes/#geojson-objects) for GeoJSON storage and queries. MongoDB 2.6 added support for additional GeoJSON types: MultiPoint, MultiLineString, MultiPolygon, GeometryCollection.

More info about GeoJSON polygon can be found [here](https://docs.mongodb.com/manual/reference/geojson/#geojson-polygon).

The docs for MongoDB geospatial query operations are [here](https://docs.mongodb.com/manual/applications/geospatial-indexes/#query-operations). We would probably need to look specifically at the [$geoWithin](https://docs.mongodb.com/manual/reference/operator/query/geoWithin/#op._S_geoWithin) operator.

There's a useful application for playing with polygons [here](http://www.birdtheme.org/useful/v3tool.html).

## Alternatives considered

There are currently no alternative ways to do this with the current Parse SDK.

<a name="footnote1"><sup>1</sup></a> Discussion: One could argue that three coordinates would be enough, and that the first coordinate would be re-added to the linear ring array before querying the database.