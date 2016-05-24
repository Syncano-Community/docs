---
title: "iOS - APNs Socket Configuration"
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
2. [Generate a Certificate Signing Request File](#generate-a-certificate-signing-request-file)
3. [Create an App ID](#create-an-app-id)
4. [Configure an App ID to use Push Notifications](#configure-an-app-id-to-use-push-notifications)
5. [Create the Provisioning Profile](#create-the-provisioning-profile)
6. [Configuring APNs Socket on Syncano](#configuring-apns-socket-on-syncano)
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

To be able to send push notifications to Apple devices, you'll need to generate an SSL Certificate. Syncano will use this certificate to send Push Notifications to the application identified by the App ID. Below you can read how to configure this functionality.
[block:api-header]
{
  "type": "basic",
  "title": "Generate a Certificate Signing Request File"
}
[/block]
To authenticate the creation of the SSL certificate, you need a certificate signing request file.

1. Open Keychain Access on your Mac (`CMD` + `Space` -> Keychain Access -> `Enter`).
2. On the Keychain Access app menu, navigate to: Keychain Access -> Certificate Assistant -> Request a Certificate From a Certificate Authority....
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/q8TEEh2aRcSiZ44fKUhb",
        "apns_ssl_cert_00.png",
        "2558",
        "1502",
        "#894439",
        ""
      ]
    }
  ]
}
[/block]
3. Input your email address and name (leave the CA Email Address blank).
4. For Request is select Saved to disk, then click Continue to save the .certSigningRequest file to your Mac.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/3lLg4EySuWY5BzoCnS8A",
        "apns_ssl_cert_01.png",
        "1228",
        "870",
        "#94acc1",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Create an App ID"
}
[/block]
An App ID is an identifier that uniquely identifies an application. As a convention it is represented by a reversed domain (e.g. io.syncano.examples.apnsexample).

[block:callout]
{
  "type": "info",
  "body": "If you already have an App ID that you would like to use, make sure that it is an explicit App ID (it does not contain a wildcard) and skip this section."
}
[/block]
1. Go to the [Apple Developer Member Center](https://developer.apple.com/account/) and log in.
2. Click on the **Certificates Identifiers and Profiles**.
3. In the drop down menu on the top left corner, select **iOS, tvOS, watchOS**, then go to Identifiers -> App IDs.
4. Click the **+** button to create a new App ID.

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/6hCv4udDQZ60XRAJB9xe",
        "apns_ssl_cert_02.png",
        "2558",
        "1306",
        "#eeeeee",
        ""
      ]
    }
  ]
}
[/block]
5. To create the new App ID:
    - Input a Name for your App ID (e.g. Syncano Example App)
    - Choose an **App ID Prefix** (the default selection should be fine)
    - In the **App ID Suffix** section, select **Explicit App ID**, then input your **Bundle ID** (e.g. io.syncano.examples.apnsexample). The value of the Bundle ID should match the value that you are using in your app's `Info.plist`.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/5RzK7BVBTP2MC2cBeSe1",
        "apns_ssl_cert_03.png",
        "2556",
        "1308",
        "#3d9cfc",
        ""
      ]
    }
  ]
}
[/block]
6. In the **App Services** section, make sure that **Push Notifications** is checked.

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/lmDbF1rTkS1I9WepUofR",
        "apns_ssl_cert_04.png",
        "2558",
        "1274",
        "#27292c",
        ""
      ]
    }
  ]
}
[/block]
7. Click **Continue** and check that your input is correct:
    - The value of Identifier should match the concatenation of the values of the App ID Prefix and of the Bundle ID
    - **Push Notifications** should be **Configurable**
8. Click **Submit** to create the App ID.


[block:api-header]
{
  "type": "basic",
  "title": "Configure an App ID to use Push Notifications"
}
[/block]
To be able to send Apple Push Notifications you will have to configure the App ID settings:
 
1. Select the App ID that we created in a previous step and click **Edit**
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Ru8PKRnQ6OemlJsfKi7A",
        "apns_ssl_cert_05.png",
        "1474",
        "1158",
        "#20557c",
        ""
      ]
    }
  ]
}
[/block]
2. Scroll down to the **Push Notifications** section. You can choose to create development or production certificate. Since we are starting out, click **Create Certificate...** the development version section
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/r5dIryEIQiGq3t8awBYQ",
        "apns_ssl_cert_06.png",
        "1464",
        "982",
        "#3c9cfc",
        ""
      ]
    }
  ]
}
[/block]
3. You'll see a page titled **About Creating a Certificate Signing Request (CSR)**. Click **Continue** button to proceed to the next step
4. You should see a **Generate Your Certificate** page. Upload the .certSigningRequest file that you created in the first section of this guide
5. Click the **Generate** button. Once the SSL Certificate is ready, click **Download** button to save it on your machine
6. Locate the certificate that you generated (by default it should be in your **Downloads** folder). Double click it to install it in your keychain
7. Open your **Keychain Access**  app (`CMD` + `Space` -> type "Keychain" -> `Enter`). Go to **My Certificates** and locate the certificate that you have just added; it should be called Apple Development IOS Push Services: your.bundle.id
8. Right-click on the certificate and select **Export** option

