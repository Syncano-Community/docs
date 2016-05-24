---
title: "GeoPoints"
excerpt: ""
---
## Chapter Contents:
1. [Overview](#overview)
2. [Creating Data Objects with a GeoPoint field](#creating-data-objects-with-a-geopoint-field)
3. [Making GeoPoint queries](#making-geopoint-queries)

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Syncano allows for storing geographic coordinates. Thanks to `geopoints` you can, for instance, compare the distance between two users. You can also find out if a given place (e.g. restaurant) is within a range (5 miles) of a user. 


[block:api-header]
{
  "type": "basic",
  "title": "Creating Data Objects with a GeoPoint field"
}
[/block]
In order to be able to use the GeoPoints functionality, you'll need a Class with a GeoPoint schema field type. You'll also need to add a filtering index to this field if you'd like to make GeoPoint based queries. You can add one filtering index to GeoPoint fields per Class.
[block:callout]
{
  "type": "info",
  "body": "You can read more about adding schema fields and creating classes in [this chapter.](doc:classes)",
  "title": ""
}
[/block]
Once you have added the GeoPoint field type to your class, you can start creating Data Objects with GeoPoints. If you named your GeoPoint field as `where_am_i`, the call would look more or less like this:

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"where_am_i\": {\"latitude\": 66.5433889, \"longitude\": 25.8447679}}' \\\n\"https://api.syncano.rocks/v1.1/instances/INSTANCE/classes/CLASS/objects/\"",
      "language": "curl"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "When creating GeoPoints you should remember that the valid ranges for the coordinates are:\n- Latitude: -89 to 89\n- Longitude -179 to 179\n\nYou can create coordinates with a 13 digit precision after decimal point."
}
[/block]
This is how a Data Object object with a `where_am_i` GeoPoint field will look like:
[block:code]
{
  "codes": [
    {
      "code": "// some fields were skipped for brevity\n{\n    \"id\": 2, \n    \"created_at\": \"2016-05-16T14:39:22.804529Z\", \n    \"updated_at\": \"2016-05-16T14:39:22.804551Z\", \n    \"revision\": 1,  \n    \"where_am_i\": {\n        \"type\": \"geopoint\",\n        \"latitude\": -30.0, \n        \"longitude\": 23.0\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]

That's it when it comes to creating Data Objects with GeoPoint fields. What you would probably like to know is how to make all those cool coordinate queries, so here it is.
[block:api-header]
{
  "type": "basic",
  "title": "Making GeoPoint queries"
}
[/block]
There are two ways of filtering the results based on spacial coordinates. You can query if objects are:
1. [Near](#section-near-query)
2. [Exist](#section-exist-query) 


### Near query

Making a `near` query will return all the objects that are within the given range.
[block:callout]
{
  "type": "info",
  "body": "Default distance in `near` query is 100 miles\n\nMaximal distance is either 40075km or 24901miles (length of the equator)."
}
[/block]
A call with the default distance will look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g 'https://api.syncano.rocks/v1.1/instances/INSTANCE/classes/CLASS/objects/?query={\"where_am_i\":{\"_near\":{\"longitude\":66.5433889,\"latitude\":25.8447679}}}'",
      "language": "curl"
    }
  ]
}
[/block]
If you would like to specify the range, you can pass an additional parameter. It could be either `distance_in_miles` or `distance_in_kilometers`:

[block:code]
{
  "codes": [
    {
      "code": "# distance within range in kilometers\ncurl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g 'https://api.syncano.rocks/v1.1/instances/INSTANCE/classes/CLASS/objects/?query={\"where_am_i\":{\"_near\":{\"longitude\":66.5433889,\"latitude\":25.8447679,\"distance_in_kilometers\":1}}}'\n\n# distance within range in miles\n\ncurl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g 'https://api.syncano.rocks/v1.1/instances/INSTANCE/classes/CLASS/objects/?query={\"where_am_i\":{\"_near\":{\"longitude\":66.5433889,\"latitude\":25.8447679,\"distance_in_miles\":1}}}'",
      "language": "curl"
    }
  ]
}
[/block]

### Exist query

You can also check if a given coordinates can be found in your Class. This is how such a lookup would look like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g 'https://api.syncano.rocks/v1.1/instances/INSTANCE/classes/CLASS/objects/?query={\"where_am_i\":{\"_exists\":{\"longitude\":66.5433889,\"latitude\":25.8447679}}}'",
      "language": "curl"
    }
  ]
}
[/block]


That's all about the GeoPoints. We hope you'll find them useful!