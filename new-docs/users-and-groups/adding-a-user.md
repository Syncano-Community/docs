---
title: "Adding a User"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Adding a User in the Dashboard]()
3. [Adding a User with Libraries]()
4. [Adding a profile for a User]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This section describes management for your application users. After reading it, you will know how to add, register and authenticate users.

[block:api-header]
{
  "type": "basic",
  "title": "Adding a User in the Dashboard"
}
[/block]
Once we create the API Key with `allow_user_create` flag set to true (mentioned in a previous [section](#creating-an-api-key-with-user-creation-permissions)) adding new users is pretty straightforward. 

Go to `Users & Groups` tab in the Dashboard's Navigation Bar.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/sjqLfgzkT3uZQKkqbUbl",
        "Add_user_01.png",
        "1679",
        "736",
        "#254472",
        ""
      ]
    }
  ]
}
[/block]
When you click `Create a User` button, you will see a pop-up. Type there new user `username` and `password` and press `Confirm` to save.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/18aYANllSMGYJBOuD7Qg",
        "Add_user_02.png",
        "1679",
        "773",
        "#254474",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Adding a User with Libraries"
}
[/block]
For adding new user in code, please see the example below:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY_WITH_ALLOW_USER_CREATE_FLAG\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"username\":\"Gordon Freeman\",\"password\":\"Black Mesa\"}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/users/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import User\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY_WITH_ALLOW_USER_CREATE_FLAG\") \n# You can use an account key as well\n\nGordon = User.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    username=\"Gordon Freeman\",\n    password=\"Black Mesa\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({apiKey: API_KEY_WITH_ALLOW_USER_CREATE_FLAG});\nvar User = connection.User;\n\nvar options = {\n  username: USERNAME, \n  password: PASSWORD,\n  instanceName: INSTANCE_NAME\n};\n\nUser.please().create(options).then(callback);",
      "language": "javascript"
    },
    {
      "code": "User newUser = new User(userName, password);\nResponse<User> response = newUser.register();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"instance_name\"];\n[SCUser registerWithUsername:username password:password completion:^(NSError *error) {\n      //handle user registration\n}];",
      "language": "objectivec"
    },
    {
      "code": "Syncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"instance_name\")\nSCUser.registerWithUsername(username, password: password) { error in\n  //handle error\n}",
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
Whoa! Quite a lot of info in this User object! Let's break it down, so that it's a bit easier to digest. If we omit the `profile` property(we'll get back to it in a moment), this is how User object looks like:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 66, \n    \"username\": \"Gordon Freeman\", \n    \"user_key\": \"33b966b883351528e4ee3206cd84bd42fcf2d4a4\", \n    \"profile\": {...}, \n    \"links\": {...}\n}",
      "language": "json"
    }
  ]
}
[/block]
Ok, so this is a bit simpler. What we've got here is:
[block:parameters]
{
  "data": {
    "0-0": "id",
    "h-0": "Property",
    "h-1": "Description",
    "0-1": "User id",
    "1-0": "username",
    "1-1": "Name of the user that was provided during registration",
    "2-0": "user_key",
    "2-1": "User authentication key. Should be used when making calls to the `/user` endpoint",
    "3-0": "profile",
    "3-1": "User profile object",
    "4-0": "links",
    "4-1": "HATEOAS links to connected resources"
  },
  "cols": 2,
  "rows": 5
}
[/block]
At this point you might probably ask "Ok, wait a sec! What exactly is this `profile` thing?". We'll explain it in the next section which is called (you'll never guess)...

[block:api-header]
{
  "type": "basic",
  "title": "Adding a profile for a User"
}
[/block]