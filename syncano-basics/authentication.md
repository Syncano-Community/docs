---
title: "Authentication & API Keys"
excerpt: "In this chapter you'll learn how to connect to the Syncano API with:\n- An Account Key\n- An API Key\n\nWe will also show what's the difference between Account Keys and API Keys."
---
Chapter sections:
1. [Overview](#overview)
2. [Connecting with an Account Key](#connecting-with-an-account-key)
3. [Connecting with an API Key ](#connecting-with-an-api-key)
4. [Summary](#summary) 
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
In this chapter you'll learn different methods of accessing the Syncano API. There are two ways of connecting to the Syncano API:

1. With an Account Key
2. With an API key

Each of these methods have distinct use cases that you can read about in the sections below.
[block:api-header]
{
  "type": "basic",
  "title": "Connecting with an Account Key"
}
[/block]
By using an Account Key you act as a Syncano Administrator. You can access almost every endpoint in the Syncano API (with [user endpoint](user-management#section-user-endpoint) being the only exception). What this means is that it can be used not only to create Users, Classes, Data Objects etc. but also to change your billing or account information. 
[block:callout]
{
  "type": "danger",
  "body": "Because the Account Key has such strong permissions, we strongly advise you to use [API Keys](#connecting-with-an-api-key) in your application.",
  "title": "Important!"
}
[/block]
What is also worth noting is that all the tasks (creating Instances, adding API Keys or billing info) that an Account Key is designed for, can be done from the [Syncano Dashboard](https://dashboard.syncano.io).

## Getting an Account Key
An Account Key is created when you sign up for Syncano. An administrator can have only one Account Key and it can be reset in the event of an account being compromised. It cannot be deleted. You can get your Account Key from the Syncano Dashboard or by a call to the Syncano API.

### Getting an Account Key via the Syncano Dashboard
When logged in:
1. Click on "Account" button in the top right corner
2. Click on your name
3. Go to "Authentication" tab
4. Copy the Account Key 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/B27rG07ASrKAjnQvmHyE",
        "Get_Account_Key.png",
        "1215",
        "626",
        "#233d64",
        ""
      ]
    }
  ]
}
[/block]
### Getting an Account Key via an API Call
You can get the Account Key by making a call to `/account/auth/` endpoint:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-d '{\"email\":\"YOUR_EMAIL\", \"password\":\"YOUR_PASSWORD\"}' \\\n\"https://api.syncano.io/v1.1/account/auth/\"",
      "language": "curl"
    },
    {
      "code": "connection = syncano.connect(\n    email='YOUR EMAIL',\n    password='YOUR PASSWORD'\n)\n\napi_key = connection.connection().authenticate_admin()\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano');  // CommonJS\nvar connection = Syncano();\n\nvar credentials = {\n  email: YOUR_EMAIL, \n  password: YOUR_PASSWORD\n};\n\nconnection.Account.login(credentials).then(function(user) {\n\tconsole.log('user', user);\n});",
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
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
In the response you'll get your account details along with an Account Key:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"first_name\": \"Terry\",\n    \"last_name\": \"Crews\",\n    \"is_active\": true,\n    \"account_key\": \"<ACCOUNT_KEY>\",\n    \"id\": 64,\n    \"has_password\": true,\n    \"email\": \"flexing@pectorals.com\"\n}",
      "language": "json"
    }
  ]
}
[/block]
## Account Key connection
Once you have your Account Key, you can connect to the Syncano API:
[block:code]
{
  "codes": [
    {
      "code": "# In cURL, you have to pass API Key with every call you make, so e.g. to list all your instances, you can use\n\ncurl -X GET \\\n-H \"X-API-KEY: <ACCOUNT_KEY>\" \\\n\"https://api.syncano.io/v1.1/instances/\"",
      "language": "curl"
    },
    {
      "code": "connection = syncano.connect(api_key=\"ACCOUNT_KEY\")\n",
      "language": "python"
    },
    {
      "code": "var connection = Syncano({accountKey: \"ACCOUNT_KEY\"});",
      "language": "javascript"
    },
    {
      "code": "new SyncanoBuilder().apiKey(\"YOUR ACCOUNT KEY\").instanceName(\"YOUR INSTANCE\")\n     .androidContext(context).setAsGlobalInstance(true).build();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "Syncano *syncano = [Syncano sharedInstanceWithApiKey:@\"ACCOUNT KEY\" instanceName:@\"INSTANCE NAME\"];\n\n// on iOS it doesn't matter which key you use, you have to always specify an Instance you're connecting to",
      "language": "objectivec"
    },
    {
      "code": "let syncano = Syncano.sharedInstanceWithApiKey(\"ACCOUNT KEY\", instanceName: \"INSTANCE NAME\")\n  \n//on iOS it doesn't matter which key you use, you have to always specify an Instance you're connecting to",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "api = Syncano.connect(api_key: \"<API_KEY>\")",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Connecting with an API Key"
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
 ## Creating an API Key
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
Example response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"api_key\": \"<API_KEY>\",\n    \"id\": 64,\n    \"created_at\": \"2015-05-27T15:17:39.536805Z\",\n    \"description\": \"\",\n    \"ignore_acl\": false,\n    \"allow_user_create\": false,\n    \"allow_anonymous_read\": true,\n    \"links\": {\n        \"reset_key\": \"/v1.1/instances/<instance>/api_keys/64/reset_key/\",\n        \"self\": \"/v1.1/instances/<instance>/api_keys/64/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
## API Key connection
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

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
You have learned about ways to connect to the Syncano API and how they are different from each other. You can now:
- Go to the next chapter of the Developer Manual and learn about [Classes](classes) concept.
- Check out the API Explorer for a more comprehensive list of [API Keys](http://docs.syncano.com/v0.1.1/docs/api-keys-list) and [Account](http://docs.syncano.com/v0.1.1/docs/account-details) methods