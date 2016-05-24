---
title: "Android"
excerpt: ""
---
## Chapter Contents:
1. [Overview](#overview)
2. [Installing the Syncano Android Library](#installing-the-syncano-android-library)
3. [Using the Syncano Library](#using-the-syncano-library)
4. [Support](#section-support)
5. [License](#section-license)
[block:callout]
{
  "type": "info",
  "title": "Sockets release",
  "body": "We recently released a new version of our API that uses Scripts instead of CodeBoxes and Script Endpoints instead of Webhooks.\nFor us, it's not just about the name change - you can [read about it](https://www.syncano.io/blog/what-are-we-cooking-up-with-sockets/) on our blog.\nFor now, you can keep using old methods, but they got Deprecated in the newest version of Android library."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This guide will walk you through the installation steps of the Syncano Library for Android as well as give you a couple of usage examples.

If you don't have a Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano). 
[block:api-header]
{
  "type": "basic",
  "title": "Installing the Syncano Android Library"
}
[/block]
1. [Source](#section-source)
2. [Android Studio](#section-android-studio)
3. [Create a New Android Project](#section-create-a-new-android-project)
4. [Adding the Syncano Library to a Project](#section-adding-the-syncano-library-to-a-project)

Source
======

We recommend using the syncano-android library through jCenter, Maven, or using `jar` files. 

You can always grab the source from our [GitHub](https://github.com/Syncano/syncano-android/releases).

Android Studio
===========

If you don’t have Android Studio already, you can find the newest version [here](http://developer.android.com/sdk/index.html).

Create a New Android Project
=====================

1. Open the Android Studio IDE
2. Choose **File -> New Project...**
3. Type in the application name and the company domain and press **Next**. We’ll use **SyncanoProject** as an app name, and **com.syncano** as a company domain in the examples below
4. Choose proper platform and SDK for your project. We’ll just stick with Phone and Tablet and default the SDK version. Click **Next** when you're done.
5. Select an activity. We’ll use **Blank activity** for the example project
6. Name your activity and layout or leave the default ones and press **Finish**

Adding the Syncano Library to a Project
===========================

##### Using Gradle

Syncano library is now also available on jCenter. To install it, add one line inside your project gradle file (dependencies section) -- and you will be able to use it out of the box without the `jar` file.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n  compile 'io.syncano:library:4.1.0'\n}",
      "language": "groovy",
      "name": "Gradle"
    }
  ]
}
[/block]
##### Using Jar

First, you need to get the `jar` file.
You can get it from the [Maven](http://mvnrepository.com/artifact/io.syncano/library/) section.

Since it's not possible to embed one jar into another, our library requires gson-2.6.2.jar to work. We deliver it on GitHub with syncano-android. It's also available on the [Maven](http://mvnrepository.com/artifact/com.google.code.gson/gson/2.6.2) repository.

1. Put the syncano-android-4.x.x.jar and gson-2.6.2.jar in the **SyncanoProject -> app -> libs directory** (the 'syncano.android-4.**x.x**' file name will contain the current version of the library)
2. Sync your project with gradle files. To do so, choose **Tools -> Android -> Sync Project with Gradle Files** from the menu.
[block:api-header]
{
  "type": "basic",
  "title": "Using the Syncano Library"
}
[/block]
1. [Permissions](#section-permissions)
2. [Importing the Syncano Library](#section-importing-the-syncano-library)
3. [Synchronous and Asynchronous requests](#section-synchronous-and-asynchronous-requests)
3. [Connecting to Syncano](#section-connecting-to-syncano)
4. [Adding your class](#section-adding-your-class)
5. [Download the most recent Objects](#section-download-the-most-recent-objects)
   1. [Synchronous](#section-synchronous)
   2. [Asynchronous](#section-asynchronous)
6. [Get a single Data Object](#section-get-a-single-data-object)
7. [Creating a new Data Object](#section-creating-a-new-data-object)
8. [Modify an Object](#section-modify-an-object)
9. [Using `Reference` type field](#section-using-reference-type-field)
10. [Clear field](#section-clear-field)
11. [Download objects matching specified criteria](#section-download-objects-matching-specified-criteria)
   1. [Where and OrderBy](#section-where-and-orderby) 
   2. [Paging](#section-paging)
   3. [Fields filtering](#section-fields-filtering)
12. [Delete a Data Object](#section-delete-a-data-object)
13. [Data Endpoints](#section-data-endpoints)
14. [Estimate count](#section-estimate-count)
15. [Scripts](#section-scripts)
16. [Script endpoints](#section-script-endpoints)
17. [Classes](#section-classes)
18. [Response codes and error messages](#section-response-codes-and-error-messages)
19. [Synchronization and Channels](#section-synchronization-and-channels)
20. [Handling users](#section-handling-users)

Permissions
=========

You'll need to add certain permissions to your project settings. Navigate to AndroidManifest.xml file  by using the menu on the left. In Android Studio, it should be in **app -> src -> main -> AndroidManifest.xml**.

Open the file and paste these two lines inside:
[block:code]
{
  "codes": [
    {
      "code": "<uses-permission android:name=\"android.permission.INTERNET\" />\n<uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\" />",
      "language": "xml"
    }
  ]
}
[/block]
Importing the Syncano Library
====================

When writing the code using Syncano objects, the IDE will suggest proper modules to be imported in your file. In general, everything is placed under **com.syncano.library**.* package, but you should rely on the IDE suggestions to provide proper imports.

Synchronous and Asynchronous requests
================================

All requests might be synchronous or asynchronous. For simplicity, further examples (with the exception of the downloading the most recent data object) will show the synchronous implementation.

- **Why asynchronous?** Android doesn't allow network operations on the UI thread. That's why asynchronous requests are better to use from an Activity or Fragment. Our asynchronous requests are queued and executed using the Thread pool - which make calls faster.
- **Why synchronous?** If you want to download data on your thread, it may be easier to use synchronous requests.

Connecting to Syncano
=================

It's recommended to create Syncano instance using SyncanoBuilder.
[block:code]
{
  "codes": [
    {
      "code": "Syncano syncano = new SyncanoBuilder().apiKey(\"YOUR API KEY\").instanceName(\"YOUR INSTANCE\").androidContext(context).build();",
      "language": "java"
    }
  ]
}
[/block]
You can set a singleton instance used in different places of the app.
- Enter your Application *.java class file to where you want to add Syncano support.
- If you do not have your custom application you can create new one that looks like:
[block:code]
{
  "codes": [
    {
      "code": "public class MyApplication extends Application\n{\n  @Override\n  public void onCreate()\n  {\n   super.onCreate();\n   new SyncanoBuilder().apiKey(\"YOUR API KEY\").instanceName(\"YOUR INSTANCE\")\n     .androidContext(context).setAsGlobalInstance(true).build();\n  }\n}",
      "language": "java"
    }
  ]
}
[/block]
And the next thing you need to do is to open the AndroidManifest.xml file and add a reference to MyApplication:
[block:code]
{
  "codes": [
    {
      "code": " <application android:icon=\"@drawable/icon\" android:label=\"@string/app_name\"\n    android:name=\".MyApplication\">",
      "language": "xml"
    }
  ]
}
[/block]
Adding your class
=============
[block:callout]
{
  "type": "info",
  "body": "Before you will use the class in your app, remember that the class should already exist in Syncano. You can add your classes quickly by using our [Dashboard](https://dashboard.syncano.io/)!\n\nYou can read more about classes [here](doc:classes).",
  "title": "Using classes in Syncano"
}
[/block]
Now we will add the Java class you had implemented on Syncano. We will be using a simple Book class which only has two attributes: "title" and "subtitle", both being of type "string".

The Schema for this class would look like this:
[block:code]
{
  "codes": [
    {
      "code": "[\n  {\"type\": \"string\",\"name\": \"title\"},\n  {\"type\": \"string\",\"name\": \"subtitle\"}\n]",
      "language": "json"
    }
  ]
}
[/block]
Our Java class would look like:
[block:code]
{
  "codes": [
    {
      "code": "import com.syncano.library.annotation.SyncanoClass;\nimport com.syncano.library.annotation.SyncanoField;\nimport com.syncano.library.data.SyncanoObject;\n\n@SyncanoClass(name = \"book\")\nclass Book extends SyncanoObject {\n\n    public static final String FIELD_TITLE = \"title\";\n    public static final String FIELD_SUBTITLE = \"subtitle\";\n\n    @SyncanoField(name = FIELD_TITLE)\n    public String title;\n\n    @SyncanoField(name = FIELD_SUBTITLE)\n    public String subtitle;\n}",
      "language": "java"
    }
  ]
}
[/block]
As you can see, we are using Java Annotations to describe our class.
- `@SyncanoClass` - Used to mark class.
- `@SyncanoField` - Used to mark fields.

Field types and names should match those from **schema**.

Our class needs to inherit the `SyncanoObject` class. It will deliver standard fields as:
- id
- created at
- updated at
- revision

Download the most recent Objects
============================

### Synchronous

In your own Thread or on a Service, it is better to use synchronous calls.
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response;\n// on global Syncano instance\nresponse = Syncano.please(Book.class).get();\n// on specific Syncano instance\nresponse = Syncano.please(Book.class).on(syncano).get();\n\nList<Book> books = response.getData();",
      "language": "java"
    }
  ]
}
[/block]
### Asynchronous

Get a page of most recent data objects in the background. Handle the UI update in a callback. Difference is that you have to pass callback object.
[block:code]
{
  "codes": [
    {
      "code": "SyncanoListCallback<Book> callback = new SyncanoListCallback<Book>() {\n    @Override\n    public void success(ResponseGetList<Book> response, List<Book> result) {\n        // Request succeed.\n    }\n\n    @Override\n    public void failure(ResponseGetList<Book> response) {\n        // Something went wrong. Check error codes in response object.\n    }\n};\n\n// on global Syncano instance\nSyncano.please(Book.class).get(callback);\n// on specific Syncano instance\nSyncano.please(Book.class).on(syncano).get(callback);",
      "language": "java"
    }
  ]
}
[/block]
Get a single Data Object
===================

Now, if you'd only like to get a single Data Object, you can get it by id.
[block:code]
{
  "codes": [
    {
      "code": "//sync\nBook book = Syncano.please(Book.class).get(id).getData();\n// async\nSyncano.please(Book.class).get(id, new SyncanoCallback<Book>() {\n    @Override\n    public void success(Response<Book> response, Book result) {\n    }\n\n    @Override\n    public void failure(Response<Book> response) {\n    }\n});",
      "language": "java"
    }
  ]
}
[/block]
Creating a new Data Object
=====================

Below, we create a Data Object and fill it's data. Then we save the object on Syncano. In the response you will get a Data Object with fields id, createdAt, updatedAt and revision. After successfull request, created book object will be modified (it will get id etc).
[block:code]
{
  "codes": [
    {
      "code": "Book book = new Book();\nbook.title = \"John's life\";\nResponse<Book> response = book.save();\n// you can check if any response succeeded calling isSuccess()\nif (response.isSuccess()) {\n}",
      "language": "java"
    }
  ]
}
[/block]
Modify an Object
==================
Do it the same way as when creating object. When object's id is set, then library tries to update it, when id is null, it tries to create it.
[block:code]
{
  "codes": [
    {
      "code": "// id has to be set in an object\nbook.title = \"New Title\";\nbook.save();",
      "language": "java"
    }
  ]
}
[/block]
Using `Reference` type field
=======================================

If you'd like to use `Reference` type field in your class -- it's possible to do that in Android. 

For example, if you defined `Author` class in the same way as `Book` above, and now you wanted to have reference to author from book (and you defined its class schema e.g. through our [dashboard](doc:classes))  - you just need to add the field to your class.
[block:code]
{
  "codes": [
    {
      "code": "@SyncanoClass(name = Book.BOOK_CLASS)\npublic class Book extends SyncanoObject {\n    public static final String BOOK_CLASS = \"Book\";\n\n    public static final String FIELD_TITLE = \"book_title\";\n    public static final String FIELD_SUBTITLE = \"subtitle\";\n    public static final String FIELD_PAGES = \"total_pages\";\n    public static final String FIELD_AUTHOR = \"author\";\n\n    @SyncanoField(name = FIELD_TITLE, orderIndex = true)\n    public String title;\n    @SyncanoField(name = FIELD_SUBTITLE, filterIndex = true, orderIndex = true)\n    public String subtitle;\n    @SyncanoField(name = FIELD_PAGES)\n    public Integer pages;\n    @SyncanoField(name = FIELD_AUTHOR, filterIndex = true)\n    public Author author;\n\n    public Book(String title, String subtitle) {\n        this.title = title;\n        this.subtitle = subtitle;\n    }\n\n    public Book(String title, Author author) {\n        this.title = title;\n        this.author = author;\n    }\n\n    public Book() {\n    }\n}\n\n@SyncanoClass(name = Author.AUTHOR_CLASS)\npublic class Author extends SyncanoObject {\n    public static final String AUTHOR_CLASS = \"Author\";\n\n    public static final String FIELD_NAME = \"name\";\n    public static final String FIELD_IS_MALE = \"is_male\";\n    public static final String FIELD_BIRTH_DATE = \"birth_date\";\n\n    @SyncanoField(name = FIELD_NAME, filterIndex = true)\n    public String name;\n    @SyncanoField(name = FIELD_IS_MALE, filterIndex = true)\n    public Boolean isMale;\n    @SyncanoField(name = FIELD_BIRTH_DATE, filterIndex = true)\n    public Date birthDate;\n\n    public Author() {\n    }\n\n    public Author(String name) {\n        this.name = name;\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
By defining class as above, whenever you save your `book` object - it will save reference to `author` automatically.

Caution: if `author` object was just created in your app, and doesn't exist on Syncano yet - library will not save it on Syncano automatically.
[block:code]
{
  "codes": [
    {
      "code": "// proper way\nBook book = new Book();\nAuthor author = new Author();\nbook.author = author;\n// author created on Syncano first and has id set\nResponse<Author> authorSave = author.save();\nResponse<Book> bookSave = book.save();\n\n// wrong way\nbook = new Book();\nauthor = new Author();\nbook.author = author;\n// book will be saved but without author\n// or will return an error if author has id set, but doesn't exist on server\nbookSave = book.save();",
      "language": "java"
    }
  ]
}
[/block]
Clear field
=======================================
Syncano Library for Android send only data when value in object is available, so you can update only one field even if you do not know anything about current status of other fields. Basically that means if any value inside your object is null you do not change its value on syncano. 
[block:code]
{
  "codes": [
    {
      "code": "Book book = new Book();\nbook.title = \"John's life\";\nbook.pages = 1500;\nbook.save();\n\nbook.clearField(Book.FIELD_TITLE);\nbook.save();\n// title field should be empty now",
      "language": "java"
    }
  ]
}
[/block]
Sometimes you want to clear field in syncano for example when you want to remove object reference or if you want to set field to ‚not specified’. In this case you should use clearField method inside your syncano Object.


Download objects matching specified criteria
=======================================

There are a couple of ways to filter results while getting a Data Object list.
More detailed information about filtering can be found [here](doc:data-objects-filtering).
[block:callout]
{
  "type": "info",
  "body": "To be able to filter and sort based on chosen fields, please remember you have to first set an index on those fields. You can read more about it [here](doc:classes).",
  "title": "Indexes"
}
[/block]
### Where and OrderBy

Data Objects might be filtered by value of it's fields.
To do so, you need to use the `where` filter.

You can also use `orderBy` to sort your data. 
Aware: If you use  filter `orderBy` should be before`where`.

E.g. in the example below, we will ask for all books with ID greater than or equal to 10 and lesser than or equal to 15.
We also want to sort our Books by title, and receive them in ascending order.
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class).orderBy(Book.FIELD_TITLE).where().gte(Book.FIELD_ID, 10).lte(Book.FIELD_ID, 15).get();\n\n//You can also query by your reference field\nresponse = Syncano.please(Book.class).where().eq(Book.FIELD_AUTHOR, Author.FIELD_NAME, \"Adam\").get();",
      "language": "java"
    }
  ]
}
[/block]
### Paging
- `limit` - Determines page size.
[block:code]
{
  "codes": [
    {
      "code": "// Automatic paging. It downloads all objects from server in a loop. It will cause problems, when you have many objects.\nResponseGetList<Book> resp = Syncano.please(Book.class).getAll(true).get();\n\n// Manual paging with specified limit of elements per one single page\nresp = Syncano.please(Book.class).limit(10).get();\nwhile (resp.hasNextPage()) {\n    resp = resp.getNextPage();\n}",
      "language": "java"
    }
  ]
}
[/block]
### Fields filtering
Sometimes you want to get only a part of your data. E.g. if your books contained also all the book content, authors, number of pages etc. and you needed only the book ID and title (itsaves bandwith), you can do it with following code
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class)\n        .selectFields(FilterType.INCLUDE_FIELDS, Book.FIELD_ID, Book.FIELD_TITLE).get();",
      "language": "java"
    }
  ]
}
[/block]
Delete a Data Object
================

You'll only need the Data Object ID to delete it.
[block:code]
{
  "codes": [
    {
      "code": "Response<Book> response = syncano.deleteObject(Book.class, bookId).send();\n// or other way, when you have object with an id set\nresponse = book.delete();",
      "language": "java"
    }
  ]
}
[/block]
Data Endpoints
=====================
Data endpoints are very handy and Android library support all their features - you can filter on them and have the relations between objects expanded.
Check more about data endpoints itself in [docs](http://docs.syncano.io/docs/endpoints-data)

Let's start with creating our data endpoint, based on classes used before:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/CraXKjhYTe6F1kV0041h",
        "data_endpoint.png",
        "2475",
        "1526",
        "#2f5c90",
        ""
      ]
    }
  ]
}
[/block]
Our endpoint name is **"book_and_author"**. The following code fetches data from it:
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class).dataEndpoint(\"book_and_author\").get();",
      "language": "java"
    }
  ]
}
[/block]
You can easily filter or order on data endpoints:
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class).dataEndpoint(\"book_and_author\").orderBy(Book.FIELD_TITLE).get();  ",
      "language": "java"
    }
  ]
}
[/block]
Estimate count
================
You can also get the Data Objects count estimation when getting the Data Objects list. Syncano shows estimate count for Classes that have more than 1000 Data Objects. This is because we can't provide a precise count without affecting the performance of the platform.

You can append the Data Objects count property to the response by adding a ".estimateCount()" to  your request. The estimatedCount property will appear in the response. See the example at the bottom:
[block:code]
{
  "codes": [
    {
      "code": "// If you want to get estimated count with your data\nResponseGetList<Book> response = Syncano.please(Book.class).estimateCount().get();\nInteger count = response.getEstimatedCount();\n// If you want only estimated count of objects\nResponse<Integer> responseCount = Syncano.please(Book.class).getCountEstimation();\ncount = responseCount.getData();",
      "language": "java"
    }
  ]
}
[/block]
Scripts
=====

Read more about Scripts in our [Developer Manual](doc:snippets-scripts).

You shouldn't run a script from your application directly if you want to get a result immediately. It's better idea to use Script Endpoint, but it's possible:
[block:code]
{
  "codes": [
    {
      "code": "Response<Trace> response = new Script(scriptId).run();\nTrace trace = response.getData();\n// running script is asynchronous\n// you can check current execution status calling fetch() on trace later\ntrace.fetch();\nif (trace.getStatus() == TraceStatus.SUCCESS) {\n}",
      "language": "java"
    }
  ]
}
[/block]
Script Endpoints
=============

Read more about Script Endpoints in our [Developer Manual](doc:endpoints-scripts).
How to run a Script Endpoint:
[block:code]
{
  "codes": [
    {
      "code": "ScriptEndpoint se = new ScriptEndpoint(\"endpoint_name\");\nResponse<Trace> response = se.run();\n// with script endpoint you get result immediately, don't have to call fetch() on Trace\nString output = se.getOutput();",
      "language": "java"
    }
  ]
}
[/block]
In case your script endpoint uses your own (custom) response format, it can be parsed by the library. You can use the following code:
[block:code]
{
  "codes": [
    {
      "code": "ScriptEndpoint se = new ScriptEndpoint(\"endpoint_name\");\nResponse<MyResponse> response = se.runCustomResponse(MyResponse.class);\nMyResponse output = se.getCustomResponse();\n\n// if you want to parse result by yourself\nResponse<String> stringResponse = se.runCustomResponse();\nString stringOutput = se.getCustomResponse();\n\n//Your custom response class\npublic class MyResponse {\n    @SyncanoField(name = \"some_param\")\n    String someParam;\n}",
      "language": "java"
    }
  ]
}
[/block]
Classes
======

Read more about Classes in our [Developer Manual](classes). These are methods supported by the Android library
- create
- get one
- get list
- delete

You can find more usage examples in the library [tests](https://github.com/Syncano/syncano-android/tree/master/library/src/test/java/com/syncano/library) source code.

Response codes and error messages
============================

If your request failed, you can always check what was the reason by looking at the result code (you can compare the code with constants defined in the `Response` class).
[block:code]
{
  "codes": [
    {
      "code": "ResponseGetList<Book> response = Syncano.please(Book.class).get();\nif (response.isSuccess()) {\n    // Success\n} else {\n    Log.d(TAG, \"Result Code: \" + response.getHttpResultCode());\n    Log.d(TAG, \"Reason Phrase: \" + response.getHttpReasonPhrase());\n}\n\nresponse.getResultCode(); // Library error codes\nresponse.getError(); // Library error message\n\nresponse.getHttpResultCode(); // Http result code\nresponse.getHttpReasonPhrase(); // Http error message",
      "language": "java"
    }
  ]
}
[/block]
More about error codes can be found [here](http://docs.syncano.com/v0.1.1/docs/errors).

Synchronization and Channels
========================

You can read more about Real-Time Syncing and channels in the [Realtime communication](doc:realtime-communication) chapter.

Handling users
============

To find out more about the Users module on Syncano - read [User management](doc:user-management) chapter. Here, we'll quickly teach you how to create a new user, log the user in and get their profile data.

### Register new user
[block:code]
{
  "codes": [
    {
      "code": "User newUser = new User(\"username\", \"password\");\nResponse<User> response = newUser.register();",
      "language": "java"
    }
  ]
}
[/block]
### Log in user with password
[block:code]
{
  "codes": [
    {
      "code": "User user = new User(\"username\", \"password\");\nResponse<User> response = user.login();",
      "language": "java"
    }
  ]
}
[/block]
### Log in user with social backend
You should get social auth token by yourself, using methods provided by "single sign on" providers.
[block:code]
{
  "codes": [
    {
      "code": "//SocialAuthBackend.FACEBOOK\n//SocialAuthBackend.GOOGLE_OAUTH2\n//SocialAuthBackend.LINKEDIN\n//SocialAuthBackend.TWITTER\nUser user = new User(SocialAuthBackend.FACEBOOK, \"auth_token\");\nResponse<User> response = user.loginSocialUser();",
      "language": "java"
    }
  ]
}
[/block]
### Subclassing user and user profile

You can also subclass AbstractUser and Profile class - really handy when you want to expand user's profile and some extra properties like avatar. For more details on how that works, go to [User management](doc:user-management#user-profiles) and see below how to prepare your classes on Android.
[block:code]
{
  "codes": [
    {
      "code": "MyUser user = new MyUser(\"username\", \"password\");\nResponse<MyUser> responseLogin = user.login();\nif (responseLogin.isSuccess()) {\n    MyUserProfile profile = user.getProfile();\n    profile.avatar = new SyncanoFile(new File(\"/path/to/file\"));\n    Response<MyUserProfile> responseSaveAvatar = profile.save();\n}\n\npublic class MyUser extends AbstractUser<MyUserProfile> {\n    public MyUser(String login, String pass) {\n        super(login, pass);\n    }\n\n    public MyUser() {\n    }\n}\n\npublic class MyUserProfile extends Profile {\n    @SyncanoField(name = \"avatar\")\n    public SyncanoFile avatar;\n}",
      "language": "java"
    }
  ]
}
[/block]
Support
======

Now you’re ready to use Syncano in your Android project. We really hope you will enjoy working with our platform. If you have any issues or suggestions, just let us know at support@syncano.com.

License
======

Syncano’s Android Library is available under the MIT license. See the LICENSE file inside library sources for more info.