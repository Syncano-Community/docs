---
title: "User management"
excerpt: "This section describes management for your application users. After reading it, you will know how to add, register and authenticate users."
---
1. [Creating an API Key with user creation permissions](#creating-an-api-key-with-user-creation-permissions)
2. [Adding a User](#adding-a-user)
3. [User Profiles](#user-profiles)
4. [Resetting a User Key](#resetting-a-user-key) 
5. [User authentication](#user-authentication)
6. [Social Login](#social-login)
[block:api-header]
{
  "type": "basic",
  "title": "Creating an API Key with user creation permissions"
}
[/block]
To be able to register Users you'll have to create an API Key that has `allow_user_create` flag set to `true` (It is possible to create new Users with the Account Key but it's not the recommended solution). 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/9dso6mq4RPuupCE7rNfm",
        "Add_API_Key_01.png",
        "1277",
        "665",
        "#27446f",
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
        "https://www.filepicker.io/api/file/I2grQlJQWaG1PW2tsJzG",
        "Add_API_Key_02.png",
        "1679",
        "872",
        "#849cb4",
        ""
      ]
    }
  ]
}
[/block]
Before confirming, in **step 5.**, please change the switch state in `User registration?` line - this way this API Key will be able to add new users to your Syncano Instance.

This is an example of such API Key creation call:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"allow_user_create\":true}' \\\n\"https://api.syncano.io/v1.1/instances/instance/api_keys/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import ApiKey\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY\")\n\nnew_key = ApiKey.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    allow_user_create=True\n)\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar ApiKey = connection.ApiKey;\n\nvar options = {\n  allow_user_create: true, \n  instanceName: \"INSTANCE_NAME\"\n};\n\nApiKey.please().create(options).then(callback);",
      "language": "javascript"
    },
    {
      "code": "// Not supported in Android Library - please use Dashboard instead.",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead.",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead.",
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

Example response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 819, \n    \"description\": \"\", \n    \"api_key\": \"78997d74c0f71141e489a3284fb4f2e464abdc19\", \n    \"ignore_acl\": false, \n    \"allow_user_create\": true, \n    \"created_at\": \"2015-04-21T14:19:41.309520Z\", \n    \"links\": {\n        \"self\": \"/v1.1/instances/<instance>/api_keys/819/\", \n        \"reset_key\": \"/v1.1/instances/<instance>/api_keys/819/reset_key/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Now we can utilize this API Key in the `User add` API call:
[block:api-header]
{
  "type": "basic",
  "title": "Adding a User"
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
Example response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 66, \n    \"username\": \"Gordon Freeman\", \n    \"user_key\": \"33b966b883351528e4ee3206cd84bd42fcf2d4a4\", \n    \"profile\": {\n        \"id\": 1198, \n        \"created_at\": \"2015-03-25T14:01:31.962783Z\", \n        \"updated_at\": \"2015-03-25T14:01:31.962810Z\", \n        \"revision\": 1, \n        \"owner\": 66, \n        \"owner_permissions\": \"full\", \n        \"group\": null, \n        \"group_permissions\": \"none\", \n        \"other_permissions\": \"none\", \n        \"channel\": null, \n        \"channel_room\": null, \n        \"links\": {\n            \"owner\": \"/v1.1/instances/<instance>/users/66/\", \n            \"self\": \"/v1.1/instances/<instance>/classes/user_profile/objects/1198/\"\n        }\n    }, \n    \"links\": {\n        \"self\": \"/v1.1/instances/<instance>/users/66/\", \n        \"reset-key\": \"/v1.1/instances/<instance>/users/66/reset_key/\", \n        \"groups\": \"/v1.1/instances/<instance>/users/66/groups/\"\n    }\n}",
      "language": "json"
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
  "title": "User Profiles"
}
[/block]
As you may have noticed when browsing through the API or Admin GUI, there's an extra default Class sitting in each Instance that you create. This Class, called "user_profile", stores Data Objects, which are the profiles of your Users. So, whenever a new User is added, a special "profile" Data Object is being created. The user_profile Class and its Data Objects profiles are protected, which means that no one can delete the user_profile Class, create new user profiles manually, delete profiles, or change ownership of the profile (user is always their profile owner).

### Why should I care?

Excellent question! Since user profiles are just Data Objects stored in a Class, you can simply change this Class Schema to add custom fields.

### Example

Let's say you'd like your users to have the ability to add profile pictures. What we'll do, is change the 'user_profile' Class to have an 'avatar' field.
[block:callout]
{
  "type": "info",
  "body": "If you'd like a reminder of what Class or Class Schema is, please see [this chapter](doc:classes)."
}
[/block]
1. Go to Classes Tab in the [Dashboard](https://dashboard.syncano.io). As you see, `user_profile` class is already there, no need to create it. To edit this class, click on dropdown icon.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/DMp8N1YlRgWRL2ROaBmz",
        "Edit_user_profile_01.png",
        "1679",
        "767",
        "#264471",
        ""
      ]
    }
  ]
}
[/block]
Choose `Edit a Class` option from the dropdown.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/EQhALFUNRKuzhPVAATaI",
        "Edit_user_profile_02.png",
        "1679",
        "770",
        "#254473",
        ""
      ]
    }
  ]
}
[/block]
Enter new field name and type (in our example it would be `avatar` field of type `file`) and click `ADD` next to it. Press `Update` to save changes.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/DoTRODJyTk6sT6GVBnR0",
        "Edit_user_profile_04.png",
        "1679",
        "773",
        "#25436f",
        ""
      ]
    }
  ]
}
[/block]
You can make same kind of changes to `user_profile` class, using our API:
[block:code]
{
  "codes": [
    {
      "code": "curl -X PATCH \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d schema='[{\"name\":\"avatar\",\"type\":\"file\"}]' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/user_profile/\"",
      "language": "curl"
    },
    {
      "code": "import syncano \n\nconnection = syncano.connect('ACCOUNT_KEY')\n\nClass = connection.Class\n\nuser_profile_class = Class.please.get(\n    instance_name='instance_name',\n    name='user_profile'\n)\n\nuser_profile_class.schema.add({\"name\": \"avatar\", \"type\": \"file\"})\nuser_profile_class.save()",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Class = connection.Class;\n\nvar query = {\n\tinstanceName: \"INSTANCE_NAME\",\n  className: \"user_profile\"\n};\n\nvar update = {\n\tschema : [{\"name\":\"avatar\",\"type\":\"file\"}]\n};\n\nClass.please().update(query, update).then(callback);",
      "language": "javascript"
    },
    {
      "code": "// MyUserProfile should extend Profile class\nResponse<SyncanoClass> response = syncano.updateSyncanoClass(MyUserProfile.class).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// You cannot change class schema from iOS, but it's really easy to do using our Dashboard!",
      "language": "objectivec"
    },
    {
      "code": "// You cannot change class schema from iOS, but it's really easy to do using our Dashboard!",
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
Now a user has an extra `avatar` field in their user profile, which the user can update with an avatar picture:
[block:code]
{
  "codes": [
    {
      "code": "curl -X PATCH \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\n-H \"Content-type:multipart/form-data\" \\\n-F name=\"avatar\" \\\n-F filename=@user_avatar_file \\\n-k \"https://api.syncano.io/v1.1/instances/instance/classes/user_profile/objects/4328/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nimage_path = \"/sample_path/user_avatar.jpg\"\navatar= open(image_path, \"r\")\n\nObject.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    class_name=\"CLASS_NAME\",\n    avatar=avatar\n)",
      "language": "python"
    },
    {
      "code": "var fs = require(\"fs\");\nvar Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar object = {\n  avatar: Syncano.file(fs.readFileSync('/sample_path/user_avatar.jpg')),\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_NAME\"\n};\n\nDataObject.please().create(object).then(callback);",
      "language": "javascript"
    },
    {
      "code": "MyUserProfile profile = user.getProfile();\nprofile.avatar = new SyncanoFile(new File(getAssetsDir(), \"blue.png\"));\nResponse<MyUserProfile> responseAvatar = profile.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "@interface MyUserProfile : SCUserProfile\n@property (nonatomic, strong) SCFile *avatar;\n@end\n\n@interface MyUser : SCUser\n@property (nonatomic,retain) MyUserProfile *profile;\n@end\n\n@implementation MyUserProfile\n@end\n  \n@implementation MyUser\n@dynamic profile;\n@end\n  \n[Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"instance_name\"];\n//call this once, before you interact with Syncano in any way\n[MyUser registerClassWithProfileClass:[MyUserProfile class]];\n//now it's safe to use custom profiles\nMyUser *currentUser = [MyUser currentUser];\nMyUserProfile *profile = currentUser.profile;\nprofile.avatar = [SCFile fileWithaData:UIImageJPEGRepresentation([UIImage imageNamed:@\"imageName\"], 0.7)];\n[profile saveWithCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "class MyUserProfile : SCUserProfile {\n    var avatar : SCFile? = nil;\n}\n\nclass MyUser : SCUser {\n    var myProfile : MyUserProfile? {\n        get {\n            return profile as? MyUserProfile\n        }\n        set {\n            profile = newValue\n        }\n    }\n}\n\nSyncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"instance_name\")\n//call this once, before you interact with Syncano in any way\nMyUser.registerClassWithProfileClass(MyUserProfile.self)\n//now it's safe to use custom profiles\nlet currentUser = MyUser.currentUser()\nlet profile = currentUser.myProfile\nprofile?.avatar = SCFile(withaData: UIImageJPEGRepresentation(UIImage(named: \"imageName\")!, 0.7))\nprofile?.saveWithCompletionBlock() { error in\n  //handle error\n}",
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
And voila! The User profile just got a new shiny avatar image:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 4328, \n    \"created_at\": \"2015-04-21T11:31:54.249412Z\", \n    \"updated_at\": \"2015-04-23T14:11:26.603282Z\", \n    \"revision\": 7, \n    \"owner\": 1429, \n    \"owner_permissions\": \"none\", \n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"none\", \n    \"channel\": null, \n    \"channel_room\": null, \n    \"avatar\": {\n        \"type\": \"file\", \n        \"value\": \"https://api.syncano.io.s3.amazonaws.com/1/5/7aecf4b79f8f033caf9cc2619ee4d14e6bda8832.jpg\"\n    }, \n    \"links\": {...}\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "User authentication"
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