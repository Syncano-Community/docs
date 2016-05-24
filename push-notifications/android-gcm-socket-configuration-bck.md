---
title: "Android - GCM Socket Configuration - bck"
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
2. Get a sample push notification application
2. Install and configure Android Studio
3. Create your app in Google Developers Console
4. Run the application on AVD and get the device registration id
5. Configure Syncano Push Notification Sockets
6. Add a device through the Syncano Dashboard
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
  "title": "Get a sample Push Notification application"
}
[/block]
We've prepared a sample application that can be used to test the Push Notification Sockets for GCM. In order to get it, go to your favorite terminal app and paste this line:
[block:code]
{
  "codes": [
    {
      "code": "git clone https://github.com/Syncano/google-services-example.git",
      "language": "shell",
      "name": "https"
    },
    {
      "code": "git clone git@github.com:Syncano/google-services-example.git",
      "language": "shell",
      "name": "ssh"
    }
  ]
}
[/block]

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
3. Now let's open up the sample project.
- Click File -> Open
- In the pop up dialog choose /google-services-example/android/gcm/

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/vP3EG5pRPCnsuC20KKj9",
        "gcm_00.png",
        "850",
        "950",
        "#1c3458",
        ""
      ]
    }
  ]
}
[/block]
- Click "OK" Button

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
4. Copy the `google-services.json` file to  `/google-services-example/android/gcm/app/` folder
[block:api-header]
{
  "type": "basic",
  "title": "Run the application on AVD and get the Device Registration ID"
}
[/block]
 1. Now you can run the application. Click "Play" button and select the default device emulator (Nexus 5 API 23 x86) as a deployment target:

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/9ZB2YEknThOCL5x7p6hd",
        "gcm_07_run.png",
        "2546",
        "1382",
        "#164a87",
        ""
      ]
    }
  ]
}
[/block]
2. Your app should print a log into the Android Studio console that will look more or less like this:
[block:code]
{
  "codes": [
    {
      "code": "12-04 17:20:03.154 5238-5272/gcm.play.android.samples.com.gcmquickstart I/RegIntentService: GCM Registration Token: dOBzwzC0vmA:APA91bEG73AmsFE2hNanz8G5wpNQFUWV25aBVPxLZ2JU9IOQRJyE7uABlW-gG6fgG1WieR9qaUGnmgdu7B745flp0P5yiHabRIZkUohLwWmZSuar471kS-C3_3GhUea9na0lRlT_P8CP\n",
      "language": "json",
      "name": "Android Studio Console"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "GCM Registration Token",
  "body": "Line after `GCM Registration Token:` phrase is the device registration id. The devices that have your app installed will be recognized by it. Copy this id as it will be needed to send a push notification message to the device."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Configure Syncano Push Notification Sockets"
}
[/block]
That was all there is to do in Android Studio. Now it time to connect Syncano to the GCM service. In order to do that, go to the [Syncano Dashboard](dashboard.syncano.io). If you don't have an account yet please follow [this guide](http://docs.syncano.io/docs/getting-started-with-syncano). Otherwise: 

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
2. On GCM Configuration Screen paste your GCM Server API Key you copied when configuring an app in Google Developers Console (it was described [here](#section--create-your-app-in-google-developers-console-)) 
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

[block:api-header]
{
  "type": "basic",
  "title": "Add a device through the Syncano Dashboard"
}
[/block]

Ok, now it's time to add your first device. 
1. Click on the "Devices" link. You'll be taken to the Devices list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/y7aEuF4OTfWJH4dIorJA",
        "gcm_syncano_config_list.png",
        "2035",
        "160",
        "#838ac2",
        ""
      ]
    }
  ]
}
[/block]
2. Click on "ADD" Button. You'll see a Dialog window that'll allow you to add a device:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/sq8GXnTSxC3uaaUPUbKY",
        "gcm_syncano_add_03.png",
        "1638",
        "1305",
        "#2b527d",
        ""
      ]
    }
  ]
}
[/block]
- Choose a label for the device
- In `Device registration id` field paste the ID that you obtained when running the sample app in Android Studio
- You can add a Syncano User ID if you want the device to be connected to a registered User of your app (you can find more info on managing users [here](doc:user-management) )
- In metadata field you can pass a json object with some additional info about the device

3. Click "Confirm" Button

Great! A new device has been added. That's it for the configuration part. If you'd like to know how to send push notification messages go to [Sending Push Notifications](doc:sending-push-notifications) chapter.