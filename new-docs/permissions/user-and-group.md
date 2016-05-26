---
title: "User and Group"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Example]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
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
## Using an API Key + User Key

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

[block:api-header]
{
  "type": "basic",
  "title": "Example"
}
[/block]

### Visual examples