

### Examples

If you never dealt with UNIX file system permissions, the concept might be a bit vague at first. We'll try to explain with a couple of examples. Let's consider 3 scenarios, when a User with an `id` = 123 will be able to read a Data Object with an `id`=1: 
1. If a User is an `owner` and `owner_permissions` is `read` or higher (`write` or `full`), this is how the Data Object permission properties will look (non-permission properties removed for clarity):
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"owner\": 123, \n    \"owner_permissions\": \"read\", \n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"none\"\n}",
      "language": "json"
    }
  ]
}
[/block]
2. If the user belongs to a `group` associated with that Data Object and that Data Object has `group_permissions` = `read` or higher. 
If our user belonged to a Group with `id` 321, then this is how the Data Object permission settings could look like:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"owner\": null, \n    \"owner_permissions\": \"none\", \n    \"group\": 321, \n    \"group_permissions\": \"read\", \n    \"other_permissions\": \"none\"\n}",
      "language": "json"
    }
  ]
}
[/block]
3. Last check is `other_permissions`. Our user with an id = `123` is not an owner of the Data Object and doesn't belong to a group that is connected with this Data Object. Nevertheless the user can still read the Data Object because `other_permissions` are set to `read`:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"owner\": null, \n    \"owner_permissions\": \"none\", \n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"read\"\n}",
      "language": "json"
    }
  ]
}
[/block]

### Creating a Data Object with an `owner_permissions` example
[block:callout]
{
  "type": "info",
  "body": "To see full guide on adding Data Objects, please see [this chapter](doc:data-objects) of Developer Manual."
}
[/block]
It's worth noting that when a Data Object is created, its `owner_permissions` are `none` by default. What it means is that even if your user creates a Data Object, the user won't have access to it.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/TsJnQM4vQAurhfObPeVN",
        "Add_data_object_01.png",
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
        "https://www.filepicker.io/api/file/o1doHmfRTkSDZkutr08V",
        "Add_data_object_02.png",
        "1679",
        "873",
        "#849cb4",
        ""
      ]
    }
  ]
}
[/block]
To make sure the user has access you should set the `owner_permissions` to `read` or higher during Data Object creation:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/iZaGfda3RnEAnxTEQZin",
        "Add_data_object_03.png",
        "1679",
        "872",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
Same rules apply when you create Data Objects in code: 
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"data\": \"I am sample data\",\"owner_permissions\":\"read\"}' \\\n\"https://api.syncano.io/v1.1/instances/<INSTANCE>/classes/<CLASS>/objects/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import User\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY\") \n\ndata_object = Object.please.create(\n    instane_name=\"INSTANCE_NAME\",\n  \tclass_name=\"CLASS_NAME\",\n    owner_permissions='read',\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar instance = new Syncano({\n  apiKey: API_KEY, \n  instance: INSTANCE_NAME,\n  userKey: USER_KEY\n});\n\nvar obj = {\"data\": \"I am sample data\",\"owner_permissions\":\"read\"};\n\ninstance.class('CLASS_NAME').dataobject(obj, callback());",
      "language": "javascript"
    },
    {
      "code": "ExampleObject obj = new ExampleObject();\nobj.setOwnerPermissions(DataObjectPermissions.READ);\nobj.data = \"I am sample data\";\nResponse<ExampleObject> response = obj.save();",
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