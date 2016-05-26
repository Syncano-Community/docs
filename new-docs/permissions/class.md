---
title: "Data Object"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Examples]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Permission types for Classes are different from the ones for Data Objects, because they not only consider access to a Class itself but also to the Data Objects that belong to that Class. So if you would like a user to have `read` access to a certain Data Object, you should not only grant that user access to that Data Object but also set its Class permissions to `read`.  If you would like your users to be able to create Data Objects within a Class, the Class should have `create_objects` permission type set for those users (for a group users belongs to, or for `others` - which will affect every other user inside your Syncano Instance).
[block:parameters]
{
  "data": {
    "0-0": "`none`",
    "1-0": "`read`",
    "2-0": "`create_objects`",
    "h-0": "Permission Type",
    "h-1": "Description",
    "0-1": "Users don't have access to a Class and they won't be able to create Data Objects with this Class schema.",
    "1-1": "Means that users can get Class info + read Data Objects associated with that Class (provided that Data Objects permissions are set for read or higher for the specific user).",
    "2-1": "Default state. Users can read and create Data Objects within that Class."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "When a Class is being created it's `group_permissions` and `other_permissions` will be `create_objects` by default. This way your application users will have an ability to create Data Objects in all of the Classes within an Instance. If you'd like to limit their access you can change the Class permissions to `read` or `none`."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Examples"
}
[/block]
We'll go through granting users permission to create Data Objects within a Class. 
We'll create a simple Class, with only one field ("title") that has `other_permissions` = `create_objects`. This way any user will be able to add and read Data Objects within this Class.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/CyHlwWwMTua3ASFfwj3J",
        "Add_class_03.png",
        "1679",
        "871",
        "#254471",
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
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"name\": <class_name>,\n     \"other_permissions\":\"create_objects\",\n     \"schema\":[{\"type\":\"string\",\"name\":\"title\"}]}' \\\n\"https://api.syncano.io/v1.1/instances/<instance>/classes/\"",
      "language": "curl"
    },
    {
      "code": "import python\nfrom syncano.models import Class\n\nsyncano.connect(api_key=\"API_KEY\")\n\nClass.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    name=\"CLASS_NAME\",\n    other_permissions=\"create_objects\",\n    schema=[{'name':'title','type':'string'}]\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar account = new Syncano({accountKey: ACCOUNT_KEY});\n\nvar options = {\n  \"name\": CLASS_NAME,\n  \"other_permissions\":\"create_objects\",\n  \"schema\":[{\"type\":\"string\",\"name\":\"title\"}]\n};\n\naccount.instance('INSTANCE_NAME').class().add(options, callback());",
      "language": "javascript"
    },
    {
      "code": "SyncanoClass syncanoClass = new SyncanoClass(ExampleObject.class);\nsyncanoClass.setOtherPermissions(SyncanoClassPermissions.CREATE_OBJECTS);\n\nResponse<SyncanoClass> response = syncano.createSyncanoClass(syncanoClass).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use our Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use our Dashboard instead",
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
And that's it! Now any user can create and read objects in this class. If you'd like to create more fine-grained permissions, you could create a group and add selected users to it. Next, when creating a Class, you'd set `group_permissions` to `create_objects` and pass a group id to the `group` parameter:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/RKsrN8NbSFDhhnfP6h5Q",
        "Add_class_04.png",
        "1678",
        "871",
        "#274470",
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
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"name\": class_name,\n     \"group_permissions\":\"create_objects\",\n     \"group\": group_id,\n     \"schema\":[{\"type\":\"string\",\"name\":\"title\"}]}' \\\n\"https://api.syncano.io/v1.1/instances/instance/classes/\"",
      "language": "curl"
    },
    {
      "code": "import python\nfrom syncano.models.base import Class\n\nsyncano.connect(api_key=\"API_KEY\")\n\nClass.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    name=\"CLASS_NAME\",\n    group_permissions=\"create_objects\",\n    group=1,\n    schema=[{'name':'newer','type':'string'}]\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar account = new Syncano({accountKey: ACCOUNT_KEY});\n\nvar options = {\n  \"name\": CLASS_NAME,\n  \"group_permissions\":\"create_objects\",\n  \"group\": GROUP_ID,\n  \"schema\":[{\"type\":\"string\",\"name\":\"title\"}]\n};\n\naccount.instance('INSTANCE_NAME').class().add(options, callback());",
      "language": "javascript"
    },
    {
      "code": "SyncanoClass syncanoClass = new SyncanoClass(ExampleObject.class);\nsyncanoClass.setGroup(group.getId());\nsyncanoClass.setGroupPermissions(SyncanoClassPermissions.CREATE_OBJECTS);\n\nResponse<SyncanoClass> response = syncano.createSyncanoClass(syncanoClass).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use our Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use our Dashboard instead",
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