---
title: "Channel Sockets (Real-time)"
excerpt: ""
---
Chapter sections:
1. [What is a Room?]()
2. [What are `separate_rooms`?]()
3. [Polling for Room Changes]()
4. [Custom Messages and Rooms]()

[block:api-header]
{
  "type": "basic",
  "title": "What is a Room?"
}
[/block]

The purpose of rooms is to provide a place for Users to talk inside a channel. Lets use a chat application as an example. Each of the application Users can have their own channel rooms. If User A wants to write a private message to User B, User A will publish a notification message to User B's channel room. User B will get the message instantly because that user is polling their own channel room. General rooms can also exist. In this scenario User A, User B and User C are all polling the notification messages from this "general" room and can publish messages to it.

So a room doesn't need to be created nor needs permissions. A User must only have permissions to a specific channel. You also cannot list current rooms as they are volatile and aren't actually created (although you could store room names in a Data Object, if that was something you would want in your application).

[block:api-header]
{
  "type": "basic",
  "title": "What are `separate_rooms`?"
}
[/block]

To be able to use the Rooms functionality, a Channel has to be created with the `type` equal to `separate_rooms`:

## Screenshot using the separate rooms flag

Data Objects need to be different too.

Data Objects connected to a `separate_rooms` channel should have `channel_room` specified during the Data Object creation.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"channel\":\"channel_with_rooms\", \"channel_room\":\"room_name\"}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/class/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.create(\n  channel=\"channel_with_rooms\",\n  channel_room=\"room_name\",\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"CLASS_NAME\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar obj = {\n  channel:\"channel_with_rooms\",\n  channel_room: \"room_name\",\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_NAME\"\n};\n\nDataObject.please().create(obj).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\nbook.setChannel(\"separate_rooms\");\nbook.setChannelRoom(\"room_name\");\nbook.save();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "Book *book = [Book new];\nbook.title = @\"How to be a Pirate\";\nbook.channel = @\"channel_name\";\nbook.channelRoom = @\"room_name\";\n[book saveWithCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "let book = Book()\nbook.title = \"How to be a Pirate\"\nbook.channel = \"channel_name\"\nbook.channelRoom = \"room_name\"\nbook.saveWithCompletionBlock { error in\n  //handle error\n}",
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

[block:api-header]
{
  "type": "basic",
  "title": "Polling for Room Changes"
}
[/block]

Polling for changes in `separate_room` channels differs from a regular polling request. What needs to be added is a `room` parameter:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET -G \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-d \"room=room_name\" \\\n-d \"last_id=last_notification_id\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/channels/channel_with_rooms/poll/",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\nsyncano.connect(api_key=\"API_KEY\")\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"channel_with_rooms\"\n)\n\nchannel.poll(callback=callback, room=\"room_name\")",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {\n  name: \"can_publish\",\n  room: \"room_name\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nvar poll = Channel.please().poll(query);\n\npoll.on('start', function() {\n  console.log('poll::start');\n});\n\npoll.on('stop', function() {\n  console.log('poll::stop');\n});\n\npoll.on('message', function(message) {\n  console.log('poll::message', message);\n});\n\npoll.on('custom', function(message) {\n  console.log('poll::custom', message);\n});\n\npoll.on('create', function(data) {\n  console.log('poll::create', data);\n});\n\npoll.on('delete', function(data) {\n  console.log('poll::delete', data);\n});\n\npoll.on('update', function(data) {\n  console.log('poll::update', data);\n});\n\npoll.on('error', function(error) {\n  console.log('poll::error', error);\n});\n\npoll.start();",
      "language": "javascript"
    },
    {
      "code": "ChannelConnection channelConnection = new ChannelConnection(syncano, new ChannelConnectionListener() {\n    @Override\n    public void onNotification(Notification notification) {\n    }\n\n    @Override\n    public void onError(Response<Notification> response) {\n    }\n});\n\nchannelConnection.start(\"channel_with_rooms\", \"room_name\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCChannel *channel = [[SCChannel alloc] initWithName:@\"can_publish\" andDelegate:self];\nchannel.room = @\"room_name\";\n[channel subscribeToChannel];",
      "language": "objectivec"
    },
    {
      "code": "let channel = SCChannel(name: \"can_publish\", andDelegate: self)\nchannel.room = \"room_name\"\nchannel.subscribeToChannel()",
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

[block:api-header]
{
  "type": "basic",
  "title": "Custom Messages and Rooms"
}
[/block]

Custom messages published by users on the `separate_rooms` channel should have both `payload` and `room` parameters specified.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"payload\":{\"content\":\"hello!\"}, \"room\":\"room_name\"}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/channels/can_publish/publish/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\n\nsyncano.connect(\n  api_key=\"API_KEY\",\n  user_key=\"USER_KEY\"\n)\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"can_publish\"\n)\n\nchannel.publish(payload={\"message\":\"hello there\"}, room=\"room_name\")",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {name: \"can_publish\", instanceName: \"INSTANCE_NAME\"};\nvar message = {\"content\":\"hello!\"};\nvar room = \"room_name\";\n\nChannel.please().publish(query, message, room).then(callback);",
      "language": "javascript"
    },
    {
      "code": "JsonObject payload = new JsonObject();\npayload.addProperty(\"content\", \"hello!\");\n\nNotification newNotification = new Notification(\"room_name\", payload);\nsyncano.publishOnChannel(\"separate_rooms\", newNotification).send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCChannel *channel = [[SCChannel alloc] initWithName:@\"can_publish\" andDelegate:self];\nchannel.room = @\"room_name\";\n[channel publish:@{@\"content\":@\"hello\"} withCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "let channel = SCChannel(name: \"can_publish\", andDelegate: self)\nchannel.room = \"room_name\"\nchannel.publishWithCompletionBlock([\"content\":\"hello\"]) { error in\n  //handle error\n}",
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

Add comments on viewing history for separate rooms and link to history section.