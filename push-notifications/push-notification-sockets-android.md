---
title: "Android - GCM Socket Configuration"
excerpt: ""
---
[block:callout]
{
  "type": "warning",
  "title": "BETA Feature",
  "body": "Push Notifications are an experimental feature. If you find any issues, please write us at [support@syncano.com](mailto:support@syncano.com)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Chapter Contents"
}
[/block]
1. Overview
2. Install and configure Android Studio
3. Create your app in Google Developers Console
4. Configure Syncano Push Notification Sockets
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Push Notification Sockets allow for sending messages directly to your users devices. Thanks to this functionality, your users can be quickly informed about changes taking place within your application. You can also keep the users in a loop when it comes to new functionalities and services. Push notifications are also a great way of increasing user engagement. 

Currently Syncano offers the functionality of sending notifications to:
1. Android devices through GCM ([Google Cloud Messaging](https://developers.google.com/cloud-messaging/))
2. iOS devices through APNS ([Apple Push Notification Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html))

Below you'll find the walkthrough for the Google Cloud Messaging configuration.
[block:api-header]
{
  "type": "basic",
  "title": "Install and configure Android Studio"
}
[/block]
1. Download and install [Android Studio](http://developer.android.com/sdk/index.html?gclid=CK-ykbGjyckCFebUcgoda4cMLwo)
2. In Android Studio, go to Settings/Preferences -> System Settings -> Android SDK
Your SDK Platform should be Android 6.0 (Marshmallow) API level 23 and you should have following SDK Tools installed:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/VBf7Vx1sRNSLMMauqcle",
        "gcm_sdk.png",
        "1055",
        "662",
        "#253462",
        ""
      ]
    }
  ]
}
[/block]
(At this point Android Studio might inform you about some dependencies missing. Confirm their installation)
[block:api-header]
{
  "type": "basic",
  "title": "Create your app in Google Developers Console"
}
[/block]
1. Go to [Google Developers](https://developers.google.com/identity/sign-in/android/start?authuser=1) website and click "Get a configuration file" button. You'll be directed to a wizard that'll walk you through configuring an application using Google Services. Please make sure that you enable Cloud Messaging in `Choose services` step


2. Once you enable the GCM service, a Server API Key will show up. Copy it as it will be needed to configure Push Notification Sockets on Syncano
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/7rS4MVqsQKu1PmnZlAaY",
        "gcm_04_server_api_key.png",
        "1582",
        "366",
        "#b0b0b0",
        ""
      ]
    }
  ]
}
[/block]
3. You should click the "Continue to generate configuration files" button. On the next page click "Download google-services.json" button.
4. The `google-services.json` file will be needed to set up your client side app. We will discuss this in detail when running the example app in the [next chapter](android-push-notifications-app).

[block:api-header]
{
  "type": "basic",
  "title": "Configure Syncano Push Notification Sockets"
}
[/block]
Now it time to connect Syncano to the GCM service. In order to do that, go to the [Syncano Dashboard](http://dashboard.syncano.io). If you don't have an account yet please follow [this guide](http://docs.syncano.io/docs/getting-started-with-syncano). Otherwise: 

1. Go to Instance -> Sockets screen and click "ADD" Button next to Push Notification Socket and select "GCM Socket"

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/s2xiV8yfQ5m0GgpKFBj6",
        "gcm_syncano_add_00.png",
        "2558",
        "1306",
        "#254c80",
        ""
      ]
    }
  ]
}
[/block]
2. On GCM Configuration Screen paste your GCM Server API Key you copied when configuring an app in Google Developers Console (it was described [here](#create-your-app-in-google-developers-console)) 
3. Click "Confirm" button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/MCqD1WdjTJCOgLleg6A2",
        "gcm_syncano_add_01.png",
        "2558",
        "1308",
        "#2b67a0",
        ""
      ]
    }
  ]
}
[/block]
That's all when it comes to GCM configuration on Syncano. If you'd like to know how to handle push notifications on the client side, check out our app in [the next chapter](android-push-notifications-app).