---
title: "Overview"
excerpt: ""
---
Chapter sections:
1. [What are permissions?]()
2. [Why do I need permissions?]()
3. [What are the different keys?]()

[block:api-header]
{
  "type": "basic",
  "title": "What are permissions?"
}
[/block]
The permissions system was designed so that you can have control over what your application users have access to on Syncano. If you create a file storage app, you'd probably want the users to see their own files and not files of other users. That's exactly what you can achieve by setting up proper permissions.

The permissions system on Syncano is inspired by [UNIX file system permissions](http://en.wikipedia.org/wiki/File_system_permissions).

When you configure permissions, you configure them for one of three categories: 
* `owner`
* `group` 
* `other`

The owner is a category that relates only to Data Objects. When a Data Object is created we can set a User as its `owner`. 
Groups are groups of Users (you can read more about creating groups in [this chapter](groups)). 
Others will be Users that are not an `owner` and are not assigned to a `group` that is connected with a resource (Data Objects, Classes and Channels). While `owner` only applies to Data Objects, `groups` and `other` categories can also be set for Classes and Channels.

The table below should clarify the concept a bit more:
[block:parameters]
{
  "data": {
    "0-0": "`owner`",
    "0-1": "`owner_permissions`",
    "1-0": "`group`",
    "1-1": "`group_permissions`",
    "2-0": "`other`",
    "2-1": "`other_permissions`",
    "0-2": "Data Objects",
    "1-2": "Data Objects, Classes, Channels",
    "2-2": "Data Objects, Classes, Channels",
    "h-0": "Category",
    "h-1": "Property",
    "h-2": "Applies to",
    "0-3": "The owner's permissions determine what actions the owner of the Data Object can perform on the resource.",
    "1-3": "The groups permissions determine what actions a user in the group can perform on a resource.",
    "2-3": "The permissions for others indicate what action all other users can perform on the resource.",
    "h-3": "Description"
  },
  "cols": 4,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Since Account Keys (administrators) and API Keys with ignore_acl set to true can ignore permissions, `other` relates only to users (who will authenticate with API Key and User Key combination).",
  "title": "other"
}
[/block]

[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Why do I need permissions?"
}
[/block]


[block:api-header]
{
  "type": "basic",
  "title": "What are the different keys?"
}
[/block]
## Account Key
By using an Account Key you act as a Syncano Administrator. You can access almost every endpoint in the Syncano API (with [user endpoint](user-management#section-user-endpoint) being the only exception). What this means is that it can be used not only to create Users, Classes, Data Objects etc. but also to change your billing or account information. 
[block:callout]
{
  "type": "danger",
  "body": "Because the Account Key has such strong permissions, we strongly advise you to use [API Keys](#connecting-with-an-api-key) in your application.",
  "title": "Important!"
}
[/block]
What is also worth noting is that all the tasks (creating Instances, adding API Keys or billing info) that an Account Key is designed for, can be done from the [Syncano Dashboard](https://dashboard.syncano.io).

## API Key
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

## User Key
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