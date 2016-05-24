---
title: "Getting Started With Syncano"
excerpt: "In this chapter you will learn the basic steps of how to get up and running on Syncano"
---
Sections:
1. [Sign up](#sign-up)
2. [Adding an Instance](#adding-an-instance)
3. [Getting an API Key](#getting-an-api-key)
4. [Connecting from a Library](#connecting-from-a-library)
5. [Where to go next](#where-to-go-next)
[block:api-header]
{
  "type": "basic",
  "title": "Sign up"
}
[/block]
To create an account in Syncano follow these steps:

1. Go to this website: [https://dashboard.syncano.io/#signup](https://dashboard.syncano.io/#signup)
2. Type in your email address
3. Type in the password you would like to use to access Syncano
4. Click on 'Create My Account' button
5. Confirm your account creation by clicking the URL in an e-mail you receive from us
 
You can also use an external service to sign up/log in. Currently we support:

+ Google
+ Facebook
+ Github 

To create an account using one of these services, just click on the specific social sign up button of your choice. 
​
Did we miss your favorite service? [send us an email](mailto:hello@syncano.com) and let us know!
[block:api-header]
{
  "type": "basic",
  "title": "Adding an Instance"
}
[/block]
Once you have clicked on the activation url, you'll be redirected to Admin Dashboard. Here is where all of your *instances* will be. You can think of instances as projects. When creating a new instance you are creating a new project in Syncano. You can create a new instance by following the steps in the image below. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/j49ghsgQTzefpyrtPuKp",
        "add_instance_0.png",
        "1439",
        "749",
        "#254472",
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
        "https://www.filepicker.io/api/file/AboKO06rRG6f4N8q79ux",
        "add_instance_1.png",
        "1278",
        "658",
        "#ee4a3e",
        ""
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Since an Instance can be shared between Syncano Admins, Instance names have to be unique across all accounts."
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Limits",
  "body": "Maximum number of Instances that an administrator can be an owner of is `16`"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Getting an API Key"
}
[/block]
When accessing an instance in the Syncano API, you can use API Keys that belong to that instance. You can find them by clicking on the instance, and then clicking the API keys selection as shown in the picture below. Here, you can add new API keys to that instance as well as view all existing keys. 

When trying to connect to an instance with one of our libraries, the API key is passed as a parameter and serves as the password for connecting to your instance. There is more on that right ahead. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/ALC5AfEfQAinUcNMsdAj",
        "Get_API_Key.png",
        "1280",
        "656",
        "#264472",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Connecting from a Library"
}
[/block]
Once you have your API Key, you can connect from the library of your choice:
[block:code]
{
  "codes": [
    {
      "code": "# In cURL, you have to pass API Key with every call you make, so e.g. to list instances associated with API Key, you can use\n\ncurl -X GET \\\n-H \"X-API-KEY: <API_KEY>\" \\\n\"https://api.syncano.io/v1.1/instances/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nsyncano.connect(api_key='API_KEY')\n\n# returns a registry object. ",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //Common JS only\nvar syncano = new Syncano({apiKey:'API_KEY', instance:'INSTANCE_NAME'});\n",
      "language": "javascript"
    },
    {
      "code": "new SyncanoBuilder().apiKey(\"YOUR API KEY\").instanceName(\"YOUR INSTANCE\")\n     .androidContext(context).setAsGlobalInstance(true).build();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "#import <Syncano.h>\n//...\nSyncano *myInstance = [Syncano sharedInstanceWithApiKey:@\"YOUR_API_KEY\" instanceName:@\"INSTANCE_NAME\"];\n// interact with your Instance!",
      "language": "objectivec"
    },
    {
      "code": "//in bridging header\n#import <Syncano.h>\n\n//in your class file\nlet myInstance = Syncano.sharedInstanceWithApiKey(\"YOUR_API_KEY\", instanceName: \"INSTANCE_NAME\")\n// interact with your Instance!",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "require 'syncano'\n\nconnection = Syncano.connect(api_key: 'YOUR_API_KEY')",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Where to go next"
}
[/block]

* You can check out available [Syncano Libraries](doc:syncano-libraries) 
* You can test the Syncano API in the [HTTP API Reference](http://docs.syncano.com/v0.1.1) section.
* To get a deeper understanding of Syncano concepts you can go through the chapters in the developer manual.