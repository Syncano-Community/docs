---
title: "Channel Sockets (Real-time)"
excerpt: ""
---
Chapter sections:
1. [The custom_publish flag]()
2. [Sending Custom Messages]()

You might want to allow your Users to be able to send custom messages on a Channel. This functionality might come in handy when you'd like to create e.g. a chat application. Here's what we'll want to do to:

[block:api-header]
{
  "type": "basic",
  "title": "The custom_publish Flag"
}
[/block]

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
  "title": "Sending Custom Messages"
}
[/block]

This is how a User can send a custom message to the `can_publish` Channel. Message data can be specified in the `payload` parameter:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"payload\":{\"content\":\"hello!\"}}'\n\"https://api.syncano.io/v1.1/instances/instance/channels/can_publish/publish/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\n\nsyncano.connect(\n  api_key=\"API_KEY\",\n  user_key=\"USER_KEY\"\n)\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"can_publish\"\n)\n\nchannel.publish(payload={\"message\":\"hello there\"})",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {name: \"can_publish\", instanceName: \"INSTANCE_NAME\"};\nvar message = {\"content\":\"hello!\"};\n\nChannel.please().publish(query, message).then(callback);",
      "language": "javascript"
    },
    {
      "code": "JsonObject payload = new JsonObject();\npayload.addProperty(\"content\", \"hello!\");\n\nNotification newNotification = new Notification(payload);\nResponse<Notification> response = syncano.publishOnChannel(\"channel_name\", newNotification).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCChannel *channel = [[SCChannel alloc] initWithName:@\"can_publish\" andDelegate:self];\n[channel publish:@{@\"content\":@\"hello\"} withCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "let channel = SCChannel(name: \"can_publish\", andDelegate: self)\nchannel.publishWithCompletionBlock([\"content\":\"hello\"]) { error in\n  //handle error\n}",
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
A User that polled for the changes in the `can_publish` channel will receive a notification message. It's a bit different from messages sent when Data Objects are created and it looks like this:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"author\": {\n        \"api_key\": 2805,\n        \"user\": 1\n    },\n    \"created_at\": \"2015-05-19T15:53:19.281155Z\",\n    \"id\": 8,\n    \"action\": \"custom\",\n    \"payload\": {\"content\":\"hello!\"},\n    \"metadata\": {\n        \"type\": \"message\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`author`",
    "0-1": "A dictionary describing who was responsible for a creation/change of a Data Object. There are three possible key:value pairs:\n- `\"admin\"`: `admin_id`\n- `\"api_key\"`: `api_key_id`\n- `\"user\"`: `user_id`",
    "1-1": "Message `id`. Should be added to subsequent polling requests.",
    "1-0": "`id`",
    "2-0": "`action`",
    "2-1": "The type of event that induced the message. In the case of custom messages it will have `custom` as a value.",
    "3-0": "`payload`",
    "3-1": "Will hold whatever we decide to include when sending a custom message.",
    "4-0": "`metadata`",
    "4-1": "Is of `{\"type\":\"message\"}` in case of custom messages"
  },
  "cols": 2,
  "rows": 5
}
[/block]