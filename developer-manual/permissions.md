---
title: "Permissions"
excerpt: "In this chapter you'll learn:\n- How to properly set up right Permissions for resources stored on Syncano \n- How to use the API and User Key combination"
---
Chapter sections:
1. [Overview](#overview)
2. [Permissions](#permissions)
3. [Permission Types](#permission-types)
4. [Using API Keys and User Keys](#using-api-keys-and-user-keys)
5. [Using API Key with allow_anonymous_read flag](#using-api-key-with-allow_anonymous_read-flag)
6. [Summary](#summary) 
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
The permissions system was designed so that you can have control over what your application users have access to on Syncano. If you create a file storage app, you'd probably want the users to see their own files and not files of other users. That's exactly what you can achieve by setting up proper permissions.

[block:api-header]
{
  "type": "basic",
  "title": "Permissions"
}
[/block]

The permissions system on Syncano is inspired by [UNIX file system permissions](http://en.wikipedia.org/wiki/File_system_permissions).


When you configure permissions, you configure them for one of three categories: 
* `owner`
* `group` 
* `other`

The owner is a category that relates only to Data Objects. When a Data Object is created we can set a User as its `owner`. 
Groups are groups of Users (you can read more about creating groups in [this chapter](groups)). 
Others will be Users that are not an `owner` and are not assigned to a `group` that is connected with a resource (Data Objects, Classes and Channels). While `owner` only applies to Data Objects, `groups` and `other` categories can also be set for Classes and Channels.

The table below should clarify the concept a bit more:
[block:parameters]
{
  "data": {
    "0-0": "`owner`",
    "0-1": "`owner_permissions`",
    "1-0": "`group`",
    "1-1": "`group_permissions`",
    "2-0": "`other`",
    "2-1": "`other_permissions`",
    "0-2": "Data Objects",
    "1-2": "Data Objects, Classes, Channels",
    "2-2": "Data Objects, Classes, Channels",
    "h-0": "Category",
    "h-1": "Property",
    "h-2": "Applies to",
    "0-3": "The owner's permissions determine what actions the owner of the Data Object can perform on the resource.",
    "1-3": "The groups permissions determine what actions a user in the group can perform on a resource.",
    "2-3": "The permissions for others indicate what action all other users can perform on the resource.",
    "h-3": "Description"
  },
  "cols": 4,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Since Account Keys (administrators) and API Keys with ignore_acl set to true can ignore permissions, `other` relates only to users (who will authenticate with API Key and User Key combination).",
  "title": "other"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Permission Types"
}
[/block]
Permission types define the level at which a user can interact with a resource.  They are represented as values that you can set for `owner_permissions`, `group_permissions` and `other_permissions` properties. These values are a bit different for:
- [Data Objects](#section-permission-types-for-data-objects-)
- [Classes](#section-permission-types-for-classes-)
- [Channels](#section-permission-types-for-channels-)

## Permission Types for Data Objects:
[block:parameters]
{
  "data": {
    "0-0": "`none`",
    "1-0": "`read`",
    "2-0": "`write`",
    "3-0": "`full`",
    "h-0": "Permission Type",
    "h-1": "Description",
    "1-1": "Grants the capability to read the Data Object.",
    "2-1": "Grants the capability to read and write the Data Object.",
    "3-1": "Grants read/write permissions plus the possibility of deleting the Data Object.",
    "0-1": "Default mode where users don't have access to the Data Object."
  },
  "cols": 2,
  "rows": 4
}
[/block]
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

[block:callout]
{
  "type": "warning",
  "body": "If a Data Object has `other_permissions` = `full`, it overrides all other permissions completely. So even if a user is not an `owner` and not in a  `group` connected with the Data Object, the user will still have access to a Data Object where `other_permissions` = `full`. Same rule applies to Classes and Channels."
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
## Permission Types for Classes:
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
### Example

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
## Permission Types for Channels:
[block:parameters]
{
  "data": {
    "h-0": "Permission Type",
    "h-1": "Description",
    "0-0": "`none`",
    "1-0": "`subscribe`",
    "2-0": "`publish`",
    "0-1": "Default mode where users don't have access to channel notifications.",
    "1-1": "User can receive notifications from a channel.",
    "2-1": "User can publish data to a channel."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Channels and their permissions are described in depth in the [Realtime Communication](realtime-communication) chapter of this manual."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using API Keys and User Keys"
}
[/block]
Creating API Keys was described in our [Getting Started section](getting-started-with-syncano#getting-an-api-key) and you can read a bit more about User Keys in the [User management chapter](user-management#user-authentication). When it comes to permissions, it is important to know that there are two flags that can be set when creating an API Key:
* `allow_user_create` - API Key with this flag set to `true` works on the instances/<instance>/users/ endpoint and enables user creation (you can read about adding new users [here](user-management#adding-a-user)).
* `ignore_acl` - API Key with this flag set to `true` will ignore access control list. What this means is that it will ignore any permissions set for the resources in Syncano. 
* [`allow_anonymous_usage`](#using-api-key-with-allow_anonymous_read-flag) - When set to `true`, API Key with `allow_anonymous_read` will allow making GET requests to Classes and Data Objects based on appropriate permissions.
[block:callout]
{
  "type": "info",
  "title": "ignore_acl",
  "body": "For example a User that will use a combination of a User Key and an API Key with `ignore_acl` = true will be able to view all Data Objects in a Class where normally permissions would prohibit that user to have read access. The user could also create Data Objects in a Class where this Class doesn't have permission type `create_objects` etc. So, for instance, `ignore_acl` API Keys might be useful if you'd like to have Admin/Moderator Users. You should be careful though because such users would have large control within an instance along with the ability to create/delete Classes."
}
[/block]
Depending on what you'd like to achieve in your app, you can either use an API Key or a combination of an API Key and a User Key. 

## Using an API Key
An API Key without `ignore_acl` or `create_user` flags has a rather limited access and mostly can only be used to read data from different endpoints. 

This is how specific API Key permissions per endpoint look like (endpoints that API Key doesn't have permission to view were omitted):
[block:parameters]
{
  "data": {
    "h-0": "Endpoint",
    "h-1": "Permission",
    "0-0": "/instances/",
    "1-0": "/api_keys/",
    "2-0": "/user/auth/",
    "3-0": "/users/",
    "4-0": "/groups/",
    "5-0": "/classes/",
    "6-0": "/classes/<class>/objects/",
    "7-0": "/channels/",
    "8-0": "/channels/<id>/publish/",
    "9-0": "/channels/<id>/history/",
    "9-1": "`read`",
    "8-1": "`write`",
    "7-1": "`read`",
    "6-1": "`create`",
    "5-1": "`read`",
    "4-1": "`read`",
    "3-1": "",
    "1-1": "`read`",
    "0-1": "`read`",
    "0-2": "Can view only the instance that the API Key belongs to.",
    "1-2": "Can view only own API Key (use account_key or the Syncano Dashboard to view full list of API Keys)",
    "3-2": "No read permission - not possible to list all the users. It's possible to add new users when API Key has `allow_user_create` = true.",
    "5-2": "Can view classes when class permission types are set to `read` or `create_objects`.",
    "6-2": "- Can create new objects inside a class when the class has `create_objects` permission set. \n- Can read/write/update/delete objects depending on specific Data Object owner/group/other permissions + also requires class permission types to be `read` or `create_objects`.",
    "7-2": "Can view channels if channel permission types are set to `subscribe` or `publish`.",
    "8-2": "Can `write` to a channel if channel permission types are set to `publish`.",
    "9-2": "Can `read` a channel if channel permissions are `subscribe` or `publish`.",
    "h-2": "Description",
    "2-2": "Endpoint where user can POST their user credentials (log in using username and password) and get User Key in response.",
    "4-2": "Can view groups."
  },
  "cols": 3,
  "rows": 10
}
[/block]
## Using an API Key + User Key

In most of the cases your users should be connecting to your app with a combination of User Key and API Key. If that's the case, then some endpoints will behave differently and will be showing data that is scoped for the current user. You'll find a list of these endpoints below:
[block:parameters]
{
  "data": {
    "0-0": "/user/",
    "0-1": "`write`",
    "0-2": "Endpoint available only with API Key + User Key (Account Key + User Key won't work here!). A User can view only their account details and change their username and/or password.",
    "1-0": "/users/",
    "1-2": "A User can view only their account details on the list.",
    "1-1": "`write`",
    "3-0": "/users/<id>/groups/",
    "3-1": "`read`",
    "3-2": "A User can see only these Groups that he is a part of.",
    "4-1": "`read`",
    "4-0": "/groups/<id>/users/",
    "4-2": "A User can only see themself being a part of a Group.",
    "h-0": "Endpoint",
    "h-1": "Permission",
    "h-2": "Description",
    "2-0": "/users/<user_id>/",
    "2-1": "`write`",
    "2-2": "A User can view their account details and change their username and/or password."
  },
  "cols": 3,
  "rows": 5
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "The Rest of the endpoints will behave in the same manner as if accessed only with an API Key."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using API Key with allow_anonymous_read flag"
}
[/block]
When set to `true`, API Key with allow_anonymous_read will allow making GET requests to Classes and Data Objects *provided that*:
- Data Objects have `other_permissions` set to `read`, `write` or `full`
- Classes have `other_permissions` set to `read` or `create_objects`
[block:callout]
{
  "type": "info",
  "body": "Please refer to the permissions section [above](#permissions) if you want to learn how to add permissions to Classes and Data Objects."
}
[/block]
What is important here is that this key will have *read* access to all the resources with appropriate permissions without the necessity to pass a User API Key along with it. It might come handy in cases where you'd like some of your apps resources to be accessible without the need for user authentication.

### Adding an API Key with allow_anonymous_read flag

This is how you can create allow_anonymous_read API Key:

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"allow_anonymous_read\": true}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE/api_keys/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\nsyncano.connect(api_key='API_KEY')\n\napi_key = ApiKey.please.create(allow_anonymous_read=True)\n",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
    },
    {
      "code": "// Not supported",
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
Response (some properties omitted for clarity):

[block:code]
{
  "codes": [
    {
      "code": "{\n    \"allow_anonymous_read\": true,\n    \"allow_user_create\": false,\n    \"api_key\": \"cc308b5a686e1ad3e7290717e2b36e11193184d5\",\n    \"id\": 2581,\n    \"ignore_acl\": false\n}",
      "language": "curl"
    },
    {
      "code": "# RAW RESPONSE\n{\n  'description': u'', \n  'links': <syncano.models.fields.LinksWrapper object at 0x7f04094e55d0>,   \n  'allow_anonymous_read': True, \n  'allow_user_create': False, \n  'instance_name': 'INSTANCE_NAME', \n  'api_key': 'API_KEY', \n  'id': ID, \n  'ignore_acl': False\n}\n\n# CODE:\nprint(api_key.allow_anonymous_read)\n>> True",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
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

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
That's all there is to know about permissions on Syncano Platform. To sum it up:
- You can give to an `owner`, `group` or `other` categories, different sets of permissions, by assigning them adequate permission types.
- An API Key with the `ignore_acl` flag set to `true` will ignore all the permissions.
- Your users will be authenticating with API Key and User Key combinations. Some of the endpoints will be scoped to only view data for that particular user.

Since you've learned how to set up permissions on Syncano, you might like to read about organizing users into [Groups](groups). If you already know how to do that, you may be interested in how [realtime communication](realtime-communication) works.