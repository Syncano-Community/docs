---
title: "User Profile"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Creating the User Profile Schema]()
3. [Creating a User with a Profile]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
As you may have noticed when browsing through the API or Admin GUI, there's an extra default Class sitting in each Instance that you create. This Class, called "user_profile", stores Data Objects, which are the profiles of your Users. So, whenever a new User is added, a special "profile" Data Object is being created. The user_profile Class and its Data Objects profiles are protected, which means that no one can delete the user_profile Class, create new user profiles manually, delete profiles, or change ownership of the profile (user is always their profile owner).

### Why should I care?

Excellent question! Since user profiles are just Data Objects stored in a Class, you can simply change this Class Schema to add custom fields.

[block:api-header]
{
  "type": "basic",
  "title": "Creating the User Profile Schema"
}
[/block]
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
Now a user has an extra `avatar` field in their user profile, which the user can update with an avatar picture:

[block:api-header]
{
  "type": "basic",
  "title": "Creating a User with a Profile"
}
[/block]

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
And voila! The User profile just got a new shiny avatar image