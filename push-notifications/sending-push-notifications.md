---
title: "Sending Push Notifications"
excerpt: ""
---
[block:callout]
{
  "type": "warning",
  "title": "BETA Feature",
  "body": "Push Notifications are an experimental feature. If you find any issues, please write to us at [support@syncano.com](mailto:support@syncano.com)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Chapter Contents"
}
[/block]
1. [Overview](#overview)
2. [ Sending Push Notification Messages to Android Devices](#sending-push-notification-messages-to-android-devi)
3. [Sending Push Notification Messages to iOS Devices
](#sending-push-notification-messages-to-ios-devices) 
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This chapter will show you how to send notification messages to iOS and Android devices.
[block:callout]
{
  "type": "warning",
  "title": "Configuration",
  "body": "To be able to send push notifications to Android or iOS devices you'll need to configure corresponding Sockets first! You can read how to do this here:\n- [GCM Socket Configuration (Android)](http://docs.syncano.io/v1.1/docs/push-notification-sockets-android)\n- [APNs Socket Configuration (iOS)](http://docs.syncano.io/v1.1/docs/push-notification-sockets-ios)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Sending Push Notification Messages to Android Devices"
}
[/block]
1. Select Android Devices from the left menu.
2. Click on the list item dropdown and choose "Send Message" option. (Alternatively you can select multiple devices)
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/srDl3bsqRau1Of6pS6wy",
        "gcm_syncano_send_00.png",
        "2558",
        "1308",
        "#274a7b",
        ""
      ]
    }
  ]
}
[/block]
3. In the Dialog select:
    - The Certificate type. `Development` will send messages to your test devices and `Production` is reserved for published applications.
    - Type a name of your app that should be displayed along with the message
    - Type message content
4. Click "Confirm" button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/6Wq43WGdRsO49eOU6Y8z",
        "gcm_syncano_send_01.png",
        "2558",
        "1308",
        "#277ed3",
        ""
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Hint!",
  "body": "If you want to send a message to a device that has our example app installed:\n1. Toggle the `JSON message` field\n2. Paste the below code to the editor:"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"data\":\n    {\n      \"message\": \"this line will be displayed in logcat\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Alternatively, you can use a cURL the send a notification message:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{ \"content\": { \"environment\": \"development\", \"registration_ids\":[\"REGISTRATION_ID\"], \"data\": {\"message\":\"sample message\"} } }'\n\"https://api.syncano.io/v1.1/instances/INSTANCE/push_notifications/gcm/messages/\"",
      "language": "curl"
    }
  ]
}
[/block]

That's it! If you are testing the workflow your message should show up in the logcat console!
[block:api-header]
{
  "type": "basic",
  "title": "Sending Push Notification Messages to iOS Devices"
}
[/block]
1. Select iOS Devices from the left menu.
2. Click on the list item dropdown and choose "Send Message" option. (Alternatively you can select multiple devices)

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/e2Rq5EeNQRmjUSvFDbbv",
        "apns_send_message_00.png",
        "1439",
        "739",
        "#264b7c",
        ""
      ]
    }
  ]
}
[/block]
3. In the Dialog select:
    - Sandbox toggle. If it's `on`, the messages will be sent to your test devices. Toggle it to `off` status will allow sending to real-life production devices. 
    - Type a name of your app that should be displayed along with the message
    - Type message content
4. Click "Confirm" button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/INzt1BFhSBULdbCRcinA",
        "apns_send_message_01.png",
        "1439",
        "741",
        "#336ba4",
        ""
      ]
    }
  ]
}
[/block]
Your message will be sent to the registered iOS devices.


[block:api-header]
{
  "type": "basic",
  "title": "Listing Push Notification Messages"
}
[/block]
Currently there's no option to view the Push Notification history from the Dashboard but you can query the API Directly:
- Go to API Reference and use "Try it out" feature
[GCM Messages Endpoint](http://docs.syncano.io/v0.1.1/docs/gcm-messages-list)
[APNS Messages Endpoint](http://docs.syncano.io/v0.1.1/docs/apns-messages-list)

- You can also use cURL:

GCM

[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\nhttps://api.syncano.io/v1/instances/INSTANCE_NAME/push_notifications/gcm/messages/ | python -m json.tool",
      "language": "curl"
    }
  ]
}
[/block]
APNS
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\nhttps://api.syncano.io/v1/instances/INSTANCE_NAME/push_notifications/apns/messages/ | python -m json.tool",
      "language": "curl"
    }
  ]
}
[/block]