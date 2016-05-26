---
title: "Account Key"
excerpt: ""
---
Chapter sections:
1. [What is an Account Key?]()
2. [Where do I find my Account Key?]()
3. [When do I use the Account Key?]()

[block:api-header]
{
  "type": "basic",
  "title": "What is an Account Key?"
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

[block:api-header]
{
  "type": "basic",
  "title": "Where do I find my Account Key?"
}
[/block]
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

[block:api-header]
{
  "type": "basic",
  "title": "When do I use the Account Key?"
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