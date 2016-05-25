---
title: "Watching a Channel"
excerpt: ""
---
Chapter sections:
1. [Polling for Notifications]()
2. [Handling Polling Requests]()

[block:api-header]
{
  "type": "basic",
  "title": "Polling for Notifications"
}
[/block]w
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

[block:api-header]
{
  "type": "basic",
  "title": "Handling Polling Requests"
}
[/block]
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