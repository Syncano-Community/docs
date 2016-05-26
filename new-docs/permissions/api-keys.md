---
title: "API Key"
excerpt: ""
---
Chapter sections:
1. [What is an API Key?]()
2. [Where do I find API Keys?]()
3. [Creating an API Key]()
4. [API Key Flags and how to use them]()
5. [When do I use API Keys?]()

[block:api-header]
{
  "type": "basic",
  "title": "What is an API Key?"
}
[/block]
You can use an Account Key to perform different operations on your Syncano account, including edit it. 
API Keys have a different role. Their main purpose is to serve as an authentication method for an application that you intend to build **ON** Syncano. Because of that there are limitations in what can be achieved with the usage of an API Key. Here's a table that lists some of the major differences between Account Keys and API Keys:
[block:parameters]
{
  "data": {
    "h-0": "Subject",
    "h-1": "Account Key",
    "h-2": "API Key",
    "0-0": "Limit",
    "0-1": "One Account Key per Administrator.",
    "0-2": "No limit.",
    "1-0": "Creation",
    "1-1": "An Account key is created when an Administrator signs up for Syncano. It can be reset by an Administrator but cannot be deleted.",
    "1-2": "Administrators can create/reset/delete API Keys.",
    "2-0": "Permissions",
    "2-1": "Full access to account, billing metrics data and every instance that an Administrator created. The only exception is a `/user/` endpoint (more on that in [User management](user-management) chapter )",
    "2-2": "No access to account, billing or metrics data. Regular API Key can only view the data of an Instance that it was created in. It is possible to create/delete Data Objects with the API Key if proper [permissions](permissions) are configured.",
    "3-0": "Scripts",
    "3-1": "Can run/create/edit/delete/ Scripts",
    "3-2": "No access to Scripts"
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Where do I find API Keys?"
}
[/block]
To add an API Key (while being in the instance menu):
1. Click API Keys menu
2. Click "Add API Key button on the top right
3. Add description
4. Choose appropriate flags
5. Click "Confirm" 

See the screenshots below for details.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/vsaYitLhTIGPbHnikMca",
        "Add_API_Key_01.png",
        "1279",
        "656",
        "#2e4c7a",
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
        "https://www.filepicker.io/api/file/TMj93VoyRGu9PDq8gRHI",
        "Add_API_Key_02.png",
        "1279",
        "656",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating an API Key"
}
[/block]
If you'd like to connect to the Syncano API via an API Key, you should create it first. Again, you can do it either via the Syncano Dashboard or an API call:
[block:callout]
{
  "type": "info",
  "title": "API Key flags",
  "body": "What is worth mentioning here is that API Keys can have three flags. Their usage is out of scope of this chapter so we are only giving a brief intro here:\n- `ignore_acl` - API Key with this flag set to `true` will ignore permissions set for instance resources. You can read more about `ignore_acl` keys in the [Permissions](permissions#using-api-keys-and-user-keys) chapter.\n- `allow_user_create` - API Key with this flag set to `true` can be used to create Users (it is not possible with a standard API Key). You can read more about User creation in the [User management](user-management#creating-an-api-key-with-user-creation-permissions) chapter.\n- `allow_anonymous_read` - When this flag is set to `true` an API Key can read Classes and Data Objects with `other_permissions` set to `read` or higher. You can read more about this key in the [Permissions chapter](permissions#using-api-key-with-allow_anonymous_read-flag)"
}
[/block]

### Creating an API Key via the Syncano Dashboard
To add an API Key (while being in the instance menu):
1. Click API Keys menu
2. Click "Add API Key button on the top right
3. Add description
4. Choose appropriate flags
5. Click "Confirm" 

See the screenshots below for details.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/vsaYitLhTIGPbHnikMca",
        "Add_API_Key_01.png",
        "1279",
        "656",
        "#2e4c7a",
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
        "https://www.filepicker.io/api/file/TMj93VoyRGu9PDq8gRHI",
        "Add_API_Key_02.png",
        "1279",
        "656",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
### Creating an API Key via a call to the Syncano API

This is how you can create an API Key:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/api_keys/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\nsyncano.connect(api_key=\"API_KEY\")\n\napi_key_instance = ApiKey.please.create(instance_name=\"INSTANCE_NAME\")\napi_key = api_key_instance.api_key",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar connection = new Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar ApiKey = connection.ApiKey;\n\nApiKey.please().create({instanceName: 'INSTANCE_NAME'}).then(callback);",
      "language": "javascript"
    },
    {
      "code": "// Not supported in Android Library",
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
      "code": "api = Syncano.connect(api_key: \"<API_KEY>\")\napi_key = api.api_keys.create",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "API Key Flags and how to use them"
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

## Anonymous Read Flag

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
  "title": "When do I use API Keys?"
}
[/block]
Now, when you have an API Key, you can connect to the Syncano API. You'll need to provide an `API Key` and an `instance name` in order to connect using this method:
[block:code]
{
  "codes": [
    {
      "code": "# In cURL, you have to pass API Key with every call you make, so e.g. to list instances associated with API Key, you can use\n\ncurl -X GET \\\n-H \"X-API-KEY: <API_KEY>\" \\\n\"https://api.syncano.io/v1.1/instances/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\n\nconnection = syncano.connect(\n  instance_name='INSTANCE_NAME', \n  api_key=\"API_KEY\")\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar connection = new Syncano({apiKey: \"API_KEY\"});",
      "language": "javascript"
    },
    {
      "code": "new SyncanoBuilder().apiKey(\"YOUR API KEY\").instanceName(\"YOUR INSTANCE\")\n     .androidContext(context).setAsGlobalInstance(true).build();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "Syncano *syncano = [Syncano sharedInstanceWithApiKey:@\"API KEY\" instanceName:@\"INSTANCE NAME\"];",
      "language": "objectivec"
    },
    {
      "code": "let syncano = Syncano.sharedInstanceWithApiKey(\"API KEY\", instanceName: \"INSTANCE NAME\")",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "api = Syncano.connect(api_key: \"MY_API_KEY\")\ninstance = api.instances.find(\"myinstance\")",
      "language": "ruby"
    }
  ]
}
[/block]