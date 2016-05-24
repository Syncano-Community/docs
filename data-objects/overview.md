---
title: "Overview"
excerpt: "In this chapter you will learn:\n- What are Data Objects\n- What's inside a Data Object"
---
## Chapter contents
1. [Introduction](#introduction) 
2. [Data Object Fields](#data-object-fields)
3. [Field Types](#field-types)
4. [Adding custom (user defined) fields to Data Objects](#adding-custom-user-defined-fields-to-data-objects)
5. [Summary](#summary) 
[block:api-header]
{
  "type": "basic",
  "title": "Introduction"
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Because Data Objects on Syncano are schema based, you'll need to create a Class, where you can define a schema for your Data Objects. Creating Classes is described in [this chapter](doc:classes)."
}
[/block]
You can store data on Syncano using Data Objects. Data Objects are similar to `JSON` objects, that contain key-value pairs defined by you during Class creation. All of them also have default attributes defined by Syncano. 
[block:api-header]
{
  "type": "basic",
  "title": "Data Object Fields"
}
[/block]
Here's how a simple Data Object representing a book could look like:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 55,\n    \"author\": \"J. R. R. Tolkien\",\n    \"title\": \"The Fellowship of the Ring\",\n    \"text\": \"Hobbits and stuff\",\n    \"created_at\": \"2015-02-05T13:38:33.829340Z\", \n    \"updated_at\": \"2015-05-18T21:36:02.410173Z\", \n    \"revision\": 2, \n    \"owner\": null, \n    \"owner_permissions\": \"none\", \n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"none\", \n    \"channel\": null, \n    \"channel_room\": null,  \n    \"links\": {\n        \"self\": \"/v1.1/instances/syncano/classes/test/objects/55/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Apart from `author`, `title` and `text` fields, which you would define when creating a class, there are several built-in fields, which every object has. They are listed in the table below:
[block:parameters]
{
  "data": {
    "h-0": "Field",
    "h-1": "Type",
    "0-0": "id",
    "0-1": "integer",
    "1-0": "created_at",
    "2-0": "updated_at",
    "3-0": "revision",
    "3-1": "integer",
    "4-0": "links",
    "1-1": "datetime",
    "2-1": "datetime",
    "4-1": "array",
    "h-2": "Description",
    "0-2": "Data Object identifier",
    "1-2": "Data Object creation date in ISO 8601, UTC date time format with microsecond precision (i.e. 2015-02-13T07:32:05.659737Z)",
    "2-2": "Data Object latest modification date in in ISO 8601, UTC date time format with microsecond precision (i.e. 2015-02-13T07:32:05.659737Z)",
    "3-2": "Version of a Data Object. It increments every time a Data Object is updated in any way.",
    "4-2": "[HATEOAS](http://stackoverflow.com/questions/9192648/hateoas-concise-description) Links."
  },
  "cols": 3,
  "rows": 5
}
[/block]
As you can see, there are several fields that were not mentioned in the table above:
- `owner`, `owner_permissions`, `group`, `group_permissions`, `other_permissions` - these fields relate to permissions system on Syncano. You can read about them later in the [Permissions Chapter](doc:permissions)
- `channel` and `channel_room` - these fields can be used to take advantage of real-time communication features Syncano provides. You can read about the concept in depth in the [Realtime Communication](doc:realtime-communication) chapter.
[block:api-header]
{
  "type": "basic",
  "title": "Field Types"
}
[/block]
In our book example, `author`, `title` and `text` fields are custom and were created by a Syncano user. These custom fields can have several different types:

[block:parameters]
{
  "data": {
    "h-0": "Type",
    "h-1": "Description",
    "h-2": "Limit",
    "0-0": "`string`",
    "1-0": "`text`",
    "2-0": "`integer`",
    "3-0": "`float`",
    "6-0": "`boolean`",
    "7-0": "`datetime`",
    "8-0": "`file`",
    "9-0": "`reference`",
    "10-0": "`geopoint`",
    "4-0": "`array`",
    "5-0": "`object`",
    "5-1": "an unordered collection of key:value pairs",
    "4-1": "an ordered sequence of values",
    "0-1": "double-quoted Unicode with backslash escaping",
    "3-1": "Fractions like 0.3, -0.912 etc. Double precision field with up to 15 decimal digit precision",
    "6-1": "`true` or `false`",
    "2-1": "Digits 1-9, 0 and positive or negative",
    "1-1": "double-quoted Unicode with backslash escaping",
    "7-1": "ISO 8601, UTC date time format with microsecond precision (i.e. 2015-02-13T07:32:05.659737Z)",
    "8-2": "128mb",
    "0-2": "128 max character length",
    "1-2": "32000 max character length",
    "2-2": "32-bit (signed)",
    "3-2": "64-bit",
    "9-1": "Reference to a different Data Object",
    "10-1": "longitude and latitude",
    "8-1": "stored as a url"
  },
  "cols": 3,
  "rows": 11
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "A default value for any field type is `null`. What it means, is that when a Data Object is created and there's no value assigned to a custom property, it's value will be `null`."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Adding custom (user defined) fields to Data Objects"
}
[/block]
The custom fields are defined when creating a Class that wraps your Data Objects. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/CH6uQxuQVSnGKyTCw3p6",
        "Field_types.png",
        "1279",
        "655",
        "#697c9e",
        ""
      ]
    }
  ]
}
[/block]
You can read more about user-defined field types in the [Classes chapter](classes#section-types-of-class-schema-fields).


[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
You should have an overview of what Data Objects are and what kind of data types can be stored in Data Objects. It's time to learn the [basics of adding, updating, deleting and listing Data Objects](basics).