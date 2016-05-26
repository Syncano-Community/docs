---
title: "User Authentication"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Simple Authentication]()
3. [Social Login]()
4. [Resetting a User Key]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]


[block:api-header]
{
  "type": "basic",
  "title": "Simple Authentication"
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "To authenticate a user, you'll need to make a call to the `/user/` endpoint. This endpoint is unique in terms of authentication because to get or update the user details you will need to use a combination of an api_key and user_key.\n\naccount_key will not work in this endpoint!",
  "title": "Important!"
}
[/block]
User authentication is used when we want our users to be able to login to our application. For a successful User auth call we need user credentials (login and password) and an API Key. Here is an example of such call:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"username\":\"Gordon Freeman\",\"password\":\"Black mesa\"}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/user/auth/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\n\nsyncano.connect(\n    api_key=\"API_KEY\",\n    instance_name=\"INSTANCE_NAME\",\n    username=\"Gordon Freeman\",\n    password=\"Black Mesa\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({apiKey: \"API_KEY\"});\nvar User = connection.User;\n\nvar query = {instanceName: \"INSTANCE_NAME\"};\nvar credentials = {email: \"EMAIL\", password: \"PASSWORD\"};\n\nUser.please().login(query, credentials).then(callback);",
      "language": "javascript"
    },
    {
      "code": "User plainUser = new User(userName, password);\n\nResponse<User> response = plainUser.on(userSyncano).login();\n// All next requests will be used using apiKey and userKey.",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"instance_name\"];\n[SCUser loginWithUsername:username password:password completion:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "Syncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"instance_name\")\nSCUser.loginWithUsername(\"username\", password: \"password\") { error in\n  //handle error\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
User login call returns `user_key` in the response:
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

[block:callout]
{
  "type": "success",
  "body": "`user_key` should be used along with `api_key` in all subsequent calls on the client side. So i.e. a call where a user is creating a Data Object would look like this:"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\": \"I am an object name\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/class/objects/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Object\nimport syncano\n\nsyncano.connect(\n    api_key=\"API_KEY\",\n    user_key=\"USER_KEY\",\n    instance_name=\"INSTANCE_NAME\"\n)\n\nObject.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\",\n    name=\"I am a data object\"\n)\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({apiKey: \"API_KEY\", userKey: \"USER_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar object = {\n  instanceName: \"INSTANCE_NAME\", \n  className: \"CLASS_NAME\",\n\tname: \"I am an object name\"\n};\n\nDataObject.please().create(object).then(callback);",
      "language": "javascript"
    },
    {
      "code": "syncano.setUserKey(userKey);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// In iOS you don't need to extract User Key manually - after logging in it will be used automatically\n\n[Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"instance_name\"];\n[SCUser loginWithUsername:username password:password completion:^(NSError *error) {\n  Book *book = [Book new];\n  book.title = @\"The Last Wish\";\n  //this call will be made with User Key\n  [book saveWithCompletionBlock:^(NSError *error) {\n  }];\n}];",
      "language": "objectivec"
    },
    {
      "code": "// In iOS you don't need to extract User Key manually - after logging in it will be used automatically\n\nSyncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"instance_name\")\nSCUser.loginWithUsername(\"username\", password: \"password\") { error in\n  //handle error\n  let book = Book()\n  book.title = \"The Last Wish\"\n  //this call will be made with User Key\n  book.saveWithCompletionBlock { error in\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
The `user_key` can be used for a prolonged period of time, because it expires only in the event of user changing his password. This way it is possible to store it within your application without the need to store the user’s password itself.

[block:api-header]
{
  "type": "basic",
  "title": "Social Login"
}
[/block]
With syncano you can allow a user to sign in with Facebook or Google.

If a user signs up for the first time, their account in syncano will be automatically created, if the user already has an account from this social service, the user will just log in.

The end effect is that, if you provide a valid authorization token, you'll get user data in the same format as you would get with a traditional username/password login.

To use social login, you first have to set up Oauth2 apps in facebook or google.

Then you have to implement it on the client side, for example by using [hello.js](http://adodson.com/hello.js/). You don't have to pass any secret keys or secret ids to syncano, just an authorization token.

You can log in by posting:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\"access_token\": \"BACKEND_PROVIDER_TOKEN\"}' \\\n\"https://api.syncano.io/v1.1/instances/<instance>/user/auth/BACKEND_NAME/\"",
      "language": "curl"
    },
    {
      "code": "# Coming soon!",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar User = connection.User;\n\nvar query = {instanceName: \"INSTANCE_NAME\", backend: \"facebook\"};\nvar credentials = {access_token: \"TOKEN\"};\n\nUser.please().socialLogin(query, credentials).then(callback);",
      "language": "javascript"
    },
    {
      "code": "User user = new User(SocialAuthBackend.FACEBOOK, socialNetworkAuthToken);\nResponse<User> response = user.loginSocialUser();",
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
Backend names:

- facebook
- google-oauth2
- twitter
- linkedin
[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For more details about the available methods for users, visit the [Users API reference](http://docs.syncano.com/v0.1.1/docs/users-list) and [User API reference](http://docs.syncano.com/v0.1.1/docs/user-log-in)."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Resetting a User Key"
}
[/block]
### When to reset a User Key
Usually you would like to reset a User Key in two situations:
- When the User Key was compromised 
- Username/password combination was compromised in some way. 

The API Key, in most cases is shared publicly, but it's a safe solution - since API Key (with the flag `ignore_acl` set to `false`) cannot modify any User data without a User Key passed together with it. When a User Key gets compromised someone could use it together with publicly known API Key to change/steal user data.

### Why reset a User Key when username/password combination is leaked? 
Because by knowing username and password, one can obtain User Key through [Users - Log In](http://docs.syncano.com/v0.1.1/docs/user-log-in) endpoint, making it possible to modify/steal user data.

If for whatever reason your users username, password, or User Key gets leaked ask them to change their password and reset their User Key right away!

### How to reset the User Key:
## Link to permissions here