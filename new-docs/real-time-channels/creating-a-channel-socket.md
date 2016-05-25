---
title: "Creating a Channel Socket"
excerpt: ""
---
Chapter sections:
1. [In the Dashboard]()
2. [Creating Data with Channels]()
3. [Channel History]()

[block:api-header]
{
  "type": "basic",
  "title": "In the Dashboard"
}
[/block]

To be able to receive notifications about changes in Data Objects we'll have to create an appropriate channel first. We'll create a `default` channel with `other_permissions` set to `subscribe`. This way the channel will be available to read to all the users. They will be able to subscribe to this channel to receive notifications about changes that happen to Data Objects which are connected to this channel. This is how the channel creation call would look:

## Screenshot

[block:api-header]
{
  "type": "basic",
  "title": "Creating Data with Channels"
}
[/block]

Now, when creating a Data Object, we have to assign it to the `real-time` channel:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"channel\":\"realtime\"}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/CLASS/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.create(\n  channel=\"realtime\",\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"CLASS_NAME\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar obj = {\n  channel:\"realtime\",\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_NAME\"\n};\n\nDataObject.please().create(obj).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\nbook.setChannel(\"channel_name\");\nbook.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "Book *book = [Book new];\nbook.title = @\"How to be a Pirate\";\nbook.channel = @\"realtime\";\n[book saveWithCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "let book = Book()\nbook.title = \"How to be a Pirate\"\nbook.channel = \"realtime\"\nbook.saveWithCompletionBlock { error in\n  //handle error\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "",
      "language": "ruby"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "A Data Object can be connected to a channel ONLY during creation. You won't be able to update a Data Object later so that it belongs to a certain channel.",
  "title": "Important!"
}
[/block]
The polling request that we made in the first step will return a notification message:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"author\": {\n        \"admin\": 1379\n    },\n    \"created_at\": \"2015-05-18T16:51:11.171275Z\",\n    \"id\": 1,\n    \"action\": \"create\",\n    \"payload\": {\n        \"group\": null,\n        \"string\": \"name\",\n        \"group_permissions\": \"none\",\n        \"created_at\": \"2015-05-18T16:51:11.162254Z\",\n        \"updated_at\": \"2015-05-18T16:51:11.162292Z\",\n        \"owner_permissions\": \"none\",\n        \"id\": 6,\n        \"owner\": null,\n        \"other_permissions\": \"none\",\n        \"revision\": 1\n    },\n    \"metadata\": {\n        \"type\": \"object\",\n        \"class\": \"class_name\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Here are the important properties of a notification message:
[block:parameters]
{
  "data": {
    "h-0": "Properties",
    "h-1": "Description",
    "0-0": "`author`",
    "0-1": "A dictionary describing who was responsible for a creation/change of a Data Object. There are three possible key:value pairs:\n- `\"admin\"`: `admin_id`\n- `\"api_key\"`: `api_key_id`\n- `\"user\"`: `user_id`",
    "1-0": "`id`",
    "1-1": "Message id. Should be added to subsequent polling requests.",
    "2-0": "`action`",
    "3-0": "`payload`",
    "3-1": "The payload holds a serialized Data Object. It will differ depending on an `action` value:\n- `create`: full Data Object\n- `update`: updated Data Object values + Data Object id\n- `delete`: only Data Object `id`",
    "4-0": "`metadata`",
    "4-1": "Info about the Data Object the notification message relates to. It has two keys:\n- `\"type\"`: `\"object\"`\n- `\"class\"`: `\"class_name\"`",
    "2-1": "The type of event that induced the message. In the case of Data Objects it can have three values:\n- `create`\n- `update`\n- `delete`"
  },
  "cols": 2,
  "rows": 5
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Channel History"
}
[/block]

Channel history is where the past notification messages are stored. What is important to know about it is that the notifications are not persistent and are removed from Syncano after 24 hours. 