[block:callout]
{
  "type": "danger",
  "title": "Export",
  "body": "Do not set a password when exporting a certificate, as it won't be possible to upload it to Syncano later on!"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/oLNo8ax5RwW5RuMF37So",
        "apns_ssl_cert_08.png",
        "1754",
        "1106",
        "#315b8e",
        ""
      ]
    }
  ]
}
[/block]
9.  Save the certificate as a .p12 file. When you're prompted to type a password to protect the exported certificate, confirm without typing in any password. You will be also asked about your Mac OS X password, to allow for exporting keys to disk - type it, and save `.p12. file on disk


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/99HrLFmOQMVnDEbGOHNQ",
        "apns_ssl_cert_09.png",
        "844",
        "452",
        "#32649e",
        ""
      ],
      "sizing": "80"
    }
  ]
}
[/block]
The certificate can now be used to configure push notifications on the Syncano Platform. 

If your application is production ready, please repeat the above section starting from step 2. You should choose **Create Certificate...** in the **Production SSL Certificate** Section instead.
[block:callout]
{
  "type": "warning",
  "body": "When developing your application, please make sure that `kGGLInstanceIDAPNSServerTypeSandboxOption` that is being passed to  `GGLInstanceID.tokenWithAuthorizedEntity:scope:options:handler:` is set to `true` for development and `false` for the production version of the app.",
  "title": "Important"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Create the Provisioning Profile"
}
[/block]
If you want to run and test an app that is not published on the App Store, you will need to create a Provisioning Profile.

[block:callout]
{
  "type": "info",
  "body": "A provisioning profile is a collection of digital entities that uniquely ties developers and devices to an authorized iPhone Development Team and enables a device to be used for testing.",
  "title": "Provisioning Profile"
}
[/block]
To create the Provisioning Profile:
1. Go to [Apple Developer Member Center](https://developer.apple.com/membercenter/index.action) and log in
2. Navigate to [Certificates, Identifiers and Profiles](https://developer.apple.com/account/ios/certificate).
3. In the drop down menu on the top left corner, select **iOS, tvOS, watchOS**, then scroll down and navigate to **Provisioning Profiles**.
4. Click the + button to create a new Provisioning Profile.
5. Select** iOS App Development** as provisioning profile type, then click Continue.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/XQrGTO0pTNaCS85Lnp38",
        "apns_provision_00.png",
        "1468",
        "1256",
        "#3f9ce0",
        ""
      ]
    }
  ]
}
[/block]
6. Select the **App ID** from the dropdown menu, then click **Continue**.
7. Select the iOS Development certificate of the App ID you have chosen in the previous step, then click **Continue**.
8. Select the iOS devices that you want to include in the Provisioning Profile, then click **Continue**. Make sure to select all the devices you want to use for your testing.
9. Name your profile and click **Generate** button.
10. Click **Download** to save the Provisioning Profile to your machine.
11. Double-click the Provisioning Profile file to install it.
[block:api-header]
{
  "type": "basic",
  "title": "Configuring APNs Socket on Syncano"
}
[/block]
In order to connect your application with Syncano:
1. Log in to the [Syncano Dashboard](https://dashboard.syncano.io/)
2. Go to Sockets view and click "ADD" button
3. Next, click "ADD" button that is next to the Push Notification Sockets and select APNS Socket
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/jMbxnbukTs6fFn1MOfNR",
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
4. On APNS Configuration Screen drop your Drag & Drop your development certificate (the one that you created in previous step).
5. Input your Bundle ID and click **Confirm**


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/b4Cbewd9S7CMFZqdzE6A",
        "apns_cert_up_00.png",
        "1752",
        "1304",
        "#2b537e",
        ""
      ]
    }
  ]
}
[/block]
Uff... That was a long config journey but now you should be up and ready for sending your users those marvelous notification messages!

- Check out [Syncano APNS Exmample](https://github.com/Syncano/syncano-apns-example) repository to learn how to implement some push functionality on the client side
- Go to Sending Messages chapter to learn how to send Push Notifications