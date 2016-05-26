---
title: "Data Object"
excerpt: ""
---
Chapter sections:
1. [Overview]()
2. [Examples]()

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
[block:parameters]
{
  "data": {
    "h-0": "Permission Type",
    "h-1": "Description",
    "0-0": "`none`",
    "1-0": "`subscribe`",
    "2-0": "`publish`",
    "0-1": "Default mode where users don't have access to channel notifications.",
    "1-1": "User can receive notifications from a channel.",
    "2-1": "User can publish data to a channel."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Channels and their permissions are described in depth in the [Realtime Communication](realtime-communication) chapter of this manual."
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important!",
  "body": "- Only Syncano Platform Administrators can create Channel Sockets.\n- Users can subscribe to a channel or publish data to a channel (provided that channel has appropriate permissions set)."
}
[/block]
An important part of operating on Channels are their permission types. You can set the permission types during channel creation and/or update them afterwards. These are the available permission types:
[block:parameters]
{
  "data": {
    "0-0": "`none`",
    "1-0": "`subscribe`",
    "2-0": "`publish`",
    "h-0": "Permission Type",
    "h-1": "Description",
    "0-1": "The default mode where users don't have access to channel notifications.",
    "1-1": "A User can receive notifications from a channel and view channel history.",
    "2-1": "A User can publish data to a channel."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/RwvJYW2NSnOMhzMNGtdS",
        "Add_channel_01.png",
        "1679",
        "709",
        "#24426e",
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
        "https://www.filepicker.io/api/file/aOzaiRHCQwWWSGq0aGKp",
        "Add_channel_02.png",
        "1677",
        "750",
        "#6c7ca4",
        ""
      ]
    }
  ]
}
[/block]

### Creating a Channel Socket with the `custom_publish` flag
To allow your Users to send custom messages, you'll have to create a channel that:
- Has appropriate permissions - `publish` permission type for either `group_permissions` or `other_permissions` (depending on which Users should be granted the ability to send the messages)
- Has the `custom_publish` flag set to `true`

This how you could create such a Channel Socket:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\":\"can_publish\",\n     \"type\":\"default\",\n     \"other_permissions\": \"publish\",\n     \"custom_publish\": \"true\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nChannel.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"can_publish\",\n  description=\"channel description\",\n  type=\"default\",\n  other_permissions=\"publish\",\n  custom_publish=True\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar channel = {\n  name: \"can_publish\",\n  type: \"default\",\n  other_permissions: \"publish\",\n  custom_publish: \"true\",\n \tinstanceName: \"INSTANCE_NAME\"\n};\n\nChannel.please().create(channel).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Channel newChannel = new Channel(\"channel_name\");\nnewChannel.setType(ChannelType.DEFAULT);\nnewChannel.setOtherPermissions(ChannelPermissions.PUBLISH);\nnewChannel.setCustomPublish(true);\n\nsyncano.createChannel(newChannel).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
      "language": "objectivec"
    },
    {
      "code": "// Not supported in iOS Library - please use Dashboard instead",
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
Now Users will be able to post custom messages to this Channel Socket, but lets set up polling first.

[block:api-header]
{
  "type": "basic",
  "title": "Examples"
}
[/block]