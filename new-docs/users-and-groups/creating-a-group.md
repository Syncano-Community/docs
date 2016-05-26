---
title: "Creating a Group"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Creating Data with Group Permissions]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This is how you can create Groups on Syncano. The only parameter you'll have to pass here is a group label (name):
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/K53IVUv7RGWcQQ8mkOYQ",
        "Add_group_01.png",
        "1679",
        "736",
        "#244371",
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
        "https://www.filepicker.io/api/file/N8soC7XgR5WDfLoZt0RT",
        "Add_group_02.png",
        "1679",
        "763",
        "#244474",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating Data with Group Permissions"
}
[/block]
When creating objects, there's an option to set Group permissions:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/EJV7bJFMQ3eoB3iLoViF",
        "Add_object_with_group_permissions_01.png",
        "1679",
        "872",
        "#264472",
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
        "https://www.filepicker.io/api/file/HZm6xDYTSMKGXyBEdO3Y",
        "Add_object_with_group_permissions_v1.png",
        "1678",
        "772",
        "#546c94",
        ""
      ]
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: API_KEY\" \\\n-d '{\"group\": 1, \"group_permissions\": \"full\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance/classes/class/objects/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Object\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY\")\n\nObject.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\",\n    group=1,\n    group_permissions=\"full\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar book = {\n  title: 'Book Title',\n  group: 1, \n  group_permissions: \"full\",\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_NAME\"\n};\n\nDataObject.please().create(book).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\nbook.title = \"Title\";\nbook.setGroup(group.getId());\nbook.setGroupPermisions(DataObjectPermissions.FULL);\n\nResponse<Book> response = book.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec"
    },
    {
      "code": "// Coming soon!",
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
Now, users from the group with an id = `1` will have full access to this Data Object.

These are the possible permissions that users can have:
[block:parameters]
{
  "data": {
    "h-0": "Roll",
    "h-1": "Description",
    "0-0": "`full`",
    "1-0": "`write`",
    "2-0": "`read`",
    "1-1": "Read/write access to the object.",
    "2-1": "Permission to access the object in read-only mode.",
    "0-1": "Read/write/delete access to the object."
  },
  "cols": 2,
  "rows": 3
}
[/block]
The example above showed how to create a Data Object with group permissions but these permissions can be set on various resources. Here's a list of objects that have 'group_permissions` property:
* Class
* Data Object
* User
[block:callout]
{
  "type": "info",
  "body": "For more details about the available methods for groups, visit the [Groups API reference](http://docs.syncano.com/v0.1.1/docs/groups-list).",
  "title": "Learn More"
}
[/block]