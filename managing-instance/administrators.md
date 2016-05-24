---
title: "Administrators"
excerpt: "Who are Syncano Admins?"
---
### Chapter sections:

1. [Overview](#overview)
2. [Listing Administrators](#listing-administrators)
3. [Inviting Administrators](#inviting-administrators)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
**Administrator** accounts are used for accessing the Syncano Admin Panel of your Instance. Each administrator has the following properties:
+ E-mail (required)
+ First and last name (optional)
+ Role

Administrators can perform different tasks based on their roles. Roles allow you to control user access of your application. Each role has different permissions:
[block:parameters]
{
  "data": {
    "h-0": "Roll",
    "h-1": "Description",
    "0-0": "Full",
    "1-0": "Write",
    "2-0": "Read",
    "1-1": "Read/write access to all the instance data.",
    "2-1": "Permission to access all instance data in read-only mode.",
    "0-1": "Read/write access to all the instance data. Can add administrators."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Listing Administrators"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/XuvL09g6R7SMjCwGXtuw",
        "Admins_list.png",
        "1679",
        "856",
        "#264470",
        ""
      ]
    }
  ]
}
[/block]
If you follow steps listed above, you will see Admins list, with Owner of the Instance on top.
[block:api-header]
{
  "type": "basic",
  "title": "Inviting Administrators"
}
[/block]
To invite an administrator, go to Admins listing, as shown above, and click "+" button in the lower right corner. 
In the pop up window, fill in the `Email` field with an email of a person you would like to invite. Select the appropriate role and click **Confirm** button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/BMwj2BFIT6eDi7LWmh3Q",
        "invite_admin_01.png",
        "1679",
        "873",
        "",
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
        "https://www.filepicker.io/api/file/EyBJb7ZSvc1gpVWB0HxQ",
        "invite_admin_02.png",
        "1678",
        "870",
        "#8494b4",
        ""
      ]
    }
  ]
}
[/block]
Once you click "Confirm" button, an email with the following body is sent:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/IxtwKXw4RVKGbYPS7O65",
        "Invitation.png",
        "1292",
        "1460",
        "#1a70ce",
        ""
      ]
    }
  ]
}
[/block]
Until the newly invited administrator registers or accepts the invitation, it will be visible in the Pending Invitations section. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/7RpNvxkASqWIsHO8AEgV",
        "Pending_01.png",
        "1678",
        "872",
        "#264572",
        ""
      ]
    }
  ]
}
[/block]
If you changed your mind and you don’t feel like working with the person you just invited, click the Cancel invitation button. The invitation email will still be sent, but the url will expire making it impossible to register/accept.