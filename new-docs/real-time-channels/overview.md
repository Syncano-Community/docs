---
title: "Overview"
excerpt: ""
---
Chapter sections:
1. [What are real-time Channels?]()
2. [Why are Channels important?]()
3. [How do you use Channels?]()

[block:api-header]
{
  "type": "basic",
  "title": "What are real-time Channels?"
}
[/block]

Channels are a way of providing real-time communication functionality in Syncano. Users can subscribe to Channels in order to get notifications about changes that happen to Data Objects connected to those Channels.

[block:callout]
{
  "type": "info",
  "body": "Let's imagine a family album application, where users (family members) can upload photos. Every user would like to know if one of the family members uploaded a new photo to the album. To do that, the user subscribes to a channel called `new_photo`. Upon creation, Data Objects with a photograph file are connected to the `new_photo` channel. This way users connected to the `new_photo` channel instantly receive a notification about a new photo added."
}
[/block]

Apart from the possibility to subscribe to changes to Data Objects, Users can be granted the ability to send custom notification messages. The most common use case for this functionality would be a chat application (described more in depth in the [publishing custom notification messages](#publishing-custom-notification-messages) and the [rooms](#rooms) sections of this chapter).

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
  "title": "Why are Channels important?"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "How do you use Channels?"
}
[/block]