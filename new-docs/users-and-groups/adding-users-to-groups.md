---
title: "Adding Users to Groups"
excerpt: ""
---
Chapter sections:
1. [Overview]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Now, when we have a group created, we can start adding the users there. 
[block:callout]
{
  "type": "info",
  "body": "This example assumes that there are existing users on your Instance. If you'd like to know how to add users to your application, go [here](user-management)."
}
[/block]
Click 'more' icon on chosen user.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/eIL2dhcrQPqU6xBhy6gW",
        "Add_group_to_user_01.png",
        "1679",
        "771",
        "#254472",
        ""
      ]
    }
  ]
}
[/block]
Click 'Edit a User' option.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/NYldtRcARYqw57oLuIIP",
        "Add_group_to_user_02.png",
        "1679",
        "771",
        "#254473",
        ""
      ]
    }
  ]
}
[/block]
On the `Edit` pop-up, select a Group from the drop-down menu.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/YtdgTs4dTHCLmRzRtJB9",
        "Add_group_to_user_03.png",
        "1679",
        "768",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
Selected Group will show up in the `Edit` window. Click `Confirm` to save changes.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Z5ndRWDxRmelj7TcJ3zv",
        "Add_group_to_user_04.png",
        "1679",
        "771",
        "#2a3e5b",
        ""
      ]
    }
  ]
}
[/block]
You will also see Groups user belongs to, on the users list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/4H9pJOMwQMOijLQL1r3q",
        "Add_group_to_user_05.png",
        "1622",
        "842",
        "#254573",
        ""
      ]
    }
  ]
}
[/block]
Call to add a group to a user can look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"user\": user_id}' \\\n\"https://api.syncano.io/v1.1/instances/instance/groups/group_id/users/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import User\nimport syncano\n\nsyncano.connect(api_key=\"API_KEY_WITH_ALLOW_USER_CREATE_FLAG\") \n\nGordon = User.please.create(\n    instance_name=\"INSTANCE_NAME\",\n    username=\"Gordon Freeman\",\n    password=\"Black Mesa\"\n)\n\ngroup = Group.objects.get(id=1)\nGordon.add_to_group(group_id=group.id)\n\n# or:\n\ngroup.add_user(user_id=Gordon.id)",
      "language": "python"
    },
    {
      "code": "// Planned for v.1.1.0",
      "language": "javascript"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "Response<GroupMembership> response = syncano.addUserToGroup(group.getId(), user.getId()).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
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