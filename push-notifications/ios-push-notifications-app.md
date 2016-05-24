---
title: "iOS - Push Notifications App"
excerpt: "Build a basic app and learn how to use Syncano's Push Notification Socket"
---
[block:callout]
{
  "type": "warning",
  "body": "Push Notifications are an experimental feature. If you find any issues, please write to us at [support@syncano.com](mailto:support@syncano.com)",
  "title": "BETA feature"
}
[/block]
Before you can start building and testing your app on device, you will need:
- Apple Developer Account ([find it here](https://developer.apple.com/account/))
- Syncano Account and Instance ([get started here](getting-started-with-syncano))
- Apple Push Notifications Certificates and Syncano's Push Notifications Socket configured ([learn how to do it here](push-notification-sockets-ios))

Because you cannot test Push Notifications on the iOS Simulator, you will also need working device. 

If you have all of that, proceed to our short tutorial!

1. [Get Syncano API Instance name and API Key](#1-get-syncano-api-instance-name-and-api-key)
2. [Run sample app](#2-run-sample-app)
3. [Build your own app](#3-build-your-own-app)
[block:api-header]
{
  "type": "basic",
  "title": "1. Get Syncano API Instance name and API Key"
}
[/block]
If you don't have a Syncano Instance yet, [follow our getting started](getting-started-with-syncano) to create one.

1. Click on your instance to enter it. 
2. Copy its name and note it down - we will need it later.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/9HIvuVDmRPq9HKvjoJGf",
        "Instance_name.jpg",
        "1230",
        "1472",
        "#264774",
        ""
      ],
      "sizing": "smart"
    }
  ]
}
[/block]
3. Create an API Key with `User registration` permissions. (go to [Authentication & API Keys](authentication#section-creating-an-api-key) to learn how to do it)

When your API Key is created, copy it and save somewhere - we will need it in a second.
[block:api-header]
{
  "type": "basic",
  "title": "2. Run sample app"
}
[/block]
1. Clone our app [from GitHub](https://github.com/Syncano/syncano-apns-example) or [download zipped repository](https://github.com/Syncano/syncano-apns-example/archive/master.zip)
2. Open project in XCode
3. Find `AppDelegate.swift` file
4. Take Instance name and API Key you obtained in previous part and paste them into Syncano configuration
[block:code]
{
  "codes": [
    {
      "code": "let syncano = Syncano.sharedInstanceWithApiKey(\"PASTE_HERE_YOUR_API_KEY\", instanceName: \"PASTE_HERE_YOUR_INSTANCE_NAME\")",
      "language": "swift"
    }
  ]
}
[/block]
5. Launch your app
6. iOS will ask your to allow Push Notifications - please do, otherwise you won't be able to receive any notifications
7. On first screen you will see username and password text fields. If you have your credentials, type them and log in. If not, type new username and password, and sign up using `Register` button (it doesn't matter what account will be used here)
8. After you log in, your iOS device will be registered on Syncano
9. [Send yourself a message](sending-push-notifications#sending-push-notification-messages-to-ios-devices) and see how well it works!

[block:api-header]
{
  "type": "basic",
  "title": "3. Build your own app"
}
[/block]
1. Start from creating a new XCode project
* In your app project settings, enable the ability to receive Push Notifications and Background Mode
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/P8ImpeuwQYyh4aOR7nvA",
        "Enable_push_and_background.jpg",
        "2824",
        "1586",
        "#2c4764",
        ""
      ]
    }
  ]
}
[/block]
2. Save and close it (we'll get back to it in a second)
3. Add our `syncano-ios` library to your project ([read our guide on how to do it](ios#installing-the-syncano-ios-library))
4. Open newly crated workspace file in XCode (it was added after running `pod install` command)
5. In the `AppDelegate.swift` file, import Syncano library
[block:code]
{
  "codes": [
    {
      "code": "import syncano_ios",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
6. Create a Syncano connection (insert your Instance name and API Key)
(In this app example you will need an API Key with Ignore ACL permissions, as we don't implement user login feature in this app -- go to [Authentication & API Keys](authentication#section-creating-an-api-key) to learn how to get this key)
[block:code]
{
  "codes": [
    {
      "code": "class AppDelegate: UIResponder, UIApplicationDelegate {\n    var window: UIWindow?\n    let syncano = Syncano.sharedInstanceWithApiKey(\"YOUR_API_KEY\", instanceName: \"YOUR_INSTANCE_NAME\")\n    ...\n}",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
7. Add functions to register your iOS Device with Syncano
[block:code]
{
  "codes": [
    {
      "code": "func registerDeviceOnSyncano(deviceToken token: NSData) {\n\tlet device = SCDevice(tokenFromData: token)\n  device.label = \"My test app device\"\n  device.saveWithCompletionBlock { error in\n  \tguard error == nil else {\n    \t//if error says device was already registered (exists) - it's ok\n      //it probably means we launched an app for the second time\n      print(error.userInfo[\"com.Syncano.response.error\"]?.description)\n      return\n    }\n  }\n}",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
8. Add a function that will display push notifications on your screen
[block:code]
{
  "codes": [
    {
      "code": "func showAlert(userInfo: [NSObject:AnyObject]) {\n\t//hide any alert that might be already presented\n\tself.window?.rootViewController?.dismissViewControllerAnimated(true, completion: nil)\n  //get notification title\n  let title = userInfo[\"aps\"]?[\"alert\"]?![\"title\"]! as? String?\n  //get notification body\n  let body = userInfo[\"aps\"]?[\"alert\"]?![\"body\"]! as? String?\n  //create an alart\n  let alert = UIAlertController(title: title!, message: body!, preferredStyle: UIAlertControllerStyle.Alert)\n  alert.addAction(UIAlertAction(title: \"OK\", style: UIAlertActionStyle.Default, handler: nil))\n  //show alert on screen\n  self.window?.rootViewController?.presentViewController(alert, animated: true, completion: nil)\n}",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
9. Add functions to register for receiving Push Notifications with Apple to your `AppDelegate.swift` file
[block:code]
{
  "codes": [
    {
      "code": "func initializeNotificationServices() {\n\tlet settings = UIUserNotificationSettings(forTypes: [.Sound, .Alert, .Badge], categories: nil)\n  UIApplication.sharedApplication().registerUserNotificationSettings(settings)\n        UIApplication.sharedApplication().registerForRemoteNotifications()\n}\n    \nfunc application(application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {\n\tregisterDeviceOnSyncano(deviceToken: deviceToken)\n}\n    \nfunc application(application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: NSError) {\n\tprint(\"Register for remote notifications failed: \\(error.description)\")\n}\n    \nfunc application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject], fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void) {\n\tprint(\"Received notification: \\(userInfo)\")\n  self.showAlert(userInfo)\n  completionHandler(UIBackgroundFetchResult.NoData)\n}",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
* As you can see, after we receive device token from Apple servers, we pass it to Syncano by calling previously added `registerDeviceOnSyncano(deviceToken: deviceToken)` function
* Also, when notification is received, we call `self.showAlert(userInfo)` to display it on the screen

10. Initialize notifications services, by calling `initializeNotificationServices()` method inside `func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool` function
[block:code]
{
  "codes": [
    {
      "code": "func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {\n\t// Override point for customization after application launch.\n  initializeNotificationServices()\n  return true\n}",
      "language": "swift",
      "name": "AppDelegate.swift"
    }
  ]
}
[/block]
That's it! Now, when your app is running, [send yourself a test message](http://docs.syncano.io/v1.1/docs/sending-push-notifications#sending-push-notification-messages-to-ios-devices) to confirm it works!

Our demo app you tested in ["Run sample app" section](#2-run-sample-app) includes a bit more than we described here - it also implements [users management](ios#section-handling-users) module -- but it's not required and you can use only Push Notifications feature of Syncano if you'd like.

If you have any questions or feedback - let us know by [sending email to support@syncano.io](mailto:support@syncano.io) or [join our Slack](http://syncano-community.github.io/slack-invite/) and talk to our developers and other Syncano users!