### Viewing Channel history
Viewing channel history requires a User to have `subscribe` or higher permission type to a channel that the user wants to view the history of. Administrators authenticating with an Account Key and API Keys with `ignore_acl` can view the channel history without restrictions.

### User viewing a Channel history:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/channels/channel/history/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Message\n\nchannel_histories = Message.please.all(channel_name='CHANNEL_NAME')\n\nfor channel_history in channel_histories:\n    payload = channel_history.payload\n    metadata = channel_history.metadata\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nChannel.please().history({instanceName: \"INSTANCE_NAME\", name: \"NAME\"}).then(callback);",
      "language": "javascript"
    },
    {
      "code": "ResponseGetList<Notification> response = syncano.getChannelsHistory(\"channel_name\").send();\nList<Notification> list = response.getData();",
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
Sample response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null, \n    \"prev\": null, \n    \"objects\": [\n        {\n            \"id\": 17, \n            \"room\": null, \n            \"created_at\": \"2015-05-20T14:24:08.490430Z\", \n            \"action\": \"create\", \n            \"author\": {\n                \"admin\": 1379\n            }, \n            \"metadata\": {\n                \"type\": \"object\", \n                \"class\": \"class\"\n            }, \n            \"payload\": {\n                // Data Object \n            }, \n            \"links\": {\n                \"self\": \"/v1.1/instances/<instance>/channels/<channel>/history/17/\"\n            }\n        }, \n        {\n            \"id\": 14, \n            \"room\": null, \n            \"created_at\": \"2015-05-20T09:42:28.515096Z\", \n            \"action\": \"custom\", \n            \"author\": {\n                \"api_key\": 2805, \n                \"user\": 1\n            }, \n            \"metadata\": {\n                \"type\": \"message\"\n            }, \n            \"payload\": {\n                \"content\": \"Hello!\"\n            }, \n            \"links\": {\n                \"self\": \"/v1.1/instances/<instance>/channels/<channel>/history/14/\"\n            }\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
### Viewing history of a Channel Socket with "separate_rooms`

Again, viewing a Channel with the `separate_rooms` functionality is a bit different from viewing a regular channel history. Users who want to access the history need to pass `room` with the room name parameter. Also permissions work differently because Users can only see the requested room history. Administrators with Account Keys and API Keys with the `ignore_acl` flag set to true can view whole channel history regardless of the room separations (they don't have to pass the room parameter).

### User viewing history of a Channel Socket with "separate_rooms"
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET -G \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-d \"room=room_name\" \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/channel_with_rooms/history/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Message\n\nchannel_histories = Message.please.all(\n  channel_name='CHANNEL_NAME', \n  room='ROOM_NAME'\n\t)\n\nfor channel_history in channel_histories:\n    payload = channel_history.payload\n    metadata = channel_history.metadata",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar instance = new Syncano({apiKey: 'API_KEY', instanceName: \"INSTANCE_NAME\", userKey: 'USER_KEY'});\n\nvar msg = {\"payload\":{\"content\":\"hello!\"}, \"room\":\"room_name\"};\n\ninstance.channel(CHANNEL).history({room: 'room_name'}, callback());",
      "language": "javascript"
    },
    {
      "code": "ResponseGetList<Notification> roomResponse = syncano.getChannelsHistory(\"channel_name\", \"room_name\").send();\nList<Notification> roomList = roomResponse.getData();",
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
### Admin viewing history of a Channel Socket with "separate_rooms"
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/channel_with_rooms/history/\"",
      "language": "curl"
    },
    {
      "code": "# Coming soon!",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar instance = new Syncano({accountKey: 'ACCOUNT_KEY'});\n\nvar msg = {\"payload\":{\"content\":\"hello!\"}, \"room\":\"room_name\"};\n\ninstance.channel(CHANNEL).history({room: 'room_name'}, callback());",
      "language": "javascript"
    },
    {
      "code": "ResponseGetList<Notification> roomResponse = syncano.getChannelsHistory(\"channel_name\", \"room_name\").send();\nList<Notification> roomList = roomResponse.getData();",
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