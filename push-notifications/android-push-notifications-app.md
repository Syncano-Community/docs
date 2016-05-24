---
title: "Android - Push Notifications App"
excerpt: ""
---
[block:callout]
{
  "type": "warning",
  "body": "Push Notifications are an experimental feature. If you find any issues, please write to us at [support@syncano.com](mailto:support@syncano.com)",
  "title": "BETA!"
}
[/block]
Before you can start building and testing your app on device, you will need:
- Syncano Account and Instance ([get started here](getting-started-with-syncano))
- Google Cloud Messaging and Syncano's Push Notifications Socket configured ([learn how to do it here](push-notification-sockets-android))
- `google-services.json` obtained from the steps done in the Config chapter

If you have all of that, proceed to our short tutorial!

1. [Get Syncano API Instance name and API Key](#1-get-syncano-api-instance-name-and-api-key)
2. [Configure your app](#2-configure-your-app)
3. [Run your app](#3-run-your-app)

[block:api-header]
{
  "type": "basic",
  "title": "1. Get Syncano API Instance name and API Key"
}
[/block]
1. Click on your instance to enter it. 
2. Copy its name and note it down - we will need it later.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/GY0TSntWScDkBKlNL3Zw",
        "Instance_name.jpg",
        "1230",
        "1472",
        "#274774",
        ""
      ]
    }
  ]
}
[/block]
3. Create an API Key with `Ignore ACL` permissions. Go to [Authentication & API Keys](http://docs.syncano.io/docs/authentication#section-creating-an-api-key) to learn how to do it. Alternatively, use the below curl call to create it.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"description\": \"GCM example app api key\", \"allow_anonymous_read\": false, \"allow_user_create\": true, \"ignore_acl\": true}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE/api_keys/\"",
      "language": "curl"
    }
  ]
}
[/block]
When your API Key is created, copy it and save somewhere - we will need it in a second.
[block:api-header]
{
  "type": "basic",
  "title": "2. Configure your app"
}
[/block]
You will need the application source first. You can get it from [syncano-gcm-example](https://github.com/Syncano/syncano-gcm-example) repository on Github or just copy the line below to your terminal and press enter:
[block:code]
{
  "codes": [
    {
      "code": "git clone git@github.com:Syncano/syncano-gcm-example.git",
      "language": "shell",
      "name": "ssh"
    },
    {
      "code": "https://github.com/Syncano/syncano-gcm-example.git",
      "language": "shell",
      "name": "https"
    }
  ]
}
[/block]
1. Copy your google-services.json to your projects folder. If you used this code, you should override a file in app directory.
2. Set your Syncano app API Key and Instance name in gradle.properties file.

[block:code]
{
  "codes": [
    {
      "code": "syncano_api_key=\"INSTANCE_API_KEY\"\nsyncano_instance=\"INSTANCE_NAME\"",
      "language": "java"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "3. Run your app"
}
[/block]
1. In the Android Studio, run the application. When run, the app will receive GCM registration id and register it on Syncano (You should see it in under **Push Devices** -> **Android Devices** in the Dashboard). It's also written to logcat.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/bVb0cm8uSsq18oCbdyJr",
        "gcm_devices_list.png",
        "2558",
        "1310",
        "#254a7c",
        ""
      ]
    }
  ]
}
[/block]
2. Send GCM messages to chosen registration ids. You can learn how to do it in [Sending Push Notifications](sending-push-notifications) chapter. You can also send a message by making a curl call:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{ \"content\": { \"environment\": \"development\", \"registration_ids\":[\"REGISTRATION_ID\"], \"data\": {\"message\":\"sample message\"} } }' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/push_notifications/gcm/messages/\" ",
      "language": "curl"
    }
  ]
}
[/block]
3. Messages should appear in your Android notifications bar.
[block:api-header]
{
  "type": "basic",
  "title": "Api keys used in this example"
}
[/block]
- Syncano Account API key - It is an administrator key, that can make any changes in his Syncano Instances.
- Syncano Instance API key - It's configured api key, it is hardcoded to the app and allowed to do specific things.
- GCM Server API key - Key that is necessary to communicate with GCM servers, used by Syncano, not an app.
- Registration id / token - It's an id given by GCM to specific app on specific device. Using this ids you can send messages to chosen devices.
[block:api-header]
{
  "type": "basic",
  "title": "Syncano related code in the app"
}
[/block]
Gradle files
[block:code]
{
  "codes": [
    {
      "code": "syncano_api_key=\"INSTANCE_API_KEY\"\nsyncano_instance=\"INSTANCE_NAME\"",
      "language": "json",
      "name": "gradle.properties"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    compile 'io.syncano:library:4.1.0'\n}",
      "language": "json",
      "name": "build.gradle"
    }
  ]
}
[/block]
SampleApplication.java - Initiating Syncano singleton
[block:code]
{
  "codes": [
    {
      "code": "new SyncanoBuilder().androidContext(getApplicationContext()).apiKey(BuildConfig.API_KEY).instanceName(BuildConfig.INSTANCE_NAME).setAsGlobalInstance(true).useLoggedUserStorage(true).build();",
      "language": "java",
      "name": "SampleApplication.java"
    }
  ]
}
[/block]
GetGCMTokenTask.java - Registering device registration id
[block:code]
{
  "codes": [
    {
      "code": "Syncano.getInstance().registerPushDevice(new PushDevice(token)).send();",
      "language": "java",
      "name": "GetGCMTokenTask.java"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "More information"
}
[/block]
- [GCM app example from Google](https://github.com/googlesamples/google-services/tree/master/android/gcm)
- For questions not related to Syncano, but to GCM itself, check the [Google Cloud Messaging Docs](https://developers.google.com/cloud-messaging/)
- [Sending Push Notifications](sending-push-notifications)