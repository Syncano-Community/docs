---
title: "Data Objects Filtering & Ordering"
excerpt: "In this chapter you'll learn:\n- How to filter Data Objects\n- How to order Data Objects\n- How to combine filtering and ordering with Data Objects pagination"
---
Chapter Sections:
1. [Overview](#overview)
2. [Setting up indexing](#setting-up-indexing)
3. [Filtering Data Objects](#filtering-data-objects)
4. [Examples of filtering Data Objects](#examples-of-filtering-data-objects)
5. [Ordering Data Objects](#ordering-data-objects)
6. [Using paging with filtering and ordering](#using-paging-with-filtering-and-ordering)
7. [Summary](#summary) 
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Once you have your data in place, you might want to get it from the Syncano Platform. To make things easier and more flexible we allow filtering and ordering of the Data Objects that you make `GET` requests on. 
[block:api-header]
{
  "type": "basic",
  "title": "Setting up indexing"
}
[/block]
To be able to perform filtering or ordering on Data Objects, you should first add appropriate indexes to the Class Schema.
[block:callout]
{
  "type": "warning",
  "body": "Without proper indexes added to the Data Objects fields in Class Schema, filtering or ordering will not work!"
}
[/block]
As an example, we will use a class named Book, with following fields:
[block:code]
{
  "codes": [
    {
      "code": "Book {\n  year : integer, //year it was published\n  num_of_pages : integer, // number of pages\n  title: string, //book title\n  intro: text, //introduction to the book - requires longer text\n  author: string, //name of the author\n  category: string //category name\n}",
      "language": "json"
    }
  ]
}
[/block]
If you wanted to filter and order on a `year` field (to e.g. get all books published after year 2010 and sort by year), you would have to have `order_index` and `filter_index` set to `true` in Class Schema:
[block:code]
{
  "codes": [
    {
      "code": "{ \n  \"name\": \"year\",\n  \"type\": \"integer\", \n  \"order_index\": true, \n  \"filter_index\": true\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "You can read more about Class Schemas [here](doc:classes)."
}
[/block]

### Filtering and ordering of the default Data Object fields
There are several default fields in a Data Object (the ones that you don't define in schema, but they are created for you). Some of these fields can be ordered and filtered but you don't have to add indexing to these fields. 

These are the operations that you can perform on the default Data Object fields:
[block:parameters]
{
  "data": {
    "0-0": "`id`",
    "1-0": "`created_at`",
    "2-0": "`updated_at`",
    "3-0": "`revision`",
    "4-0": "`owner`",
    "5-0": "`group`",
    "h-1": "Filtering",
    "h-2": "Ordering",
    "0-1": "<div style=\"color: green\">&#x2713;</div>",
    "1-1": "<div style=\"color: green\">&#x2713;</div>",
    "2-1": "<div style=\"color: green\">&#x2713;</div>",
    "3-1": "<div style=\"color: green\">&#x2713;</div>",
    "4-1": "<div style=\"color: green\">&#x2713;</div>",
    "5-1": "<div style=\"color: green\">&#x2713;</div>",
    "0-2": "<div style=\"color: green\">&#x2713;</div>",
    "1-2": "<div style=\"color: green\">&#x2713;</div>",
    "2-2": "<div style=\"color: green\">&#x2713;</div>",
    "3-2": "<div style=\"color: red\">&#x2717;</div>",
    "4-2": "<div style=\"color: red\">&#x2717;</div>",
    "5-2": "<div style=\"color: red\">&#x2717;</div>",
    "h-0": "Field"
  },
  "cols": 3,
  "rows": 6
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Filtering Data Objects"
}
[/block]

In general, you would add filtering to your request, if you wanted to get only the Data Objects that meet certain criteria. You can achieve that by adding extra parameters to your Data Object `GET` call:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g \"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\" \\\n-d query={\"created_at\":{\"_gt\":\"2015-02-23T12:00:00Z\"}}",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar filter = {\"created_at\":{\"_gt\":\"2015-02-23T12:00:00Z\"};\n\nDataObject.please().list(list).filter(filter).then(callback);",
      "language": "javascript"
    },
    {
      "code": "import datetime\nimport syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\ndate = datetime.datetime.strptime(\"2013-06-22 18:27:13.886665\",\n                                  \"%Y-%m-%d %H:%M:%S.%f\")\n# created_at is the property we are checking\n# is greater than '__gt'\nbooks = Object.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"book\"\n).filter(created_at__gt=date)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseList = Syncano.please(Book.class)\n        .where().gt(Entity.FIELD_CREATED_AT, date).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPredicate *pirateBookPredicate = [SCPredicate whereKey:@\"title\" isEqualToString:@\"Pirate ships\"];\n[[Book please] giveMeDataObjectsWithPredicate:pirateBookPredicate parameters:nil completion:^(NSArray *books, NSError *error) {\n   //error handling\n}];",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "let pirateBookPredicate = SCPredicate.whereKey(\"title\", isEqualToString: \"Pirate ships\")\nBook.please().giveMeDataObjectsWithPredicate(pirateBookPredicate, parameters: nil) { books, error in\n   //error handling\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
In the example above, results will be filtered by the `created_at` field and they will be greater than (`_gt`) the given date.

Here you can find all the possible filter options for different types of Data Object fields:
[block:parameters]
{
  "data": {
    "h-0": "Name of the filter",
    "h-1": "Description",
    "h-2": "Example",
    "0-0": "**_gt**",
    "1-0": "**_gte**",
    "2-0": "**_lt**",
    "3-0": "**_lte**",
    "4-0": "**_eq**",
    "5-0": "**_neq**",
    "6-0": "**_exists**",
    "7-0": "**_in**",
    "7-1": "**In a given list**\n\n- checking if value of the field is on the provided list\n\n(list can contain up to 128 values)",
    "0-2": "```\n{\"year\": {\"_gt\": 1998}}\n```\n```\n{\"meeting_date\": {\"_gt\": \"2015-02-22T05:09:24Z\"}}\n```",
    "1-2": "```\n{\"year\": {\"_gte\": 2000}}\n```",
    "0-1": "**Greater than**\n\n- checking if value is greater than provided\n- for `string`, it compares the alphabetical ordering",
    "1-1": "**Greater than or equal to**\n\n- checking if value is greater or equal than provided\n- for `string`, it compares the alphabetical ordering",
    "2-1": "**Less than**\n\n- checking if value is lower than provided\n- for `string`, it compares the alphabetical ordering",
    "3-1": "**Less than or equal to**\n\n- checking if value is lower or equal than provided\n- for `string`, it compares the alphabetical ordering",
    "4-1": "**Equal to**\n\n- checking if value is equal than provided",
    "5-1": "**Not equal to**\n\n- checking if value different than provided",
    "6-1": "**Exist**\n\n- checking if field is not empty",
    "6-2": "```\n{\"year\": {\"_exists\":true}}\n```\n```\n{\"subtitle\":{\"_exists\":true}}\n```",
    "7-2": "```\n{\"year\":{\"_in\":[2000, 2001, 2002]}}\n```",
    "5-2": "```\n{\"year\":{\"_neq\":2001\"}}\n```\n```\n{\"title\":{\"_neq\":\"The Old Man and the Sea\"}}\n```",
    "4-2": "```\n{\"year\":{\"_eq\":2000}}\n```\n```\n{\"title\": {\"_eq\":\"The Old Man and the Sea\"}}\n```\n```\n{\"meeting_date\":{\"_eq\": \"2015-02-22T05:09:24Z\"}}\n```",
    "3-2": "```\n{\"year\": {\"_gte\": 2000}}\n```",
    "2-2": "```\n{\"price\": {\"_lt\": 2.25}}\n```",
    "h-3": "Suitable for",
    "0-3": "`integer`, `float`, `string`, `datetime`",
    "1-3": "`integer`, `float`, `string`, `datetime`",
    "2-3": "`integer`, `float`, `string`, `datetime`",
    "3-3": "`integer`, `float`, `string`, `datetime`",
    "4-3": "`integer`, `float`, `boolean`, `string`, `datetime`",
    "5-3": "`integer`, `float`, `boolean`, `string`, `datetime`",
    "6-3": "`integer`, `float`, `boolean`, `string`, `datetime`",
    "7-3": "`integer`, `float`, `boolean`, `string`, `datetime`"
  },
  "cols": 4,
  "rows": 8
}
[/block]
There are also several filtering options available for `string` field types:


[block:callout]
{
  "type": "info",
  "body": "All examples below assume that we will be searching for a book with a title \"The Old Man and the Sea\". All the queries would return a book with such title"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Name of the filter",
    "h-1": "Description",
    "h-2": "Example",
    "0-0": "**_startswith**",
    "1-0": "**_endswith**",
    "2-0": "**_contains**",
    "3-0": "**_istartswith**",
    "4-0": "**_iendswith**",
    "5-0": "**_icontains**",
    "6-0": "**_ieq**",
    "0-1": "**Starts with**\n\nChecks if a string starts with the given query",
    "1-1": "**Ends with**\n\nChecks if a string ends with the given query",
    "2-1": "**Contains**\n\nChecks if a string contains the given query",
    "3-1": "**Case insensitive starts with**\n\nChecks if a string starts with the given query while ignoring the case of the letters",
    "4-1": "**Case insensitive ends with**\n\nChecks if a string ends with the given query while ignoring the case of the letters",
    "5-1": "**Case insensitive contains**\n\nChecks if a string contains the given query while ignoring the case of the letters",
    "6-1": "**Case insensitive equal to**\n\nChecks if a string is equal the given query while ignoring the case of the letters",
    "0-2": "```\n{\"title\": {\"_startswith\":\"The Old Man\"}}\n```",
    "1-2": "```\n{\"title\": {\"_endswith\":\"the Sea\"}}\n```",
    "2-2": "```\n{\"title\": {\"_contains\":\"Man\"}}\n```",
    "3-2": "```\n{\"title\": {\"_istartswith\":\"ThE OLd m\"}}\n```",
    "4-2": "```\n{\"title\": {\"_iendswith\":\"the SEA\"}}\n```",
    "5-2": "```\n{\"title\": {\"_icontains\":\"OLD MaN\"}}\n```",
    "6-2": "```\n{\"title\": {\"_ieq\":\"ThE OlD mAn anD thE SeA\"}}\n```"
  },
  "cols": 3,
  "rows": 7
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Filtering in Syncano Libraries",
  "body": "Using one of the Syncano Libraries, filtering might look differently from what was described here. To learn how to filter using chosen library, please follow our quick start guides available [here](doc:syncano-libraries)."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Examples of filtering Data Objects"
}
[/block]
1. [Introduction](#section-introduction)
2. [Simple filter on one field](#section-simple-filter-on-one-field)
3. [Complex filtering on one field](#section-complex-filtering-on-one-field)
4. [Filtering on multiple fields](#section-filtering-on-multiple-fields)

### Introduction
Below you'll find couple of Data Object filtering examples. The examples assume that you have a Class called `books` and that this Class has the following schema:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"type\": \"string\", \n    \"name\": \"string\"\n}, \n{\n    \"order_index\": true, \n    \"filter_index\": true, \n    \"type\": \"integer\", \n    \"name\": \"release_year\"\n}, \n{\n    \"type\": \"float\", \n    \"name\": \"float\"\n}, \n{\n    \"type\": \"text\", \n    \"name\": \"text\"\n}, \n{\n    \"type\": \"text\", \n    \"name\": \"text2\"\n}, \n{\n    \"order_index\": true, \n    \"filter_index\": true, \n    \"type\": \"integer\", \n    \"name\": \"pages\"\n}, \n{\n    \"type\": \"file\", \n    \"name\": \"file\"\n}",
      "language": "json"
    }
  ]
}
[/block]
In this Class, there are two fields with indexes, book `release_year` and  number of `pages`.

I've populated `books` Class with some Data Objects, so without filtering I would see all of them.
[block:callout]
{
  "type": "warning",
  "title": "Examples use fields skipping for clarity.",
  "body": "In the examples below I use fields skipping that shows only fields that I'm interested in. All examples would also work without it but there would be much more noise.\n\nYou can read more about fields skipping [here](http://docs.syncano.com/v0.1.1/docs/skipping-fields-in-api-endpoints)"
}
[/block]
I'll skip all the fields except `release_year`, `id` and `pages` fields. Let's try it out:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\"\\\n-d fields=release_year,id,pages",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar fields = [\"release_year\", \"id\", \"pages\"];\n\nDataObject.please().list(list).fields(fields).then(callback);",
      "language": "javascript"
    },
    {
      "code": "[Book.please giveMeDataObjectsWithParameters:@{ \n    SCPleaseParameterFields : @[@\"release_year\", @\"id\", @\"pages\"] \n} completion:^(NSArray *books, NSError *error) {\n        //handle error\n}];",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "Book.please().giveMeDataObjectsWithParameters([SCPleaseParameterFields:[\"release_year\", \"id\", \"pages\"]]) \n{ books, error in\n    //handle error\n}",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "ResponseGetList<Book> responseSkip = Syncano.please(Book.class)\n        .selectFields(FilterType.INCLUDE_FIELDS, \"release_year\", \"id\", \"pages\").get();",
      "language": "java",
      "name": "Android"
    }
  ]
}
[/block]
Ok, here is the response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }, \n        {\n            \"id\": 643, \n            \"release_year\": 1735, \n            \"pages\": null\n        }, \n        {\n            \"id\": 644, \n            \"release_year\": 1999, \n            \"pages\": null\n        }, \n        {\n            \"id\": 2134, \n            \"release_year\": 2010, \n            \"pages\": 666\n        }, \n        {\n            \"id\": 2135, \n            \"release_year\": 650, \n            \"pages\": 672\n        }, \n        {\n            \"id\": 2136, \n            \"release_year\": 1223, \n            \"pages\": 110\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
I created the `pages` field later, so some of the Data Objects have null as the value for the `pages` field.

### Simple filter on one field

We will start from a simple query on `release_year`. We want to filter Data Objects that have a value of `release_year` greater than 1900. To achieve that, we only have to add a *query* param to the URL:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\"\\\n-d fields=release_year,id,pages \\\n-d query={\"release_year\":{\"_gt\":1900}}",
      "language": "json"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar filter = {\"release_year\":{\"_gt\":1900}};\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar fields = [\"release_year\", \"id\", \"pages\"];\n\nDataObject.please()\n  .list(list)\n\t.filter(filter)\n  .fields(fields)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).filter(release_year__gt=1900)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseSimple = Syncano.please(Book.class).where().gt(\"release_year\", 1900).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPredicate *bookPredicate = [SCPredicate whereKey:@\"release_year\" isGreaterThanNumber:@1990];\n[[Book please] giveMeDataObjectsWithPredicate:bookPredicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n}];",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "let bookPredicate = SCPredicate.whereKey(\"release_year\", isGreaterThanNumber: 1990)\nBook.please().giveMeDataObjectsWithPredicate(bookPredicate, parameters: nil) { books, error in\n    //error handling\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
We can see that Data Objects with ids 643, 2135 and 2136 disappeared, because they had a smaller `release_year` value. 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }, \n        {\n            \"id\": 644, \n            \"release_year\": 1999, \n            \"pages\": null\n        }, \n        {\n            \"id\": 2134, \n            \"release_year\": 2010, \n            \"pages\": 666\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]

### Complex filtering on one field

We can apply multiple filters to one field by adding another key-value pair to the query dictionary.
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\"\\\n-d fields=release_year,id,pages \\\n-d query={\"release_year\":{\"_gt\":1900,\"_lte\":2000}}",
      "language": "json"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar filter = {\"_gt\":1900,\"_lte\":2000}};\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar fields = [\"release_year\", \"id\", \"pages\"];\n\nDataObject.please()\n  .list(list)\n\t.filter(filter)\n  .fields(fields)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models.base import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).filter(\n    release_year__gt=1900,\n    release_year__lte=2000\n)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseComplex = Syncano.please(Book.class)\n        .selectFields(FilterType.INCLUDE_FIELDS, \"release_year\", \"id\", \"pages\")\n        .where().gt(\"release_year\", 1900).lte(\"release_year\", 2000).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPredicate *bookPredicate1990 = [SCPredicate whereKey:@\"release_year\" isGreaterThanNumber:@1990];\nSCPredicate *bookPredicate2000 = [SCPredicate whereKey:@\"release_year\" isLessThanOrEqualToNumber:@2000];\nSCCompoundPredicate *predicate = [SCCompoundPredicate compoundPredicateWithPredicates:@[bookPredicate1990,bookPredicate2000]];\n[[Book please] giveMeDataObjectsWithPredicate:predicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n}];",
      "language": "objectivec"
    },
    {
      "code": "let bookPredicate1990 = SCPredicate.whereKey(\"release_year\", isGreaterThanNumber: 1990)\nlet bookPredicate2000 = SCPredicate.whereKey(\"release_year\", isLessThanOrEqualToNumber: 2000)\nlet predicate = SCCompoundPredicate.compoundPredicateWithPredicates([bookPredicate1990,bookPredicate2000])\nBook.please().giveMeDataObjectsWithPredicate(predicate, parameters: nil) { books, error in\n    //error handling\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
In this case we would get Data Objects with `release_year` values that are greater than 1900 and smaller or equal to 2000.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }, \n        {\n            \"id\": 644, \n            \"release_year\": 1999, \n            \"pages\": null\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
### Filtering on multiple fields

It's also possible to do filtering on more than one field at once. Here is a query which is the same as the previous one but also has a second filter for the `pages` value.
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\"\\\n-d fields=release_year,id,pages \\\n-d query={\"release_year\":{\"_gt\":1900,\"_lte\":2000},\"pages\":{\"_gt\":199}}",
      "language": "json"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar filter = {\"release_year\":{\"_gt\":1900,\"_lte\":2000},\"pages\":{\"_gt\":199}};\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar fields = [\"release_year\", \"id\", \"pages\"];\n\nDataObject.please()\n  .list(list)\n\t.filter(filter)\n  .fields(fields)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).filter(\n    release_year__gt=1900,\n    release_year__lte=2000,\n    pages__gt=199\n)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseMultiple = Syncano.please(Book.class)\n        .selectFields(FilterType.INCLUDE_FIELDS, \"release_year\", \"id\", \"pages\")\n        .where().gt(\"release_year\", 1900).lte(\"release_year\", 2000).gt(\"pages\", 199).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPredicate *bookPredicate1990 = [SCPredicate whereKey:@\"release_year\" isGreaterThanNumber:@1990];\nSCPredicate *bookPredicate2000 = [SCPredicate whereKey:@\"release_year\" isLessThanOrEqualToNumber:@2000];\nSCPredicate *bookPredicatePages199 = [SCPredicate whereKey:@\"pages\" isGreaterThanNumber:@199];\n\nSCCompoundPredicate *predicate = [SCCompoundPredicate compoundPredicateWithPredicates:@[bookPredicate1990,bookPredicate2000,bookPredicatePages199]];\n[[Book please] giveMeDataObjectsWithPredicate:predicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n}];",
      "language": "objectivec"
    },
    {
      "code": "let bookPredicate1990 = SCPredicate.whereKey(\"release_year\", isGreaterThanNumber: 1990)\nlet bookPredicate2000 = SCPredicate.whereKey(\"release_year\", isLessThanOrEqualToNumber: 2000)\nlet bookPredicatePages199 = SCPredicate.whereKey(\"pages\", isGreaterThanNumber: 199)\n\nlet predicate = SCCompoundPredicate.compoundPredicateWithPredicates([bookPredicate1990,bookPredicate2000,bookPredicatePages199])\nBook.please().giveMeDataObjectsWithPredicate(predicate, parameters: nil) { books, error in\n    //error handling\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
This time, we're left with only one value! The `release_year` value that is greater than 1900 _and_ smaller than or equal to 2000 _and_ `pages` value is bigger than 199. 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Ordering Data Objects"
}
[/block]
Apart from limiting the response by using filtering, you can also request that the Data Objects to be ordered the way you want. To do that you should add an `order_by` parameter to the `GET` requests that you make. Assuming that you have an `order_index` set to `true` for `release_year` field, this is how such a call will look like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g \"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\" \\\n-d order_by=release_year",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar orderBy = \"release_year\";\n\nDataObject.please()\n  .list(list)\n\t.orderBy(orderBy)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).order_by(\n    'pages'\n)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseOrdered = Syncano.please(Book.class)\n        .orderBy(\"release_year\").get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[Book.please giveMeDataObjectsWithParameters:@{\n    SCPleaseParameterOrderBy : @\"release_year\" \n} completion:^(NSArray *books, NSError *error) {\n     //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "Book.please().giveMeDataObjectsWithParameters(\n    [SCPleaseParameterOrderBy:\"release_year\"]) { books, error in\n    //handle error\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
The default direction is ascending. If you want to reverse direction of ordering, add a minus sign to the field name:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-g \"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\" \\\n-d order_by=-release_year",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar orderBy = \"-release_year\";\n\nDataObject.please()\n  .list(list)\n\t.orderBy(orderBy)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).order_by(\n    '-pages'\n)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseOrderedReversed = Syncano.please(Book.class)\n        .orderBy(\"release_year\", SortOrder.DESCENDING).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[Book.please giveMeDataObjectsWithParameters:@{\n    SCPleaseParameterOrderBy : @\"-release_year\" \n} completion:^(NSArray *books, NSError *error) {\n     //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "Book.please().giveMeDataObjectsWithParameters(\n    [SCPleaseParameterOrderBy:\"-release_year\"]) { books, error in\n    //handle error\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
If you try to order by an attribute that doesn't exist, you will get an error message:

[block:code]
{
  "codes": [
    {
      "code": "{ \"detail\": \"Incorrect order_by parameter provided.\" }",
      "language": "json"
    }
  ]
}
[/block]
Ordering works with filtering. Just add another query parameter to the request:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/my_instance/classes/books/objects/\" \\\n-d -g query={\"release_year\":{\"_gt\":1900}} \\\n-d fields=release_year,id,pages \\\n-d order_by=release_year",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar filter = {\"release_year\":{\"_gt\":1900}};\nvar fields = [\"release_year\", \"id\", \"pages\"];\nvar orderBy = \"-release_year\";\n\nDataObject.please()\n  .list(list)\n\t.filter(filter)\n\t.fields(fields)\n\t.orderBy(orderBy)\n  .then(callback);",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.list(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\"\n).filter(\n    release_year__gt=1900,\n    release_year__lte=2000,\n    pages__gt=199\n).order_by(\n    'pages'\n)",
      "language": "python"
    },
    {
      "code": "ResponseGetList<Book> responseOrderedFiltered = Syncano.please(Book.class)\n        .selectFields(FilterType.INCLUDE_FIELDS, \"release_year\", \"id\", \"pages\")\n        .orderBy(\"release_year\").where().gt(\"release_year\", 1990).get();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPredicate *bookPredicate1990 = [SCPredicate whereKey:@\"release_year\" isGreaterThanNumber:@1990];\n[Book.please giveMeDataObjectsWithPredicate:bookPredicate1990 parameters:@{\n    SCPleaseParameterOrderBy : @\"release_year\" \n} completion:^(NSArray *books, NSError *error) {\n     //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "let bookPredicate1990 = SCPredicate.whereKey(\"release_year\", isGreaterThanNumber: 1990)\n \nBook.please().giveMeDataObjectsWithPredicate(bookPredicate1990, parameters:[SCPleaseParameterOrderBy:\"release_year\"]) { books, error in\n    //handle error\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
Result will be filtered to Data Objects that have `release_year` values greater than 1900 and will be sorted in a ascending order:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 2134, \n            \"release_year\": 2010, \n            \"pages\": 666\n        }, \n        {\n            \"id\": 644, \n            \"release_year\": 1999, \n            \"pages\": null\n        },\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using paging with filtering and ordering"
}
[/block]
It works as you would expect, you can combine it:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/books/objects/\" \\\n-d -g query={\"release_year\":{\"_gt\":1900,\"_lte\":2000},\"pages\":{\"_gt\":199}} \\\n-d fields=release_year,id,pages \\\n-d page_size=1",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar list = {instanceName: \"INSTANCE_NAME\", className: \"CLASS_NAME\"};\nvar fields = [\"release_year\", \"id\", \"pages\"];\nvar pageSize = 1;\n\nDataObject.please()\n  .list(list)\n\t.fields(fields)\n\t.pageSize(pageSize)\n  .then(callback);",
      "language": "javascript"
    }
  ]
}
[/block]
The API will automatically create pages with size equal to `page_size` for you and provide a link to the next page.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": \"/v1.1/instances/syncano/classes/books/objects/?direction=1&last_pk=2134&page_size=1&query=%7B%22release_year%22%3A%7B%22_gt%22%3A2%2C%22_lte%22%3A163%7D%2C%22pages%22%3A%7B%22_gt%22%3A12%7D%7D\", \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 642, \n            \"release_year\": 1985, \n            \"pages\": 200\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
More info about paging can be found [here](http://docs.syncano.com/v0.1/docs/general-information#paging).
[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
In this chapter you have learned how to use filtering and ordering when making Data Object requests to Syncano. You have also learned how to combine the ordering and filtering queries and use paging on the top of that.

Go to [Traces](doc:traces) chapter to learn about debugging your Scripts run on Syncano.