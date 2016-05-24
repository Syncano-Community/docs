---
title: "Data Objects"
excerpt: "In this chapter you will learn:\n- What are Data Objects\n- What's inside a Data Object\n- How to perform different operations on Data Objects"
---
[block:api-header]
{
  "type": "basic",
  "title": "Chapter Contents:"
}
[/block]
1. [Overview](#overview)
2. [Data Object Management Overview](#data-object-management-overview)
3. [Creating a Data Object](#creating-a-data-object)
4. [Creating a Data Object with `file` property](#creating-a-data-object-with-file-property)
5. [Updating a Data Object](#updating-a-data-object)
6. [Incrementing/decrementing Data Object fields](#incrementingdecrementing-data-object-fields)
7. [Deleting a Data Object](#deleting-a-data-object)
8. [Getting Data Object details](#getting-data-object-details)
9. [Listing Data Objects](#listing-data-objects)
10. [Getting Data Objects Count](#getting-data-objects-count)
11. [Summary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Because Data Objects on Syncano are schema based, you'll need to create a Class, where you can define a schema for your Data Objects. \nCreating Classes is described in [this](doc:classes) chapter."
}
[/block]
You can store data on Syncano using Data Objects. 
Data Objects are similar to `JSON` objects, that contain key-value pairs defined by you on Class creation. All of them also have some default attributes defined by Syncano. 
Here's how a simple Data Object representing a book could looks like:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 55,\n    \"author\": \"J. R. R. Tolkien\",\n    \"title\": \"The Fellowship of the Ring\",\n    \"text\": \"<Hobbits and stuff>\",\n    \"created_at\": \"2015-02-05T13:38:33.829340Z\", \n    \"updated_at\": \"2015-05-18T21:36:02.410173Z\", \n    \"revision\": 2, \n    \"owner\": null, \n    \"owner_permissions\": \"none\", \n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"none\", \n    \"channel\": null, \n    \"channel_room\": null,  \n    \"links\": {\n        \"self\": \"/v1.1/instances/syncano/classes/test/objects/55/\"\n    }\n}",
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
    "0-2": "Data Object id",
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

# Field types

In our book example, `author`, `title` and `text` fields are custom and were created by a Syncano user. These custom fields can have several different types:
+ string
+ text
+ integer
+ float
+ boolean
+ datetime
+ file
+ reference
[block:callout]
{
  "type": "info",
  "body": "A default value for any field type is `null`. What it means, is that when a Data Object is created and there's no value assigned to a custom property, it's value will be `null`."
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
  "title": "Data Object Management Overview"
}
[/block]
Syncano allows to perform various actions on Data Objects. You can create, update, delete and get details of Data Objects, as well as list them. 
All of the actions performed on Data Objects can be done both from our Dashboard and when using Syncano's API / Libraries. You can see both ways in the examples below.
[block:callout]
{
  "type": "warning",
  "body": "All the examples below assume that you have created a \"book\" Class with the following schema:"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "[\n  {\n    \"name\": \"title\",\n    \"type\": \"string\"\n  },\n  {\n    \"name\": \"author\",\n    \"type\": \"string\"\n  }\n]",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating a Data Object"
}
[/block]
When you create a Data Object, you can set values for the fields that you defined when creating a Class. Since our Data Object has `title` and `author` fields, we add new object like this:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/XfBYgAxRZDxxpwYrAtGA",
        "Add_data_object_00.png",
        "1279",
        "655",
        "#25426e",
        ""
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/j4FN53RSviNutz1vOrFR",
        "Add_data_object_01.png",
        "1279",
        "655",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
Example of an object could look like on a screenshots below.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/SrzJYvGjTXOOYWvaKGig",
        "Add_data_object_02.png",
        "1556",
        "986",
        "#257ad5",
        ""
      ]
    }
  ]
}
[/block]
You can also add new object with code.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"title\": \"The Old Man and the Sea\", \"author\": \"Ernest Hemingway\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/book/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"book\",\n  title=\"The Old Man and the Sea\",\n  author=\"Ernest Hemingway\"\n)\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar book = {\n  title: \"The Old Man and the Sea\", \n  author: \"Ernest Hemingway\",\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().create(book).then(function(book) {\n\tconsole.log(\"book\", book);\n});",
      "language": "javascript"
    },
    {
      "code": "Book newBook = new Book();\nnewBook.author = \"Ernest Hemingway\";\nnewBook.title = \"The Old Man and the Sea\";\n\nResponse<Book> response = newBook.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "//define Class of an object\n@interface Book : SCDataObject\n@property (nonatomic,copy) NSString *title;\n@property (nonatomic,copy) NSString *subtitle;\n@end\n  \n@implementation Book\n@end\n  \n//creating a book using Book class\nBook *book = [Book new];\nbook.title = @\"How to be a Pirate\";\nbook.subtitle = @\"10 tips that will change your life.\";\n[book saveWithCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "//define Class of an object\nclass Book : SCDataObject {\n    var title = \"\"\n    var subtitle = \"\"\n}\n\n//creating a book using Book class\nlet book = Book()\nbook.title = \"How to be a Pirate\"\nbook.subtitle = \"10 tips that will change your life.\"\nbook.saveWithCompletionBlock { error in\n  //process saved book\n  //handle error\n}",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "require 'syncano'\n\nbook_class = instance.classes.find('book')\n\nbook_class.objects.create(title: 'The Old Man and the Sea', author: 'Ernest Hemingway')",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating a Data Object with `file` property"
}
[/block]
Adding Data Objects with the `file` property is a special case. First, make sure you've created a Class with a `file` field type in the Class Schema. 
Once you have done that, this is how creating a Data Object with a file looks like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: multipart/form-data\" \\\n-H \"X-API-KEY: API_KEY\" \\\n-F \"file=@PATH_TO_FILE\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/class_name/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nimage_path = \"/sample_path/image.jpg\"\nimage_file = open(image_path, \"r\")\nObject.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"CLASS_NAME\",\n  image=image_file\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\n// To upload your file, you have to use the `files` property of the input and attach it to the object via `Syncano.file` method. Do this during the submission of the form. \nvar object = {\n  file_field: Syncano.file(input.files[0]),\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\n// To upload files from the server environment, use the `fs`and `Syncano.file` method for attaching the file to your object.\n\nvar fs = require(\"fs\");\nvar object = {\n  file_field_name: Syncano.file(fs.readFileSync(\"path_to_my_file.txt\")),\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().create(object).then(function(object) {\n\tconsole.log(\"object\", object);\n});",
      "language": "javascript"
    },
    {
      "code": "Book bookWithCover = new Book();\nbookWithCover.author = \"Ernest Hemingway\";\nbookWithCover.title = \"The Old Man and the Sea\";\nbookWithCover.cover = new SyncanoFile(new File(getAssetsDir(), \"blue.png\"));\n\nResponse<Book> response = bookWithCover.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec"
    },
    {
      "code": "// Coming soon!",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Current limit for file size that can be stored on Syncano is 128MB."
}
[/block]
In the response, the `file` property will have to fields:
- type
- value

Value field is a url. The resource that you just uploaded is being stored under this url. 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"channel\": null,\n    \"channel_room\": null,\n    \"created_at\": \"2015-07-22T15:21:23.388062Z\",\n    \"file\": {\n        \"type\": \"file\",\n        \"value\": \"https://d3rij3t703q5l6.cloudfront.net/305/10/523956605dbc303fa74e06061f40ccf1ce1ea978.txt\"\n    },\n    \"group\": null,\n    \"group_permissions\": \"none\",\n    \"id\": 29,\n    \"links\": {\n        \"self\": \"/v1.1/instances/instance_name/classes/class/objects/29/\"\n    },\n    \"other_permissions\": \"none\",\n    \"owner\": null,\n    \"owner_permissions\": \"none\",\n    \"revision\": 1,\n    \"string\": null,\n    \"updated_at\": \"2015-07-22T15:21:23.388100Z\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Updating a Data Object"
}
[/block]
To update a Data Object, you need to know its ID. You can get IDs of all Data Objects by [listing them](#listing-data-objects).
[block:code]
{
  "codes": [
    {
      "code": "curl -X PATCH \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"title\": \"Harry Potter and the Philosophers stone\", \"author\": \"J. K. Rowling\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/book/objects/data_object_id/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.update(\n  id=OBJECT_ID,\n  title=\"Harry Potter and the Philosophers stone\",\n  author=\"J. K. Rowling\",\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"book\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n\tid: 7,\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nvar book = {\n  title: \"Harry Potter and the Philosopher's Stone\",\n  author: \"J. K. Rowling\"\n};\n\nDataObject.please().update(query, book).then(function(book) {\n\tconsole.log(\"book\", book)\n});",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\n// object to update has to have id set\nbook.setId(serverId);\nbook.author = \"Ernest Hemingway\";\nbook.title = \"The Old Man and the Sea\";\nbook.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "//we assume class is already defined\n@interface Book : SCDataObject\n  ...\n@end\n\nBook *book = ...//obtain book to update\nbook.title = @\"Pirate ships\";\nbook.subtitle = @\"Which was the biggest?\";\n[book saveWithCompletionBlock:^(NSError *error) {\n  //error handling\n}];",
      "language": "objectivec"
    },
    {
      "code": "//we assume class is already defined\nclass Book : SCDataObject {\n   //...\n}\n\nlet book = ...//obtain book to update\nbook.title = \"Pirate ships\"\nbook.subtitle = \"Which was the biggest?\"\nbook.saveWithCompletionBlock { error in\n  //error handling\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "book = book_class.object.find(6)\n\nbook.update_attributes title: 'The New Old Man and the See'\n\n# or \n\nbook.title = 'The New Old Man and the See'\nbook.save",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Incrementing/decrementing Data Object fields"
}
[/block]
Data object can have fields of different types (read about it more [here](classes#section-types-of-class-schema-fields).
If the type is set to `integer` - number field - it's possible not only to update object with new, passed value (so e.g. change `number_of_games ` from `20` to `25`), but also to increment/decrement chosen field by given value (e.g. by `1` or `5`).

How does it work? Let's say we have a game app and we created a Class for it named `Player`.
It holds informations about game users, and has one attribute called `number_of_games`. 
Players can buy games inside your app (we will be increasing `number_of_games` value) and every time they play a game - we want to decrease value of `number_of_games` by 1. 
We could do it by updating it's value - e.g. when the old value was 20, we could update it with 19. But to avoid conflicts, for situations where game is played at the same time on two devices, it's much safer to user our increase/decrease api.

Instead of sending a new value of the field when a player buys 10 games, you could send the following 
[block:code]
{
  "codes": [
    {
      "code": "{“number_of_games”: {“_increment”: 10}}",
      "language": "json"
    }
  ]
}
[/block]
When player played 1 game and we want to decrease it by one, we could send:
[block:code]
{
  "codes": [
    {
      "code": "{“number_of_games”: {“_increment”: -1}} ",
      "language": "json"
    }
  ]
}
[/block]
This way, you don't even have to know the old value of the field!

The whole request could look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X PATCH \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"number_of_games\": {\"_increment\":10}}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/player/objects/data_object_id/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.increment(\n  'counter', #field to increment\n  1, #by what value to increment\n  instance_name='instance_name, \n  class_name='class_name', \n  id=object_id #e.g. id = 45 \n  different_string_field = 'new content' #you can also update different fields \n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n\tid: 7,\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nvar book = {\n  number_of_games: {_increment: -1}\n};\n\nDataObject.please().update(query, book).then(function(book) {\n\tconsole.log(\"book\", book)\n});",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\n// object to update has to have id set\nbook.setId(serverId);\nbook.increment(\"number_of_games\", 10);\nbook.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// not supported yet",
      "language": "objectivec"
    },
    {
      "code": "// not supported yet",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "# not supported yet",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Deleting a Data Object"
}
[/block]
To delete a Data Object, its ID has to be known. You can get IDs of all Data Objects by [listing them](#listing-data-objects).
[block:code]
{
  "codes": [
    {
      "code": "curl -X DELETE \\\n-H \"X-API-KEY: API KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/book/objects/data_object_id/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.delete(\n  id=OBJECT_ID,\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"book\"\n)\n\n# or\n\ndata_object = Object.please.get(id='ID', instance_name=\"INSTANCE_NAME\")\ndata_object.delete()",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n\tid: 7,\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().delete(query).then(function() {\n\tconsole.log('delete');\n});",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\n// object to delete has to have id set\nbook.setId(serverId);\nbook.delete();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "//we assume class is already defined\n@interface Book : SCDataObject\n  ...\n@end\n  \nBook *book = ...//obtain book to delete\n[book deleteWithCompletion:^(NSError *error) {\n  //error handling\n}];",
      "language": "objectivec"
    },
    {
      "code": "//we assume class is already defined\nclass Book : SCDataObject {\n   //...\n}\n\nvar book = ..//obtain book to delete\nbook.deleteWithCompletion { error in\n  //error handling\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "book_class.objects.delete(6)",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Getting Data Object details"
}
[/block]
This will return the corresponding data object that you request. You can access the properties by using any language's specific methods of accessing Object properties. For example in Python, you type "book.title" to access the title property of a book object. 
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/book/objects/data_object_id/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\na_book_object = Object.please.get(\n  id=OBJECT_ID,\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"book\"\n)\n\nprint a_book_object.title",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n\tid: 7,\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().get(query).then(function(object) {\n\tconsole.log('object', object);\n});",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\n// object to delete has to have id set\nbook.setId(serverId);\n// values in the object will be updated\nbook.fetch();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "Book *book = [Book new];\nbook.objectId = @2; //ID of an object you'd like to download\n[book fetchWithCompletion:^(NSError *error) {\n    //error handling\n}];",
      "language": "objectivec"
    },
    {
      "code": "let book = Book()\nbook.objectId = 2 //ID of an object you'd like to download\nbook.fetchWithCompletion { error in\n    //error handling\n}",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "book_class.objects.get(6)",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Listing Data Objects"
}
[/block]
Easiest way to see all the Data Objects is with our [Dashboard](https://dashboard.syncano.io).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/QMADYBLRQOYpXeSxbvGQ",
        "Listing_objects_01.png",
        "1279",
        "656",
        "#274471",
        ""
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/OdxiwPpQXePurW7uLMRe",
        "Listing_objects_02.png",
        "1279",
        "653",
        "#27426b",
        ""
      ]
    }
  ]
}
[/block]
Or use one of our libraries:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/book/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.list(instance_name=\"INSTANCE_NAME\", class_name=\"book\")",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().list(query).then(function(objects) {\n\tconsole.log('objects', objects);\n});",
      "language": "javascript",
      "name": null
    },
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class).get();\nList<Book> books = response.getData();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[[Book please] giveMeDataObjectsWithCompletion:^(NSArray *books, NSError *error) {\n    //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "Book.please().giveMeDataObjectsWithCompletion { books, error in\n    //handle error\n}",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "book_class.objects.all",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Getting Data Objects Count"
}
[/block]
You can get the Data Objects count **estimation** when listing your objects. 
Syncano gives an exact count for Classes that have less than 1000 Data Objects and an estimate for Classes that have more than 1000 Data Objects. 
We are not able to provide exact count without affecting the performance of the platform. Here's how you can view the Data Objects count estimation:
[block:code]
{
  "codes": [
    {
      "code": "# page_size set to zero to exclude any Data Objects from the response for clarity\n\ncurl -X GET -G \\\n-H \"X-API-KEY: API_KEY\" \\\n-d include_count=true \\\n-d page_size=0 \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/CLASS_NAME/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models.base import Class\n\nconnection = syncano.connect(api_key='API_KEY')\nklass = Class.please.get(instance_name='INSTANCE_NAME', name='CLASS_NAME')\nklass.objects.count()\n                             \n# or:\n\nObject.please.list(\n  instance_name='INSTANCE_NAME', class_name=\"CLASS_NAME\").count()",
      "language": "python"
    },
    {
      "code": "// Planned for v1.1.0",
      "language": "javascript"
    },
    {
      "code": "Response<Integer> response = Syncano.please(Book.class).getCountEstimation();\nInteger count = response.getData();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPlease *please = [Book please];\n[please giveMeDataObjectsWithParameters:@{SCPleaseParameterIncludeCount : @(YES)} completion:^(NSArray *objects, NSError *error) {     \n  NSNumber *objectsCount = please.objectsCount;\n}];",
      "language": "objectivec"
    },
    {
      "code": "let please:SCPlease = Book.please()\nplease.giveMeDataObjectsWithParameters([SCPleaseParameterIncludeCount : true]) { (objects, error) in\n    let objectsCount:NSNumber = please.objectsCount\n}",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
Response:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"prev\":null,\n  \"objects_count\":48843,\n  \"objects\":[],\n  \"next\":null\n}",
      "language": "json"
    },
    {
      "code": "48837",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
Great! Now you know the basics of Syncano and you can actually start making you first apps! You can also move to the next chapter that is about running code snippets in the cloud, which we call [Scripts](doc:snippets-scripts).