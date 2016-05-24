---
title: "Classes"
excerpt: "What is a class? How do I create one? What can it hold?"
---
## Chapter Contents:
1. [Overview](#overview)
2. [Creating a Class](#creating-a-class)
3. [Class Schema](#class-schema)
4. [Class Statuses](#class-statuses)
5. [Class Data Objects count](#class-data-objects-count)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
**Classes** are templates for data objects you will store in Syncano. In order to be able to [add Data Objects](data-objects#section-creating-a-data-object), you have to define a **Class** for that type of data object. To [create a class](#creating-a-class) you have to provide:

- The name of the class
- A description (optional)
- A schema
[block:callout]
{
  "type": "info",
  "title": "Limits",
  "body": "For paid accounts, maximum number of Classes that can be created within an Instance is `100`."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating a Class"
}
[/block]
There are couple of ways to create a Class. You can do it from the Dashboard (the recommended way) or by using one of the libraries that supports this feature. We show both approaches below:
[block:callout]
{
  "type": "warning",
  "title": "Class permissions",
  "body": "This section explains how to Create Classes as a Syncano Administrator (which is you). If you'd like your users to be able to interact with Classes and their Data Objects, you must make sure to use the correct Class [permissions](doc:permissions)."
}
[/block]
### Creating a Class from the Dashboard
To create a class, you choose the respective instance, and then click  "Classes" in sidebar as shown below.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/lWafsIguQO6oHViTjLRQ",
        "Add_class_01.png",
        "1213",
        "622",
        "#26426f",
        ""
      ],
      "caption": "Adding class"
    }
  ]
}
[/block]
Click the the plus button, add class name and the schema (field names and types) to your Class.  Additionally you can also add a description, and decide if you you want the field to have `Filter` or `Order` index (more on indexes [here](#section-indexes)).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/GhGfdMT6Rjahwc8mEg8V",
        "Add_class_02.png",
        "1279",
        "653",
        "#6c7c9c",
        ""
      ]
    }
  ]
}
[/block]
### Creating a Class by using the API
[block:callout]
{
  "type": "warning",
  "body": "Although we recommend using the [Syncano Dashboard](https://dashboard.syncano.io/#/login) to create a Class, you can also create one by using Syncano's API. \nIn order to do so, you must make sure that you have connected to your instance using an Account Key (not an API Key). \nTo learn more about them, see our [previous chapter](doc:authentication) where we talk about API and Account Keys ."
}
[/block]
 Once you have connected to an instance with an Account Key, you can follow the code below. You can notice that we gave a name to our Class, an optional description, and a **schema**, which is where we define all the attributes for our class. 
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d name=book \\\n-d schema='[{\"name\":\"book_title\",\"type\":\"string\"},\n            {\"name\": \"publish_date\",\"type\":\"datetime\"},\n            {\"name\":\"total_pages\",\"type\":\"integer\"},\n            {\"name\":\"non-fiction\",\"type\":\"boolean\"},\n            {\"name\":\"author\",\"type\":\"reference\", \"target\":\"author\"}]' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models.base import Class\n\nconnection = syncano.connect(api_key=\"ACCOUNT_KEY\")\n\nbook_class = Class.please.create(\n    instance_name='INSTANCE_NAME',\n    name='books',\n    schema=[\n        {\"name\": \"book_title\", \"type\": \"string\"},\n        {\"name\": \"publish_date\", \"type\": \"datetime\"},\n        {\"name\": \"total_pages\", \"type\": \"integer\"},\n        {\"name\": \"non-fiction\", \"type\": \"boolean\"},\n        {\"name\": \"author\", \"type\": \"reference\", \"target\": \"self\"}\n    ])",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano');  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Class = connection.Class; \n\nvar schema = [\n  {\"name\": \"book_title\", \"type\": \"string\"},\n  {\"name\": \"publish_date\", \"type\": \"datetime\"},\n  {\"name\": \"total_pages\", \"type\": \"integer\"},\n  {\"name\": \"non-fiction\", \"type\": \"boolean\"},\n  {\"name\": \"author\", \"type\": \"reference\", \"target\": \"self\"}\n]\n\nvar data = {\n  name: 'Class_name',\n  description: 'New description',\n  schema: schema,\n  instanceName: 'INSTANCE_NAME'\n};\n\nClass.please().create(data).then(function(cls) {\n\tconsole.log('class', cls)\n});",
      "language": "javascript"
    },
    {
      "code": "    @SyncanoClass(name = \"Book\")\n    class Book extends SyncanoObject {\n\n        public static final String FIELD_TITLE = \"book_title\";\n        public static final String FIELD_PAGES = \"total_pages\";\n        public static final String FIELD_NON_FICTION = \"non-fiction\";\n        public static final String FIELD_AUTHOR = \"author\";\n\n        @SyncanoField(name = FIELD_TITLE)\n        public String title;\n        @SyncanoField(name = FIELD_PAGES)\n        public Integer pages;\n        @SyncanoField(name = FIELD_NON_FICTION)\n        public Boolean nonFiction;\n        @SyncanoField(name = FIELD_AUTHOR)\n        public Author author;\n    }\n\nsyncano.createSyncanoClass(Book.class).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "instance.classes.create(name: 'book', schema: [\n  {name: 'title', type: 'string', filter_index: true },\n  {name: 'publish_date', type: 'datetime', order_index: true },\n  {name: 'total_pages', type: 'integer'},\n  {name: 'non-fiction', type: 'boolean'},\n  {name: 'author', type: 'string', target: 'author'} ])\n    ",
      "language": "ruby"
    }
  ]
}
[/block]

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

