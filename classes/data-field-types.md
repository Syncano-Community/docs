---
title: "Class Data Field Types"
excerpt: ""
---

[block:api-header]
{
  "type": "basic",
  "title": "Class Schema"
}
[/block]
The `Class schema` is the set of fields you would like to use in a Data Object. In Syncano, the `class schema` is defined as a JSON list. Below we have an example JSON list for the `class schema` of a book.
[block:code]
{
  "codes": [
    {
      "code": "[  \n  {  \n    \"name\":\"book_title\",\n    \"type\":\"string\"\n   },\n  {\n  \t\"name\":\"publish_date\",\n    \"type\":\"datetime\"\n  },\n  {\n    \"name\":\"total_pages\",\n    \"type\":\"integer\"\n  },\n  {\n    \"name\":\"non-fiction\",\n    \"type\":\"boolean\"\n  },\n  {  \n    \"name\":\"author\",\n    \"type\":\"reference\",\n    \"target\":\"author\"\n  }\n]",
      "language": "json"
    }
  ]
}
[/block]
## Types of Class Schema Fields

The following types of fields are supported in class schema:
[block:parameters]
{
  "data": {
    "h-0": "field type",
    "h-2": "example",
    "0-0": "**string**",
    "1-0": "**text**",
    "2-0": "**integer**",
    "3-0": "**float**",
    "4-0": "**boolean**",
    "5-0": "**datetime**",
    "6-0": "**file**",
    "7-0": "**reference**",
    "0-2": "*This is my short book description.*",
    "1-2": "*This is my books long description... and it can be a very long string.*",
    "2-2": "*1337*",
    "3-2": "*2.25*",
    "4-2": "*true* or *false*",
    "h-1": "description",
    "0-1": "text field limited to 128 chars",
    "1-1": "text field limited to 32000 chars",
    "2-1": "number field",
    "3-1": "floating point field",
    "4-1": "boolean field",
    "6-1": "binary data, max 128MB",
    "7-2": "*65356*",
    "7-1": "reference to Data Object (related Data Object ID)",
    "5-1": "date in UTC format",
    "5-2": "2015-02-22T05:09:244327Z",
    "8-0": "**array**",
    "9-0": "**object**",
    "8-1": "array field of string types, int, boolean and float",
    "8-2": "*[1, 2, 3, 4]*",
    "9-1": "object field - JSON like",
    "9-2": "*{\n    \"attributeA\": \"A\",\n    \"attributeB\": \"B\",\n}*"
  },
  "cols": 3,
  "rows": 10
}
[/block]
### Reference field

If a field in the schema is of type `reference`, there has to be a specified target. A Target can either be self or a name of another class.

### Indexes

Indexes are necessary if you want to filter or order data objects based on a chosen field.
You can add indexes to most of the field types. Types without indexes are:

- text
- file

Below is an example of a schema with indexes:
[block:code]
{
  "codes": [
    {
      "code": "[  \n   {  \n      \"name\":\"title\",\n      \"type\":\"string\",\n      \"order_index\":true,\n      \"filter_index\":true\n   },\n   {  \n      \"name\":\"release_year\",\n      \"type\":\"integer\",\n      \"order_index\":true,\n      \"filter_index\":true\n   },\n   {  \n      \"name\":\"price\",\n      \"type\":\"float\",\n      \"order_index\":true,\n      \"filter_index\":true\n   },\n   {  \n      \"name\":\"author\",\n      \"type\":\"reference\",\n      \"order_index\":true,\n      \"filter_index\":true,\n      \"target\":\"Author\"\n   }\n]",
      "language": "json"
    }
  ]
}
[/block]
