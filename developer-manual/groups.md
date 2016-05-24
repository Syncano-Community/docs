---
title: "Groups"
excerpt: "In this chapter you'll learn how to:\n* Create groups\n* Add users to a group\n* Add objects with group permissions\n* Learn groups usage with examples"
---
Chapter sections:
1. [Groups overview](#groups-overview)
2. [Creating a Group](#creating-a-group)
3. [Adding Users to a Group](#adding-users-to-a-group)
4. [Creating objects with Group permissions](#creating-objects-with-group-permissions)
[block:api-header]
{
  "type": "basic",
  "title": "Groups overview"
}
[/block]
Groups on Syncano are tightly coupled with permissions. They can be used to construct different levels of access to resources stored on the platform. After you create a group, you can add users to this group. Later on, during Data Object creation, you can decide which group of users will have access to it. We will discuss this in details in the sections below.
[block:callout]
{
  "type": "info",
  "title": "Limits",
  "body": "Maximum number of Groups user can belong to is `32`"
}
[/block]
This is how a Group object looks like when stored on Syncano:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"label\": \"group_label\",\n    \"description\": \"group description\"\n    \"links\": {\n        \"self\": \"/v1.1/instances/INSTANCE/groups/1/\", \n        \"users\": \"/v1.1/instances/INSTANCE/groups/1/users/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
It has two properties:
[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`id`",
    "0-1": "Group identifier.",
    "1-0": "`label`",
    "1-1": "Group label.",
    "2-0": "`description`",
    "2-1": "Group description."
  },
  "cols": 2,
  "rows": 3
}
[/block]
List of currently existing Groups can be found in the left column of `Users & Groups` view in the [Dashboard](https://dashboard.syncano.io).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Dv6Kp0WJTgq7SY75s1eM",
        "Groups_01.png",
        "1679",
        "771",
        "#254473",
        ""
      ]
    }
  ]
}
[/block]
So the Group object is pretty simple. Let's learn how to create Groups.
[block:api-header]
{
  "type": "basic",
  "title": "Creating a Group"
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

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"label\":\"my_group_label\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance/groups/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Group\nimport syncano\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nGroup.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    label=\"my new group\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Group = connection.Group;\n\nvar options = {\n  label: \"GROUP_LABEL\", \n  description: \"GROUP_DESCRIPTION\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nGroup.please().create(options).then(callback);",
      "language": "javascript"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "Group newGroup = new Group(\"group_label\");\nResponse<Group> response = syncano.createGroup(newGroup).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
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

[block:api-header]
{
  "type": "basic",
  "title": "Adding Users to a Group"
}
[/block]
Now, when we have a group created, we can start adding the users there. 
[block:callout]
{
  "type": "info",
  "body": "This example assumes that there are existing users on your Instance. If you'd like to know how to add users to your application, go [here](user-management)."
}
[/block]
Click 'more' icon on chosen user.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/eIL2dhcrQPqU6xBhy6gW",
        "Add_group_to_user_01.png",
        "1679",
        "771",
        "#254472",
        ""
      ]
    }
  ]
}
[/block]
Click 'Edit a User' option.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/NYldtRcARYqw57oLuIIP",
        "Add_group_to_user_02.png",
        "1679",
        "771",
        "#254473",
        ""
      ]
    }
  ]
}
[/block]
On the `Edit` pop-up, select a Group from the drop-down menu.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/YtdgTs4dTHCLmRzRtJB9",
        "Add_group_to_user_03.png",
        "1679",
        "768",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
Selected Group will show up in the `Edit` window. Click `Confirm` to save changes.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Z5ndRWDxRmelj7TcJ3zv",
        "Add_group_to_user_04.png",
        "1679",
        "771",
        "#2a3e5b",
        ""
      ]
    }
  ]
}
[/block]
You will also see Groups user belongs to, on the users list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/4H9pJOMwQMOijLQL1r3q",
        "Add_group_to_user_05.png",
        "1622",
        "842",
        "#254573",
        ""
      ]
    }
  ]
}
[/block]
Call to add a group to a user can look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"user\": user_id}' \\\n\"https://api.syncano.io/v1.1/instances/instance/groups/group_id/users/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import User\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY_WITH_ALLOW_USER_CREATE_FLAG\") \n\nGordon = User.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    username=\"Gordon Freeman\",\n    password=\"Black Mesa\"\n)\n\ngroup = Group.objects.get(id=1)\nGordon.add_to_group(group_id=group.id)\n\n# or:\n\ngroup.add_user(user_id=Gordon.id)",
      "language": "python"
    },
    {
      "code": "// Planned for v.1.1.0",
      "language": "javascript"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "Response<GroupMembership> response = syncano.addUserToGroup(group.getId(), user.getId()).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
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

[block:api-header]
{
  "type": "basic",
  "title": "Creating objects with Group permissions"
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