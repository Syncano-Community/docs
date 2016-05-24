---
title: "Basics"
excerpt: "In this chapter you will learn:\n- What are Data Objects\n- What's inside a Data Object\n- How to perform different operations on Data Objects"
---
## Chapter contents

1. [Overview](#overview)
2. [Creating a Data Object](#creating-a-data-object)
3. [Updating a Data Object](#updating-a-data-object)
4. [Deleting a Data Object](#deleting-a-data-object)
5. [Getting Data Object details](#getting-data-object-details)
6. [Listing Data Objects](#listing-data-objects)
7. [Summary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
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
Response:
[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
Great! Now you know the basics of Syncano and you can actually start making you first apps! You can also move to the next chapter that is about [Filtering & Ordering](doc:data-objects-filtering-ordering) of Data Objects.