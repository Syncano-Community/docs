---
title: "Channel Sockets (Real-time)"
excerpt: ""
---
Chapter sections:
1. [Channel Sockets Overview](#channel-sockets-overview)
2. [Channel Sockets Permissions](#channel-sockets-permissions)
3. [Notifications about Data Object changes](#notifications-about-data-object-changes)
4. [Publishing custom notification messages](#publishing-custom-notification-messages)
5. [Rooms](#rooms)
6. [Channel Sockets History](#channel-history)
7. [Summary](#summary)

[block:api-header]
{
  "type": "basic",
  "title": "Channel Sockets Overview"
}
[/block]

Channels are a way of providing realtime communication functionality in Syncano. Users can subscribe to Channels in order to get notifications about changes that happen to Data Objects connected to those Channels. 
[block:callout]
{
  "type": "info",
  "body": "Let's imagine a family album application, where users (family members) can upload photos. Every user would like to know if one of the family members uploaded a new photo to the album. To do that, the user subscribes to a channel called `new_photo`. Upon creation, Data Objects with a photograph file are connected to the `new_photo` channel. This way users connected to the `new_photo` channel instantly receive a notification about a new photo added."
}
[/block]
Apart from the possibility to subscribe to changes to Data Objects, Users can be granted the ability to send custom notification messages. The most common use case for this functionality would be a chat application (described more in depth in the [publishing custom notification messages](#publishing-custom-notification-messages) and the [rooms](#rooms) sections of this chapter). 

Ok, lets get to the point. This is how a Channel object looks like on Syncano:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"name\": \"new_channel\", \n    \"type\": \"default\",\n    \"description\": \"channel description\",\n    \"group\": null, \n    \"group_permissions\": \"none\", \n    \"other_permissions\": \"none\", \n    \"created_at\": \"2015-05-14T17:02:55.638356Z\", \n    \"updated_at\": \"2015-05-14T17:02:55.638393Z\", \n    \"custom_publish\": false, \n    \"links\": {\n      // links to connected resources\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:parameters]
{
  "data": {
    "h-1": "Description",
    "h-0": "Property",
    "0-0": "`name`",
    "0-1": "Channel name.",
    "1-0": "`description`",
    "2-0": "`type`",
    "1-1": "Channel description (optional).",
    "2-1": "Possible values are:\n- `default`\n- `separate_rooms`\n\nWhen `separate_rooms` is enabled, users can create `rooms`. The `rooms` functionality is described in depth in the [rooms](#rooms) section of this chapter.",
    "3-0": "`custom_publish`",
    "3-1": "Possible values are:\n- `true`\n- `false`\n\nWith `custom_publish` (and proper channel permissions set) users can publish custom notification messages to a channel. Described in depth in the [publishing custom notification messages](#publishing-custom-notification-messages) section of this chapter."
  },
  "cols": 2,
  "rows": 4
}
[/block]
You can find all your channels in Syncano's [Dashboard](https://dashboard.syncano.io) under `Channels` tab.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/q0ZejpnlRLqvicTYYeAL",
        "Channels_list_01.png",
        "1404",
        "593",
        "#24426e",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Channel Sockets Permissions"
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
Enough with the theory! Lets move to more practical examples.
[block:api-header]
{
  "type": "basic",
  "title": "Notifications about Data Object changes"
}
[/block]
Channels can be connected to Data Objects so that they will send messages whenever a Data Object is created, changed or deleted. This section will walk you through:
1. [Creating a Channel Socket](#section-creating-a-channel-socket)
2. [Polling for notification messages](#section-polling-for-notification-messages)
3. [Creating a Data Object](#section-creating-a-data-object)
4. [Handling the polling requests](#section-handling-the-polling-requests)


### Creating a Channel Socket
To be able to receive notifications about changes in Data Objects we'll have to create an appropriate channel first. We'll create a `default` channel with `other_permissions` set to `subscribe`. This way the channel will be available to read to all the users. They will be able to subscribe to this channel to receive notifications about changes that happen to Data Objects which are connected to this channel. This is how the channel creation call would look:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\":\"realtime\",\n     \"type\":\"default\",\n     \"other_permissions\": \"subscribe\",\n     \"custom_publish\": \"false\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nChannel.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"channel\",\n  description=\"channel description\",\n  type=\"default\",\n  other_permissions=\"publish\",\n  custom_publish=True\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar channel = {\n  name:\"realtime\",\n  type:\"default\",\n  other_permissions: \"subscribe\",\n  custom_publish: \"false\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nChannel.please().create(channel).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Channel newChannel = new Channel(\"channel_name\");\nnewChannel.setType(ChannelType.DEFAULT);\nnewChannel.setOtherPermissions(ChannelPermissions.SUBSCRIBE);\nnewChannel.setCustomPublish(false);\n\nResponse<Channel> response = syncano.createChannel(newChannel).send();",
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
With a channel created, we can start polling for changes in it...

### Polling for notification messages
To be able to receive a notification message you have to poll for it. This call shows how a user would poll for changes that happen in a channel named `realtime`:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/channels/realtime/poll/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\nsyncano.connect(api_key=\"API_KEY\")\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"realtime\"\n)\n\nchannel.poll(callback=callback)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {\n  name:\"realtime\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nvar poll = Channel.please().poll(query);\n\npoll.on('start', function() {\n  console.log('poll::start');\n});\n\npoll.on('stop', function() {\n  console.log('poll::stop');\n});\n\npoll.on('message', function(message) {\n  console.log('poll::message', message);\n});\n\npoll.on('custom', function(message) {\n  console.log('poll::custom', message);\n});\n\npoll.on('create', function(data) {\n  console.log('poll::create', data);\n});\n\npoll.on('delete', function(data) {\n  console.log('poll::delete', data);\n});\n\npoll.on('update', function(data) {\n  console.log('poll::update', data);\n});\n\npoll.on('error', function(error) {\n  console.log('poll::error', error);\n});\n\npoll.start();",
      "language": "javascript"
    },
    {
      "code": "ChannelConnection channelConnection = new ChannelConnection(syncano, new ChannelConnectionListener() {\n   @Override\n   public void onNotification(Notification notification) {\n   }\n   @Override\n   public void onError(Response<Notification> response) {\n   }\n});\n\nchannelConnection.start(\"channel_name\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// You need to implement SCChannelDelegate protocol\n@interface ViewController ()<SCChannelDelegate>\n@property (strong) SCChannel *channel;\n//...\n@end\n  \n@implementation ViewController\n\n//implement protocol method\n- (void)chanellDidReceivedNotificationMessage:(SCChannelNotificationMessage *)notificationMessage {\n    //handle notification\n}\n\n- (void)viewDidLoad {\n  [Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"instance_name\"];\n  //create channel object and start listening to notifications\n  self.channel = [[SCChannel alloc] initWithName:@\"realtime\" andDelegate:self];\n  [self.channel subscribeToChannel];\n}\n\n@end",
      "language": "objectivec"
    },
    {
      "code": "// You need to implement SCChannelDelegate protocol\nclass SwiftViewController: SCChannelDelegate {\n  let syncano = Syncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"instance_name\")\n  var channel: SCChannel? = nil\n  //...\n  \n  //implement protocol method\n  func chanellDidReceivedNotificationMessage(notificationMessage: SCChannelNotificationMessage) {\n  //handle notification\n  }\n  \n  func viewDidLoad() {\n    //create channel object and start listening to notifications\n    self.channel = SCChannel(name: \"realtime\", andDelegate: self)\n    self.channel?.subscribeToChannel()\n  }\n}",
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
### Creating a Data Object
Now, when creating a Data Object, we have to assign it to the `realtime` channel:
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
### Handling the polling requests
An important part of the notification message is its `id`. If you want to constantly receive new notification messages about changes in the Data Objects, you should pass the last notification message `id` in the next calls that you make. Since the notification message `id` that we received in our last call was equal to `1` we should pass it as a `last_id` in our next call:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET -G \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n-d \"last_id=1\"\n\"https://api.syncano.io/v1.1/instances/instance/channels/realtime/poll/\"",
      "language": "curl"
    },
    {
      "code": "# Coming soon!",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {\n  name: \"can_publish\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nvar poll = Channel.please().poll(query);\n\npoll.on('start', function() {\n  console.log('poll::start');\n});\n\npoll.on('stop', function() {\n  console.log('poll::stop');\n});\n\npoll.on('message', function(message) {\n  console.log('poll::message', message);\n});\n\npoll.on('custom', function(message) {\n  console.log('poll::custom', message);\n});\n\npoll.on('create', function(data) {\n  console.log('poll::create', data);\n});\n\npoll.on('delete', function(data) {\n  console.log('poll::delete', data);\n});\n\npoll.on('update', function(data) {\n  console.log('poll::update', data);\n});\n\npoll.on('error', function(error) {\n  console.log('poll::error', error);\n});\n\npoll.start();",
      "language": "javascript"
    },
    {
      "code": "channelConnection.start(\"channel_name\", null, lastId);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// See example above. Everything you need to do is start listening to notifications, we will handle last_id parameter for you\n\n//will get new last_id and use it in subsequent calls\nself.channel = [[SCChannel alloc] initWithName:@\"realtime\" andDelegate:self];\n[self.channel subscribeToChannel];",
      "language": "objectivec"
    },
    {
      "code": "// See example above. Everything you need to do is start listening to notifications, we will handle last_id parameter for you\n\n//will get new last_id and use it in subsequent calls\nself.channel = SCChannel(name: \"realtime\", andDelegate: self)\nself.channel?.subscribeToChannel()",
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
Now, when a Data Object is changed, we'll receive a next notification message that will hold updated values of the Data Object and a new notification message `id` that we should pass in our next call if we want to keep listening to changes on this channel.
[block:callout]
{
  "type": "warning",
  "title": "Request Timeout",
  "body": "Timeout for a request to a channel is 5 minutes. When the request times out it returns a `204 NO CONTENT` empty request. To handle this simply repeat the previous request with an unchanged `last_id`."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# Not supported",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\nsyncano.connect(api_key=\"API_KEY\")\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"realtime\"\n)\n\nchannel.poll(callback=callback)  # the last_id is handled here by default;\n# if you wanna start listening from some point:\nchannel.poll(callback=callback, last_id=\"LAST_ID\")",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar instance = new Syncano({apiKey: 'API_KEY', userKey: 'USER_KEY'});\n\nvar lastId = 1;\n\n(function watch(lastId) {\n  instance.channel('realtime').poll({lastId: lastId})\n  .then(function(res) {\n    if (res === \"NO CONTENT\") { // <-- this line checks for empty response\n        watch();\n    } else {\n      console.log(res);\n      watch();\n    }\n  })\n  .catch(function(err) {\n    console.log(err);\n  });\n})(lastId);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "// Handled by library automatically",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// See example above. Everything you need to do is start listening to notifications, we will handle listening properly for changed in the background for you\n\n//will create new polling requests when needed\nself.channel = [[SCChannel alloc] initWithName:@\"realtime\" andDelegate:self];\n[self.channel subscribeToChannel];",
      "language": "objectivec"
    },
    {
      "code": "// See example above. Everything you need to do is start listening to notifications, we will handle listening properly for changed in the background for you\n\n//will create new polling requests when needed\nself.channel = SCChannel(name: \"realtime\", andDelegate: self)\nself.channel?.subscribeToChannel()",
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
  "title": "Publishing custom notification messages"
}
[/block]
You might want to allow your Users to be able to send custom messages on a Channel. This functionality might come in handy when you'd like to create e.g. a chat application. Here's what we'll want to do to:
1. [Create a Channel Socket with the "custom_publish" flag](#section-creating-a-channel-socket-with-the-custom_publish-flag)
2. [Poll for changes](#section-polling-for-changes)
3. [Send custom messages](#section-sending-custom-messages)

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

### Polling for changes 
We will poll for changes in the "can_publish" channel that we just created:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"X-USER-KEY: USER_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/can_publish/poll/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\n\ndef callback(message=None):\n    print message.payload\n    return True\n\nsyncano.connect(api_key=\"API_KEY\")\n\nchannel = Channel.please.get(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"can_publish\"\n)\n\nchannel.poll(callback=callback)\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar query = {\n  name: \"can_publish\",\n  instanceName: \"INSTANCE_NAME\"\n};\n\nvar poll = Channel.please().poll(query);\n\npoll.on('start', function() {\n  console.log('poll::start');\n});\n\npoll.on('stop', function() {\n  console.log('poll::stop');\n});\n\npoll.on('message', function(message) {\n  console.log('poll::message', message);\n});\n\npoll.on('custom', function(message) {\n  console.log('poll::custom', message);\n});\n\npoll.on('create', function(data) {\n  console.log('poll::create', data);\n});\n\npoll.on('delete', function(data) {\n  console.log('poll::delete', data);\n});\n\npoll.on('update', function(data) {\n  console.log('poll::update', data);\n});\n\npoll.on('error', function(error) {\n  console.log('poll::error', error);\n});\n\npoll.start();",
      "language": "javascript"
    },
    {
      "code": "ChannelConnection channelConnection = new ChannelConnection(syncano, new ChannelConnectionListener() {\n    @Override\n    public void onNotification(Notification notification) {\n    }\n\n    @Override\n    public void onError(Response<Notification> response) {\n    }\n});\n\nchannelConnection.start(\"channel_name\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCChannel *channel = [[SCChannel alloc] initWithName:@\"can_publish\" andDelegate:self];\n[channel subscribeToChannel];",
      "language": "objectivec"
    },
    {
      "code": "let channel = SCChannel(name: \"can_publish\", andDelegate: self)\nchannel.subscribeToChannel()",
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
### Sending custom messages
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

[block:api-header]
{
  "type": "basic",
  "title": "Rooms"
}
[/block]
The purpose of rooms is to provide a place for Users to talk inside a channel. Lets use a chat application as an example. Each of the application Users can have their own channel rooms. If User A wants to write a private message to User B, User A will publish a notification message to User B's channel room. User B will get the message instantly because that user is polling their own channel room. General rooms can also exist. In this scenario User A, User B and User C are all polling the notification messages from this "general" room and can publish messages to it.

So a room doesn't need to be created nor needs permissions. A User must only have permissions to a specific channel. You also cannot list current rooms as they are volatile and aren't actually created (although you could store room names in a Data Object, if that was something you would want in your application). 

## Using rooms

1. [Creating "separate_rooms" Channel Socket](#section-creating-separate_rooms-channel-socket)
2. [Polling for changes in a room](#section-polling-for-changes-in-a-room)
3. [Creating Data Objects connected to the "separate_rooms" Channel Socket](#section-creating-data-objects-connected-to-the-separate_rooms-channel-socket-)
4. [Publishing custom messages to the "separate_rooms" Channel Socket](#section-publishing-custom-messages-to-the-separate_rooms-channel-socket-)


### Creating `separate_rooms` Channel Socket
To be able to use the Rooms functionality, a Channel has to be created with the `type` equal to `separate_rooms`:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\":\"channel_with_rooms\",\n     \"type\":\"separate_rooms\",\n     \"other_permissions\": \"publish\",\n     \"custom_publish\": \"true\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance/channels/\"",
      "language": "curl"
    },
    {
      "code": "from syncano.models import Channel\nimport syncano\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nChannel.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  name=\"channel_with_rooms\",\n  description=\"channel description\",\n  type=\"separate_rooms\",\n  other_permissions=\"publish\",\n  custom_publish=True\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Channel = connection.Channel;\n\nvar channel = {\n  name: \"channel_with_rooms\",\n  type: \"separate_rooms\",\n  other_permissions: \"publish\",\n  custom_publish: \"true\",\n \tinstanceName: \"INSTANCE_NAME\"\n};\n\nChannel.please().create(channel).then(callback);",
      "language": "javascript"
    },
    {
      "code": "Channel newChannel = new Channel(\"channel_with_rooms\");\nnewChannel.setType(ChannelType.SEPARATE_ROOMS);\nnewChannel.setOtherPermissions(ChannelPermissions.PUBLISH);\nnewChannel.setCustomPublish(true);\n\nsyncano.createChannel(newChannel).send();",
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
### Polling for changes in a room
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
### Creating Data Objects connected to the `separate_rooms` Channel Socket:
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
### Publishing custom messages to the `separate_rooms` Channel Socket:
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

### User viewing history of a Channel Socket with "separate_rooms`
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
### Admin viewing history of a Channel Socket with "separate_rooms`
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

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
Congratulations! If you've reached this far, then you know quite about coding your app on Syncano. You can either check out the Advanced concepts below or just start hacking!

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/4wOxjYWSIm07DR59ml1I",
        "Animals-Chicken-icon.png",
        "128",
        "128",
        "#040404",
        ""
      ]
    }
  ]
}
[/block]