---
title: "Ruby"
excerpt: ""
---
## Chapter Contents:
1. [Overview](#overview)
2. [Installing Syncano Ruby Library](#installing-syncano-ruby-library)
3. [Using Syncano Library](#using-syncano-library)
4. [Contributing](#contributing)
4. [Support](#section-support)
5. [License](#section-license)
[block:callout]
{
  "type": "info",
  "body": "We recently released a new version of our API that uses Scripts instead of CodeBoxes and Script Endpoints instead of Webhooks.\nFor us, it's not just about the name change - you can [read about it](https://www.syncano.io/blog/what-are-we-cooking-up-with-sockets/) on our blog.\nFor now, you can keep using old methods, and when we release new version of this library, we will update our docs here.",
  "title": "Sockets release"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This guide will walk you through the installation steps of Syncano Library for Ruby as well as give you a couple of usage examples.

If you don't have Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano).
[block:api-header]
{
  "type": "basic",
  "title": "Installing Syncano Ruby Library"
}
[/block]
# Install the library using gems:
[block:code]
{
  "codes": [
    {
      "code": "$ gem install syncano - -pre",
      "language": "ruby"
    }
  ]
}
[/block]
# API Root

After installation, you have to set a path for Syncano's API root.
[block:code]
{
  "codes": [
    {
      "code": "$ export API_ROOT=https://api.syncano.io",
      "language": "ruby",
      "name": "Linux/MacOS/Unix"
    },
    {
      "code": "$ SET API_ROOT=https://api.syncano.io",
      "language": "ruby",
      "name": "Windows"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using Syncano Library"
}
[/block]
Ok, now we can start coding!

1. [First steps](#section-first-steps)
2. [API Basics](#section-api-basics)
3. [Instances](#section-instances)
4. [Classes and Objects](#section-classes-and-objects)
5. [CodeBoxes](#section-codeboxes)
6. [Webhooks](#section-webhooks)

# First steps

To start using the Syncano library after installation is finished, you must first `require` it in your app. 
Next to connect to Syncano use your API Key - you can either pass your API Key to the connect method as shown below, or set `ENV['API_KEY']` and call just `Syncano.connect`.
Finally, to test if the connection is established, we will list our instances.
[block:code]
{
  "codes": [
    {
      "code": "require 'syncano'\nsyncano = Syncano.connect(api_key: 'API_KEY')\nsyncano.instances.all.each { |instance| puts instance }",
      "language": "ruby"
    }
  ]
}
[/block]
# API Basics

The Syncano API is a nested API - all the endpointes are scoped by an Instances, e.g. the CodeBoxes path is `/instance/YOUR_INSTANCE_NAME/codeboxes/`. 

# Instances

A Syncano Instance is more or less like a schema in a relational database. 
[block:callout]
{
  "type": "info",
  "body": "Your instance name must be unique across all existing Syncano Instances, not only limited to your account."
}
[/block]
In order to do anything with Syncano, you have to create an Instance. Choose a globally unique name, here is the call:
[block:code]
{
  "codes": [
    {
      "code": "instances = syncano.instances.create name: 'MY_INSTANCE_NAME'       \ninstance.first\n\n#=> #<Syncano::Resources::Instance created_at: Sun, 26 Apr 2015 18:09:46 +0000, description: \"\", metadata: {}, name: \"my_instance_name\", owner: nil, role: \"full\", updated_at: Sun, 26 Apr 2015 18:09:46 +0000>",
      "language": "ruby"
    }
  ]
}
[/block]

# Classes and Objects

In order to save Objects in Syncano, first you need to create a Class. A Class defines the Data Object's attributes in the Class schema. 

The attribute definition has two mandatory fields (`name`, `type`) and two optional fields (`filter_index` and `order_index`).

What these fields represent is quite self-descriptive: 
- `name` defines Data Objects' attribute name, 
- `type` defines type (you can read more about available types in the [Classes](doc:classes) section of this Developer Manual. 
- `order_index` allows you to order the returned collections, 
- `filter_index` allows filtering in a various ways. 
You can read more about filtering in our [docs on filtering](doc:data-objects-filtering).
[block:code]
{
  "codes": [
    {
      "code": "stock = instance.classes.create name: 'stock_items',\nschema: [\n  { name: 'name', type: 'string', filter_index: true },\n  { name: 'amount', type: 'integer', filter_index: true, order_index: true }]",
      "language": "ruby"
    }
  ]
}
[/block]
Once we have a Class, we can start creating Data Objects. 
[block:code]
{
  "codes": [
    {
      "code": "  chorizo = stock.objects.create name: 'Chorizo', amount: 100\n  black_pudding = stock.objects.create name: 'Black pudding', amount: 200\n  curry_wurts = stock.objects.create name: 'Curry wurst', amount: 150\n  kabanos = stock.objects.create name: 'Kabanos' \n  something = stock.objects.create amount: 3",
      "language": "ruby"
    }
  ]
}
[/block]
Now, since we have a few items in stock, let's try filtering:
[block:code]
{
  "codes": [
    {
      "code": "stock.objects.all(order_by: '-amount', query: { amount: { _lte: 150 }, name: { _exists: true } })\n\n#=> #<Syncano::Resources::Collection:0x007fc18b9c7698 @next=false, @prev=false, @collection=[#<Syncano::Resources::Object amount: 150, channel: nil, channel_room: nil, created_at: Mon, 27 Apr 2015 05:21:31 +0000, group: nil, group_permissions: \"none\", id: 12, name: \"Curry wurst\", other_permissions: \"none\", owner: nil, owner_permissions: \"none\", revision: 1, updated_at: Mon, 27 Apr 2015 05:21:31 +0000>, #<Syncano::Resources::Object amount: 100, channel: nil, channel_room: nil, created_at: Mon, 27 Apr 2015 05:21:30 +0000, group: nil, group_permissions: \"none\", id: 10, name: \"Chorizo\", other_permissions: \"none\", owner: nil, owner_permissions: \"none\", revision: 1, updated_at: Mon, 27 Apr 2015 05:21:30 +0000>]> ",
      "language": "ruby"
    }
  ]
}
[/block]
Let's give `something` a name and try again.
[block:code]
{
  "codes": [
    {
      "code": "something.name = 'Unidentified sausage' \nsomething.save\n  \nstock.objects.all(order_by: '-amount', query: { amount: { _lte: 150 }, name: { _exists: true } })\n\n#=> #<Syncano::Resources::Collection:0x007fc18d58a628 @next=false, @prev=false, @collection=[#<Syncano::Resources::Object amount: 150, channel: nil, channel_room: nil, created_at: Mon, 27 Apr 2015 05:21:31 +0000, group: nil, group_permissions: \\\"none\\\", id: 12, name: \\\"Curry wurst\\\", other_permissions: \\\"none\\\", owner: nil, owner_permissions: \\\"none\\\", revision: 1, updated_at: Mon, 27 Apr 2015 05:21:31 +0000>, #<Syncano::Resources::Object amount: 100, channel: nil, channel_room: nil, created_at: Mon, 27 Apr 2015 05:21:30 +0000, group: nil, group_permissions: \\\"none\\\", id: 10, name: \\\"Chorizo\\\", other_permissions: \\\"none\\\", owner: nil, owner_permissions: \\\"none\\\", revision: 1, updated_at: Mon, 27 Apr 2015 05:21:30 +0000>, #<Syncano::Resources::Object amount: 3, channel: nil, channel_room: nil, created_at: Mon, 27 Apr 2015 05:30:18 +0000, group: nil, group_permissions: \\\"none\\\", id: 15, name: \\\"Unidentified sausage\\\", other_permissions: \\\"none\\\", owner: nil, owner_permissions: \\\"none\\\", revision: 2, updated_at: Mon, 27 Apr 2015 05:30:48 +0000>]>",
      "language": "ruby"
    }
  ]
}
[/block]
Now it matches the query and appears in the result.

# CodeBoxes

CodeBoxes are small pieces of code that run on Syncano servers. 
You can run them: 
- Manually by using the API, 
- You can create a schedule to run them periodically, 
- You can create a Webhook (and optionally make it public) to run them from the web, 
- You can also create a trigger that will run the CodeBox after a Data Object from a connected Class is created, updated or deleted. 

Currently, there are four runtimes available: Ruby, Python, Node and Go. This gem is available in the Ruby runtime (just needs to be required). Let's create a simple CodeBox and run it:
[block:code]
{
  "codes": [
    {
      "code": "clock = instance.codeboxes.create(name: 'clock', source: 'puts Time.now', runtime_name: 'ruby')\n\n#=> #<Syncano::Resources::CodeBox config: {}, created_at: Thu, 30 Apr 2015 05:50:09 +0000, description: \"\", id: 1, name: \"clock\", runtime_name: \"ruby\", source: \"puts Time.now\", updated_at: Thu, 30 Apr 2015 05:50:09 +0000>\n\nclock.run \n\n#=> {\"status\"=>\"pending\", \"links\"=>{\"self\"=>\"gv1/instances/a523b7e842dea927d8c306ec0a9a7a4ac30191c2cd034b11d/codeboxes/1/traces/1/\"}, \"executed_at\"=>nil, \"result\"=>\"\", \"duration\"=>nil, \"id\"=>1}",
      "language": "ruby"
    }
  ]
}
[/block]
When you ask for a CodeBox to run, it returns the trace. Immediately after the call, it's status is pending, so you need to check the trace for the CodeBox execution result.
[block:code]
{
  "codes": [
    {
      "code": "clock.traces.first\n\n=> #<Syncano::Resources::CodeBoxTrace duration: 526, executed_at: Thu, 30 Apr 2015 05:25:14 +0000, id: 1, result: \"2015-04-30 05:25:14 +0000\", status: \"success\">",
      "language": "ruby"
    }
  ]
}
[/block]
The `run` method is asynchronous and returns the response immediately. You should use this to run CodeBoxes when you don't care about results at the very moment. If you want to run the CodeBox and get results in the same call, you should use Webhooks.

# Webhooks

You can use Webhooks to run CodeBoxes synchronously. Webhooks can be either public or private. You have to provide your API key when calling private ones, where as  the public ones don't require it - you can call them with curl, connect with third party services etc. To create and run one in Ruby:
[block:code]
{
  "codes": [
    {
      "code": "webhook = @instance.webhooks.create name: 'clock-webhook', codebox: clock.primary_key, public: true\n\n#=> #<Syncano::Resources::Webhook codebox: 1, public: true, public_link: \"a20b0ae122b53b2f2c445f6a7a202b274c3631ad\", name: \"clock-webhook\">\n\nwebhook.run['result']\n\n#=> \"2015-04-30 05:51:45 +0000\"",
      "language": "ruby"
    }
  ]
}
[/block]
and to run the same Webhook using cURL:
[block:code]
{
  "codes": [
    {
      "code": "$ curl \"https://api.syncano.rocks/v1/instances//af248d3e8b92e6e7aaa42dfc41de80c66c90d620cbe3fcd19/webhooks/p/a20b0ae122b53b2f2c445f6a7a202b274c3631ad/\"",
      "language": "curl"
    }
  ]
}
[/block]
Result of a cURL call:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\", \n  \"duration\": 270, \n  \"result\": \"2015-04-30 06:11:08 +0000\", \n  \"executed_at\": \"2015-04-30T06:11:08.607389Z\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Contributing"
}
[/block]
1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

Support
======

Now you’re ready to use Syncano in your Ruby projects. We really hope you will enjoy working with our platform. If you have any issues, just let us know at support@syncano.com.

License
======

Syncano’s Ruby Library is available under the MIT license. See the LICENSE file inside library sources for more info.