[block:callout]
{
  "type": "warning",
  "title": "index fields limit",
  "body": "You can add only 16 fields of type \"orderIndex\" or \"filterIndex\". For example: a class schema could contain 10 filter indexes and 6 order indexes (8 in total)."
}
[/block]
Filtering and ordering is explained in detail in the [Data Objects Filtering & Ordering](doc:data-objects-filtering) chapter.
[block:api-header]
{
  "type": "basic",
  "title": "Class Statuses"
}
[/block]
Each class has a status, which is one of two values:
- `ready` - means that the Class is ready and Data Objects can be created within the Class.
- `migrating` - class schema and relations are being set up. It's not possible to create Data Objects within the Class.

A Class can have a `migrating` status in two situations: 
- after the Class was created
- after the Class schema was edited. 
 
Class migrations usually don't take longer than a couple of seconds. If your app doesn't rely on editing or creating Classes, you won't be affected by this behavior. 
[block:api-header]
{
  "type": "basic",
  "title": "Class Data Objects count"
}
[/block]
As I mentioned in the beginning of this chapter, Classes define a structure for the Data Objects to base their properties on. Another cool feature of Classes, is that you can see a number of Data Objects that inherit from the Class. 
To see that, go to `Dashboard` -> `Classes` view. The `Objects` column on the Classes list will tell you the current number of Data Objects:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/w3mNoLYHQVSn2b9lHHkY",
        "classes_foo.png",
        "2050",
        "699",
        "#265eb5",
        ""
      ]
    }
  ]
}
[/block]
As you can see, there are `5` Objects in the Class called, wait for it.. `class`! 
That's is quite straightforward. 
What can be a bit more surprising is the object count for the `trigger` Class -- which is `~ 48837`. The `~` sign shows that this is an **estimation**. Syncano shows an estimate count for Classes that have more than 1000 Data Objects. The reason for it is that we can't provide a precise count without affecting the performance of the platform.
[block:callout]
{
  "type": "info",
  "title": "Did you know?",
  "body": "You can also get count of Data Objects using our API, when making a call directly to the Data Objects endpoint! \nSee the [Data Objects](data-objects) chapter for details."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For more details about the available methods for classes, visit the [Classes API reference](http://docs.syncano.com/v0.1.1/docs/classes-list)."
}
[/block]