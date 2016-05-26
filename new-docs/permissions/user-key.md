---
title: "User Key"
excerpt: ""
---
Chapter sections:
1. [What is a User Key?]()
2. [How do I find a User Key?]()
3. [How do I use User Keys?]()
4. [How to reset a User Key]()

[block:api-header]
{
  "type": "basic",
  "title": "What is a User Key?"
}
[/block]


[block:api-header]
{
  "type": "basic",
  "title": "How do I find a User Key"
}
[/block]
### Explain that it can only be found when a user signs in

[block:api-header]
{
  "type": "basic",
  "title": "How do I use User Keys?"
}
[/block]
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
  "title": "How to reset a User Key"
}
[/block]
This is how you can reset the User Key by a call to Syncano API:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: <API_KEY>\" \\\n-H \"Content-Type: application/json\" \\\n\"https://api.syncano.io/v1.1/instances/<instance_name>/users/<user_id>/reset_key/\"",
      "language": "curl"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar User = connection.User;\n\nUser.please().resetKey({instanceName: INSTANCE_NAME, id: 7}).then(callback);",
      "language": "javascript"
    },
    {
      "code": "from syncano.models import *\nimport syncano\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nthe_user = User.please.get(\n    instance_name=\"INSTANCE_NAME\",\n    id=USER_ID\n)\n\nthe_user.reset_key()",
      "language": "python",
      "name": "Python"
    },
    {
      "code": "syncano.setUser(null);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "",
      "language": "objectivec",
      "name": ""
    },
    {
      "code": "",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
The result will be a User object with an updated `user_key` field.