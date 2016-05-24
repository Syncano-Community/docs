---
title: "iOS"
excerpt: ""
---
## Chapter Contents:
1. [Overview](#overview)
2. [Installing the Syncano iOS Library](#installing-the-syncano-ios-library)
3. [Using the Syncano Library](#using-the-syncano-library)
4. [Support](#section-support)
5. [License](#section-license)
[block:callout]
{
  "type": "info",
  "title": "Sockets release",
  "body": "We recently released a new version of our API that uses Scripts instead of CodeBoxes and Script Endpoints instead of Webhooks.\nFor us, it's not just about the name change - you can [read about it](https://www.syncano.io/blog/what-are-we-cooking-up-with-sockets/) on our blog.\nFor now, you can keep using old methods, and when we release new version of this library, we will update our docs here."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This guide will walk you through the installation steps of the Syncano Library for iOS as well as give you a couple of usage examples.

If you don't have a Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano).

For the full API listing, view the [syncano-ios reference](http://cocoadocs.org/docsets/syncano-ios/4.0.8/).
[block:api-header]
{
  "type": "basic",
  "title": "Installing the Syncano iOS Library"
}
[/block]
1. [Source](#section-source)
2. [CocoaPods](#section-cocoapods)
3. [XCode](#section-xcode)
4. [Create New XCode project](#section-create-new-xcode-project)
5. [Add the Syncano Library to the project](#section-add-the-syncano-library-to-the-project)

Source
======

We recommend installing the syncano-ios library through CocoaPods, but you can always grab the source from [our GitHub](https://github.com/syncano/syncano-ios).

CocoaPods
=========

To install our Syncano iOS Library, you need to use [CocoaPods](http://cocoapods.org/) . If you don’t already have it, open the Terminal app and paste the following line:
[block:code]
{
  "codes": [
    {
      "code": "sudo gem install cocoapods",
      "language": "shell"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Installing CocoaPods",
  "body": "If you didn't have CocoaPods installed before, please make sure to close and open a new Terminal window before proceeding to the next steps - otherwise `pod` command might not be recognized when you will try to use it."
}
[/block]
XCode
=====

We assume you already have an XCode IDE. If not, you can find it and install using the MacOS [AppStore](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)

Create New XCode project
====================

Launch XCode and do the following:

1. Using the XCode menu, choose **File -> New -> Project**
2. Choose the template that best suits your project. For now, to keep it simple, we’ll use **Single View Application**
3. Name the product as you want, or just type **SyncanoProject**. In the prefix field enter SYN (you can keep devices in **Universal** state, or change it to either **iPhone** or **iPad**)
4. Choose where you want to keep your project on the disk and hit the **Create** button
5. Close the XCode project for now

Add the Syncano Library to the project
==========================

Open the Terminal app and `cd` into the folder where you chose to save your project. If you don’t know how to do this, type
[block:code]
{
  "codes": [
    {
      "code": "cd",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]
and drag & drop the folder that contains your XCode project in Finder into the Terminal app. If you followed previous steps, you should have a file named **SyncanoProject.xcodeproj** inside a folder **SyncanoProject**. That’s the folder you need to drag from Finder into Terminal. When you do this, go to the Terminal app and press **ENTER** to confirm.

If you don’t have CocoaPods yet, check how to install it [here](#section-cocoapods). Once you have it, in Terminal (in folder where **.xcodeproj** file is) type:
[block:code]
{
  "codes": [
    {
      "code": "pod init",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]
Now, open the file named **Podfile** in your desired text editor and under app target, before the end, add this line:
[block:code]
{
  "codes": [
    {
      "code": "pod 'syncano-ios'",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]
If you followed the suggested naming, your **Podfile** should now look like below.
[block:callout]
{
  "type": "info",
  "title": "Using Syncano in a Swift project",
  "body": "If you use Syncano in Swift with CocoaPods - please make sure to uncomment in the `Podfile` below line saying `use_frameworks!`. \n\nIf for some reason you cannot use pods as frameworks - read more about [how to use a bridging header](#section-swift-bridging-header)."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# Uncomment this line if you're using Swift\n# use_frameworks!\n\ntarget \"SyncanoProject\" do\npod 'syncano-ios'\nend\n\ntarget \"SyncanoProjectTests\" do\n\nend",
      "language": "ruby",
      "name": "Obj-C project"
    },
    {
      "code": "# Uncomment this line if you're using Swift\nuse_frameworks!\n\ntarget \"SyncanoProject\" do\npod 'syncano-ios'\nend\n\ntarget \"SyncanoProjectTests\" do\n\nend",
      "language": "ruby",
      "name": "Swift project"
    }
  ]
}
[/block]
Now you can save the **Podfile**. Go back to Terminal and install the Syncano library by typing:
[block:code]
{
  "codes": [
    {
      "code": "pod install",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]
When the process is finished, you should see in the project’s folder a new **.xworkspace** (presumably **SyncanoProject.xcworkspace**) and from now on, you should work using this file, instead of the **.xcodeproj** one. You can open the workspace by double clicking it in Finder or just stay in Terminal and typing:
[block:code]
{
  "codes": [
    {
      "code": "open SyncanoProject.xcworkspace",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]
The library is already downloaded and added to your project. Every time you want to use it, just import Syncano:
[block:code]
{
  "codes": [
    {
      "code": "#import <Syncano.h>",
      "language": "objectivec"
    },
    {
      "code": "import syncano_ios",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using the Syncano Library"
}
[/block]
1. [Importing Syncano Headers](#section-importing-syncano-headers)
2. [Connecting to Syncano](#section-connecting-to-syncano)
3. [Adding your class](#section-adding-your-class)
   1. [Using references type field](#section-using-reference-type-field)
4. [Download the most recent Objects](#section-download-the-most-recent-objects)
5. [Create New Object](#section-create-new-object)
6. [Modify Object](#section-modify-object)
7. [Download objects matching specified criteria](#section-download-objects-matching-specified-criteria)
   1. [Using Predicates](#section-using-predicates)
   2. [Page Size and Order By](#section-page-size-and-order-by)
   3. [Using Pages](#section-using-pages)
   4. [Field filtering](#section-field-filtering) 
8. [Data Views](#section-data-views)
9. [Delete Objects](#section-delete-objects)
10. [Download files](#section-download-files)
11. [Running CodeBoxes and Webhooks](#section-running-codeboxes-and-webhooks)
   1. [CodeBoxes](#section-codeboxes)
   2. [Webhooks](#section-webhooks) 
11. [Synchronization and Channels](#section-synchronization-and-channels)
12. [Handling users](#section-handling-users)
   1. [Register new user](#section-register-new-user)
   2. [Log in user with password](#section-log-in-user-with-password)
   3. [Log in user with social backend](#section-log-in-user-with-social-backend)
   3. [Get user profile](#section-get-user-profile) 
   4. [Subclassing User and User Profile](#section-subclassing-user-and-user-profile)
13. [Support](#section-support)
14. [License](#section-license)

Importing Syncano Headers
=====================

+ Click a chosen file, we’ll use **SYNViewController.m** for now
+ Import Syncano libraries on top of your file, by adding following lines:
[block:code]
{
  "codes": [
    {
      "code": "#import <Syncano.h>",
      "language": "objectivec"
    },
    {
      "code": "import syncano_ios\n// or, if you used a bridging header in Swift, \n// there's no need to import anything\n// you already imported Syncano header in the bridging file",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Connecting to Syncano
=====================

+ Add a property, that will store the main Syncano object.
[block:code]
{
  "codes": [
    {
      "code": "@interface SYNViewController ()\n@property (strong) Syncano *syncano;\n@end",
      "language": "objectivec"
    },
    {
      "code": "//In Swift we can both declare and initialize our constant Syncano object, there's no need to do it in two places like in Obj-C\n\nlet syncano = Syncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"INSTANCE_NAME\")",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
+ In the **viewDidLoad** method, initialize a Syncano object:
[block:code]
{
  "codes": [
    {
      "code": "- (void)viewDidLoad {\n  [super viewDidLoad];\n  // Do any additional setup after loading the view, typically from a nib.\n  self.syncano = [Syncano sharedInstanceWithApiKey:@\"API_KEY\" instanceName:@\"INSTANCE_NAME\"];\n  \n}",
      "language": "objectivec"
    },
    {
      "code": "//No need to add anything in Swift, Syncano object is already initialized",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Adding your class
================
[block:callout]
{
  "type": "info",
  "title": "Using classes in Syncano",
  "body": "Before you will use the class in your app, remember that the class should already exist in Syncano. You can add your classes quickly using our [Dashboard](https://dashboard.syncano.io)!\n\nYou can read more about classes [here](doc:classes)."
}
[/block]
Now we will add the class you had implemented on Syncano. We will be using a simple Book class which only has two attributes: "title" and "subtitle", both being of type "string".
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
We will add this class in the app as well. Either in a separate file, or in the same view controller. For simplicity, we will assume you're adding this class to the `SYNViewController.m` file (or in `ViewController.swift`), in this case, add the following code right under `#import <Syncano.h>` (or for Swift, right above `class ViewController: UINavigationController` line)
[block:code]
{
  "codes": [
    {
      "code": "#import <Syncano.h>\n\n@interface Book : SCDataObject\n@property (nonatomic,copy) NSString *title;\n@property (nonatomic,copy) NSString *subtitle;\n@end",
      "language": "objectivec",
      "name": ""
    },
    {
      "code": "class Book : SCDataObject {\n  var title = \"\"\n  var subtitle = \"\"\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Add the class implementation below (or in the Book.m file if you are choosing to use separate files for this class). For Swift we don't need a separate implementation, as declaration and implementation happens in one place.
[block:code]
{
  "codes": [
    {
      "code": "@implementation Book\n@end",
      "language": "objectivec"
    },
    {
      "code": "//No need to add anything more, we already implemented this function.",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Registering a class",
  "body": "Before you continue, make sure to register all your classes. We recommend adding this code for all your classes in your app delegate."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n  // Override point for customization after application launch.\n  [Book registerClass];\n  [Author registerClass];\n  //register this way all classes, including subclasses of SCUser and SCUserProfile\n  [MyUserProfile registerClass];\n  [MyUser registerClassWithProfileClass:[MyUserProfile class]];\n  return YES;\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {\n\t// Override point for customization after application launch.\n\tBook.registerClass()\n\tAuthor.registerClass()\n  //register this way all classes, including subclasses of SCUser and SCUserProfile\n  MyUserProfile.registerClass()\n  MyUser.registerClassWithProfileClass(MyUserProfile.self)\n\treturn true\n}",
      "language": "swift"
    }
  ]
}
[/block]
### Using `Reference` type field

If you'd like to use `Reference` type field in your class -- it's possible to do that in iOS. 

For example, if you defined `Author` class in the same way as `Book` above, and now you wanted to have reference to author from book (and you defined its class schema e.g. through our [dashboard](doc:classes))  - you just need to add the field to your class.
[block:code]
{
  "codes": [
    {
      "code": "#import <Syncano.h>\n\n@interface Book : SCDataObject\n@property (nonatomic,copy) NSString *title;\n@property (nonatomic,copy) NSString *subtitle;\n@property (nonatoic,strong) Author *author;\n@end",
      "language": "objectivec"
    },
    {
      "code": "class Book : SCDataObject {\n  var title = \"\"\n  var subtitle = \"\"\n  var author : Author?\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
By defining class as above, whenever you save your `book` object - it will save reference to `author` automatically.

If `author` object was just created in your app, and doesn't exist on Syncano yet - library will save it on Syncano as well. If `author` object already exists on Syncano - library won't save it, but will update reference to it in `book` object. 
[block:code]
{
  "codes": [
    {
      "code": "// Case nr. 1\n// Book already exists, but we're adding to it a new Author\n// book will be saved, author will be saved on Syncano as well\n// and book will hold a reference to the new author object\nBook *book = [self getExistingBook];\nAuthor *author = [[Author alloc] init];\nauthor.first_name = @\"Tom\";\nauthor.last_name = @\"Tomson\";\nbook.author = author;\n[book saveWithCompletionBlock: ^(NSError *error){\n}];\n\n// Case nr. 2\n// Book already exists and Author already exists on Syncano\n// book will saved and reference to author wll be saved\n// author object will not even be sent to Syncano\nlet book : Book = [self getExistingBook];\nlet author = [self getExistingAuthor];\nbook.author = author;\n[book saveWithCompletionBlock: ^(NSError *error){\n}];",
      "language": "objectivec"
    },
    {
      "code": "// Case nr. 1\n// Book already exists, but we're adding to it a new Author\n// book will be saved, author will be saved on Syncano as well\n// and book will hold a reference to the new author object\nlet book : Book = getExistingBook()\nlet author = Author()\nauthor.first_name = \"Tom\"\nauthor.last_name = \"Tomson\"\nbook.author = author\nbook.saveWithCompletionBlock() { error in }\n\n// Case nr. 2\n// Book already exists and Author already exists on Syncano\n// book will saved and reference to author wll be saved\n// author object will not even be sent to Syncano\nlet book : Book = getExistingBook()\nlet author = getExistingAuthor()\nbook.author = author\nbook.saveWithCompletionBlock() { error in }",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
Download the most recent Objects
==============================

Add the following function to download the most recent Books, somewhere in the view controller class implementation
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadBooks {\n    [[Book please] giveMeDataObjectsWithCompletion:^(NSArray *books, NSError *error) {\n        //handle error\n        //do something with downloaded objects\n    }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadBooks() {\n  Book.please().giveMeDataObjectsWithCompletion { books, error in\n    //handle error\n    //do something with downloaded objects\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Make sure you call that function from inside of the `viewDidLoad` method, so the code runs when you launch your app.
[block:code]
{
  "codes": [
    {
      "code": "- (void)viewDidLoad {\n    [super viewDidLoad];\n  \n    self.syncano = [Syncano sharedInstanceWithApiKey:@\"YOUR_KEY\" instanceName:@\"YOUR_INSTANCE\"];\n    [self downloadBooks];\n}",
      "language": "objectivec"
    },
    {
      "code": "override func viewDidLoad() {\n  super.viewDidLoad()\n  self.downloadBooks()\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
That's it, you just download a list of your books stored on Syncano!

Create New Object
===============

Often, you will not be only downloading data created by others, but you will also want to create new objects yourself.
To do it, you need to create a new object of the class you'd like to work with, set chosen attributes, and save the object to Syncano.
[block:code]
{
  "codes": [
    {
      "code": "//define a class like shown earlier, in \"Adding your class\"\n\n- (void)addNewBook {\n  Book *book = [Book new];\n  book.title = @\"How to be a Pirate\";\n  book.subtitle = @\"10 tips that will change your life.\";\n  [book saveWithCompletionBlock:^(NSError *error) {\n    //process saved book\n    //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "//define a class like shown earlier, in \"Adding your class\"\n\nfunc addNewBook() {\n  let book = Book()\n  book.title = \"How to be a Pirate\"\n  book.subtitle = \"10 tips that will change your life.\"\n  book.saveWithCompletionBlock { error in\n    //handle error\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Modify Object
================

You can modify Syncano objects exactly in the same way as you modify every object in iOS. The only difference is that you have to save the changes at the end, otherwise your modifications will not be saved in Syncano.
[block:code]
{
  "codes": [
    {
      "code": "- (void)updateBook:(Book *)book {\n  book.title = @\"Pirate ships\";\n  book.subtitle = @\"Which was the biggest?\";\n  [book saveWithCompletionBlock:^(NSError *error) {\n    //error handling\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func updateBook(book: Book) {\n  book.title = \"Pirate ships\"\n  book.subtitle = \"Which was the biggest?\"\n  book.saveWithCompletionBlock { error in\n    //error handling\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
After adding this method, you will be able to pass it any book object, and it will be updated with a new `title` and `subtitle`.

Download objects matching specified criteria
==================================

There are couple of ways to filter results while getting a Data Object list.
More detailed information about filtering can be found [here](http://docs.syncano.com/v0.1.1/docs/filtering-data-objects).
[block:callout]
{
  "type": "info",
  "title": "Indexes",
  "body": "To be able to filter and sort based on chosen fields, please remember you have to first set an index on those fields. You can read more about it [here](doc:classes)."
}
[/block]
### Using Predicates

Usually we don't need to download all objects - we need only specific ones, matching our needs. E.g. books written only by one chosen author, or books with more than 300 pages - this can all be done by Syncano as well.

For the example below, we'll try to find a book titled `Pirate ship`. In the previous code examples above, that's how we titled one of our existing books.
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadPirateBooks {\n    SCPredicate *pirateBookPredicate = [SCPredicate whereKey:@\"title\" isEqualToString:@\"Pirate ships\"];\n    [[Book please] giveMeDataObjectsWithPredicate:pirateBookPredicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n    }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadPirateBooks() {\n  let pirateBookPredicate = SCPredicate.whereKey(\"title\", isEqualToString: \"Pirate ships\")\n  Book.please().giveMeDataObjectsWithPredicate(pirateBookPredicate, parameters: nil) { books, error in\n    //error handling\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
**Using multiple predicates**

If you want to filter by more than one attribute at the same time, or to apply multiple criteria to one attribute - you can do that using `SCCompoundPredicate`
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadPirateBooks {\n    SCPredicate *pirateBookPredicate = [SCPredicate whereKey:@\"title\" isEqualToString:@\"Pirate ships\"];\n  SCPredicate *numberOfPagesPredicate = [SCPredicate whereKey:@\"numberOfPages\" isGreaterThanOrEqualToNumber:@500];\n  SCCompoundPredicate *compoundPredicate = [SCCompoundPredicate compoundPredicateWithPredicates:@[pirateBookPredicate,numberOfPagesPredicate]];\n    [[Book please] giveMeDataObjectsWithPredicate:compoundPredicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n    }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadLongPirateBooks() {\n  let pirateBookPredicate = SCPredicate.whereKey(\"title\", isEqualToString: \"Pirate ships\")\n  let numberOfPagesPredicate = SCPredicate.whereKey(\"numberOfPages\", isGreaterThanOrEqualToNumber:500)\n  let compoundPredicate = SCCompoundPredicate(predicates: [pirateBookPredicate,numberOfPagesPredicate])\n  Book.please().giveMeDataObjectsWithPredicate(compoundPredicate, parameters: nil) { books, error in\n    //error handling\n  }\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Using Multiple Predicates",
  "body": "When joining predicates using `SCCompoundPredicate`, please remember they are joined using `AND` - returned objects will have to match all passed criteria. \nAt the moment there is no way to pass predicates joined by an `OR` statement."
}
[/block]
**Using referenced class queries**
If you want to find books written by authors from given country, you will find referenced class queries useful. Keep in mind you must have the filter checkbox marked in the class field you want to use or you will get failed requests:
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadBooksFromNorway {\n    SCPredicate *predicate = [SCPredicate whereKey:@\"author\" satisfiesPredicate:\n                     [SCPredicate whereKey:@\"country\" isEqualToString:@\"Norway\"]];\n    [[Book please] giveMeDataObjectsWithPredicate:predicate parameters:nil completion:^(NSArray *books, NSError *error) {\n        //error handling\n    }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadBooksFromNorway() {\n    let innerPredicate = SCPredicate.whereKey(\"country\", isEqualToString: \"Norway\")\n    let predicate = SCPredicate.whereKey(\"author\", satisfiesPredicate: innerPredicate)\n    Book.please().giveMeDataObjectsWithPredicate(predicate, parameters: nil) { books, error in\n        //error handling\n    }\n}",
      "language": "swift"
    }
  ]
}
[/block]
### Page Size and Order By

When you don't need to display a lot of data on the screen at once, you may want to limit the number of objects downloaded at once. You can do that by setting the `Page Size` parameter.

You can also modify the way the data returned from Syncano is sorted - if you want to sort on a different field from `id`, e.g. `title` of the book, you can also configure it this way.
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadBooksOrderedByTitle:(NSInteger)count {\n  [Book.please giveMeDataObjectsWithParameters:@{\n    SCPleaseParameterPageSize : @(count),\n    SCPleaseParameterOrderBy : @\"title\" \n  } completion:^(NSArray *books, NSError *error) {\n     //handle error\n   }];\n}\n\n//if you wanted to download only 5 books, you could do e.g.\n//[self downloadBooksOrderedByTitle:5];",
      "language": "objectivec",
      "name": ""
    },
    {
      "code": "func downloadBooksOrderedByTitle(count: Int) {\n  Book.please().giveMeDataObjectsWithParameters(\n    [SCPleaseParameterPageSize:count,\n     SCPleaseParameterOrderBy:\"title\"]) { books, error in\n    //handle error\n  }\n}\n\n//if you wanted to download only 5 books, you could do e.g.\n//self.downloadBooksOrderedByTitle(5)",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
### Using Pages

By default, when you use function  `giveMeDataObjectsWithCompletion`, or `giveMeDataObjectsWithParameters` etc - they return up to 100 objects in request (less if you have less than 100 objects in your class, or if you set page size to be less than 100).

To get more objects then only the ones from the first page, you need to other methods on our `SCPlease` object.

You can ask for next/previous page of results (e.g. in response to user clicking "load more" button).
[block:code]
{
  "codes": [
    {
      "code": "SCPlease *please = [Book please];\n// at some point in your coode you already downloaded some data, using predicate, parameters or not\n// e.g. by using 'giveMeDataObjectsWithCompletion' method\n// you should save SCPlease object as a variable and pass it around to get next or previous page\n[please giveMeNextPageOfDataObjectsWithCompletion:^(NSArray *objects, NSError *error) {        \n\t//work with your objects\n}];\n\n//or\n\n[please giveMePreviousPageOfDataObjectsWithCompletion:^(NSArray *objects, NSError *error) {        \n\t//work with your objects\n}];",
      "language": "objectivec"
    },
    {
      "code": "let please = Book.please()\n// at some point in your coode you already downloaded some data, using predicate, parameters or not\n// e.g. by using 'giveMeDataObjectsWithCompletion' method\n// you should save SCPlease object as a variable and pass it around to get next or previous page\nplease.giveMeNextPageOfDataObjectsWithCompletion { books, error in\n  //work with your objects\n}\n\n//or\n\nplease.giveMePreviousPageOfDataObjectsWithCompletion { books, error in\n  //work with your objects\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Get next or previous page",
  "body": "Remember, that when using `[Book please]` in Obj-C or `Book.please()` in Swift - it always returns a new SCPlease object.\nUsing `giveMeNextPageOfDataObjectsWithCompletion` or `giveMePreviousPageOfDataObjectsWithCompletion` on a new SCPlease object will always return 0 objects, as `next` or `previous` must be used in context of a request done earlier.\n\nTo use these methods properly:\n1) Make sure to store SCPlease \n2) Make a normal request, using e.g. `giveMeDataObjectsWithCompletion` method on your stored SCPlease object\n3) only after 2) was done, ask for next/previous pages.\n\nIf you ever need to start from page 0 - just get the new SCPlease object using `please` method on your class."
}
[/block]
If you know you will need all the objects from your class, or more than one page, but you don't know how many pages - you can use convenient method `enumaratePagesWithPredicate: parameters:`.

It takes a predicate and/or parameters (both can be nil) and a block (closure). 

Block gives you an error (if one was set), array with objects from Syncano and a pointer to BOOL value - you should set it to true if you'd like to stop enumerating.
[block:code]
{
  "codes": [
    {
      "code": "[[Book please] enumaratePagesWithPredicate:nil parameters:@{SCPleaseParameterPageSize : @(5)} withBlock:^(BOOL *stop, NSArray *objects, NSError *error) {\n  if (error) {\n    //handle error\n  }\n  for (Book *book in objects) {\n    //do something with my books\n  }\n  // if you got all the results you wanted and don't need to get more pages - set *stop to true\n  if (/*you have enough results == */ YES) {\n    *stop = YES;\n  } else {\n    //it will keep enumarating and block will be called for next page of results\n  }\n}];",
      "language": "objectivec"
    },
    {
      "code": "Book.please().enumaratePagesWithPredicate(nil, parameters: [SCPleaseParameterPageSize : 5]) { stop, books, error in\n  guard error == nil, let books = books as? [Book] else {\n    //handle error\n    return\n  }\n  for book in books {\n  \t//do something with my books\n  \tprint(\"Title: \\(book.title)\")\n  }\n  // if you got all the results you wanted and don't need to get more pages - set Bool 'stop' points to, to true\n  if (/*you have enough results == */ true) {\n  \tstop.memory = true;\n  } else {\n  \t//it will keep enumarating and block will be called for next page of results\n  }\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If you need to enumerate all pages, starting from page 0, it's safe to use `[Book please]` or `Book.please()` directly (as in the example above).",
  "title": "Enumerate pages"
}
[/block]
### Field filtering

Sometimes you may not need to display all the data inside your objects - e.g. your Book class may contain info about author, number of pages, title, subtitle, publisher, year it was published, category, summary and much more. But your view shows only book title and you'd like to download only this one field - title - allowing for smaller data transfer thus faster download time.

You can do that as well, using the following code:
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadBookTitles {\n  [Book.please giveMeDataObjectsWithParameters:@{ \n    SCPleaseParameterFields : @[@\"title\"] \n  } completion:^(NSArray *books, NSError *error) {\n        //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadBookTitles() {\n  Book.please().giveMeDataObjectsWithParameters([SCPleaseParameterFields:[\"title\"]]) \n  { books, error in\n    //handle error\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
You can find list of all possible parameters in `SCPlease.h` file.

Data Views
=====================
Views are very handy and iOS library support all their features - you can filter on views and have the relations between objects expanded. 

Let's start with modifying our base classes:
[block:code]
{
  "codes": [
    {
      "code": "@interface Author : SCDataObject\n@property (nonatomic,copy) NSString *name;\n@property (nonatomic,copy) NSString *country;\n@end\n\n@interface Book : SCDataObject\n@property (nonatomic,copy) NSString *title;\n@property (nonatomic,copy) NSString *subtitle;\n@property (nonatomic,copy) SCFile *cover;\n@property (nonatomic,strong) Author *author;\n@end",
      "language": "objectivec"
    },
    {
      "code": "class Author : SCDataObject {\n    var name = \"\"\n    var country = \"\"\n}\n\nclass Book : SCDataObject {\n    var title = \"\"\n    var subtitle = \"\"\n    var cover: SCFile?\n    var author: Author?\n}",
      "language": "swift"
    }
  ]
}
[/block]
Our view name is **"books_with_authors"**. The following code fetches data from the view:
[block:code]
{
  "codes": [
    {
      "code": "[Book registerClass];\n[Author registerClass];\n\n- (void)downloadBooksAndAuthors {\n  [[Book pleaseForView:@\"books_with_authors\"] giveMeDataObjectsWithCompletion:^(NSArray *objects, NSError *error) {\n    //error handling\n    Book *book = objects.firstObject;\n    //If your view is properly configured you can now access book.author.country\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "Book.registerClass()\nAuthor.registerClass()\n\nfunc downloadBooksAndAuthors() {\n\tBook.pleaseForView(\"books_with_authors\").giveMeDataObjectsWithCompletion { books, error in\n    //handle error\n    let book = books.first\n    //If your view is properly configured you can now access book.author.country\n  }\n}",
      "language": "swift"
    }
  ]
}
[/block]
You can easily filter on views:
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadBooksWithNbOfPages:(NSNumber*)nbOfPages {\n  [[Book pleaseForView:@\"books_with_authors\"] giveMeDataObjectsWithPredicate:[SCPredicate whereKey:@\"numofpages\" isEqualToNumber:nbOfPages] parameters:nil completion:^(NSArray *objects, NSError *error) {\n    //error handling\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadBooksWithNbOfPages(nbOfPages: NSNumber!) {\n\tBook.pleaseForView(\"books_with_authors\").giveMeDataObjectsWithPredicate(SCPredicate.whereKey(\"numofpages\", isEqualToNumber: nbOfPages), parameters: nil) { books, error in\n  //handle error\n  }\n}",
      "language": "swift"
    }
  ]
}
[/block]
If you have a class which represents Data View you can specify view name for this class and type less:
[block:code]
{
  "codes": [
    {
      "code": "@interface BooksView : Book\n@end\n\n@implementation BooksView\n+ (NSString *)viewNameForAPI {\n    return @\"books_with_authors\";\n}\n@end\n  \n- (void)downloadBooksAndAuthors {\n  [[BooksView please] giveMeDataObjectsWithCompletion:^(NSArray *objects, NSError *error) {\n    //error handling\n  }];\n}  ",
      "language": "objectivec"
    },
    {
      "code": "class BooksView : Book {\n    override class func viewNameForAPI() -> String! {\n        return \"books_with_authors\"\n    }\n}\n\nfunc downloadBooksAndAuthors() {\n\tBooksView.please().giveMeDataObjectsWithCompletion { books, error in\n  //handle error\n  //do something with downloaded objects\n  }\n} ",
      "language": "swift"
    }
  ]
}
[/block]
Delete Objects
=====================
Add this following function to delete a passed Book object.
[block:code]
{
  "codes": [
    {
      "code": "- (void)deleteBook:(Book *)book {\n  [book deleteWithCompletion:^(NSError *error) {\n    //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func deleteBook(book: Book) {\n  book.deleteWithCompletion { error in\n    //handle error\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Now you can call this method whenever you want to delete a chosen objects.

Download files
=====================
File can be saved to memory, use this for smaller files:
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadCoverForBook:(Book *)book {\n  book.cover.storeDataAfterFetch = YES;//set to YES when you want to access data via: book.cover.data property\n  [book.cover fetchInBackgroundWithCompletion:^(NSData *data, NSError *error) {\n    //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadCoverForBook(book: Book) {\n    book.cover?.storeDataAfterFetch = true//set to true when you want to access data via: book.cover.data\n    book.cover?.fetchInBackgroundWithCompletion { data, error in\n        //handle error\n    }\n}",
      "language": "swift"
    }
  ]
}
[/block]
For bigger files or when it's useful, a file can be saved to disk (temporary location):
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadCoverToDiskForBook:(Book *)book {\n  [book.cover fetchToFileInBackgroundWithProgress:^(NSURLSessionDownloadTask *downloadTask, int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {\n    //display progress NSLog(@\"%lld %lld %.0f\",totalBytesWritten,totalBytesExpectedToWrite,((float)(totalBytesWritten * 100) / totalBytesExpectedToWrite));\n  } completion:^(NSURLResponse *response, NSURL *filePath, NSError *error) {\n    //handle error\n    NSData *data = [NSData dataWithContentsOfURL:filePath];\n    //use data\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadCoverToDiskForBook(book: Book) {\n    book.cover?.fetchToFileInBackgroundWithProgress({ downloadTask, bytesWritten, totalBytesWritten, totalBytesExpectedToWrite in\n            //display progress\n        }, completion: { response, filePath, error in\n            //handle error\n            let data = NSData(contentsOfURL: filePath)\n            //use data\n    })\n}",
      "language": "swift"
    }
  ]
}
[/block]
A file can be also saved to documents:
[block:code]
{
  "codes": [
    {
      "code": "- (void)downloadCoverToDocumentsForBook:(Book *)book {\n  NSURL *storePathDocuments = [[NSFileManager defaultManager] URLForDirectory:NSDocumentDirectory inDomain:NSUserDomainMask appropriateForURL:nil create:NO error:nil];\n  NSURL *storePath = [storePathDocuments URLByAppendingPathComponent:@\"bookCover.png\"];\n  \n  [book.cover fetchToFileInBackground:storePath withProgress:^(NSURLSessionDownloadTask *downloadTask, int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {\n            //display progress\n        } completion:^(NSURLResponse *response, NSURL *filePath, NSError *error) {\n    //handle error\n    NSData *data = [NSData dataWithContentsOfURL:storePath];\n    //use data\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func downloadCoverToDocumentsForBook(book: Book) throws {\n    let storePathDocuments = try NSFileManager.defaultManager().URLForDirectory(.DocumentDirectory, inDomain: .UserDomainMask, appropriateForURL: nil, create: false)\n    let storePath = storePathDocuments.URLByAppendingPathComponent(\"bookCover.png\")\n    \n    book.cover?.fetchToFileInBackground(storePath, withProgress: { downloadTask, bytesWritten, totalBytesWritten, totalBytesExpectedToWrite in\n            //display progress\n        }, completion: { response, filePath, error in\n            //handle error\n            let data = NSData(contentsOfURL: filePath)\n            //use data\n    })\n}",
      "language": "swift"
    }
  ]
}
[/block]
Running CodeBoxes and Webhooks
============================
Learn more about [CodeBox](doc:snippets-scripts) and [Webhooks](doc:endpoints-scripts) in our Developer Manual. Here, you can see how to quickly run a CodeBox or a Webhook.

###CodeBoxes

Run a codebox with a specified ID and get back its Trace. You can find out what is a Trace in the [Traces](doc:traces) chapter.
[block:code]
{
  "codes": [
    {
      "code": "- (void)runCodeBox:(NSInteger)codeboxID {\n  [SCCodeBox runCodeBoxWithId:@(codeboxID) params:nil completion:^(SCTrace *trace, NSError *error) {\n    //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func runCodeBox(codeboxID: Int) {\n  SCCodeBox.runCodeBoxWithId(codeboxID, params: nil) { trace, error in\n    //handle error\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
###Webhooks

You can run a webhook by passing its name. You will receive the details of the execution in a response (`SCWebhookResponseObject` class).
[block:code]
{
  "codes": [
    {
      "code": "- (void)runWebHook:(NSString *)webhookName {\n  [SCWebhook runWebhookWithName:webhookName completion:^(SCWebhookResponseObject *response, NSError *error) {\n    //handle error\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func runWebHook(name: String) {\n  SCWebhook.runWebhookWithName(name) { response, error in\n    //handle error\n   }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
In case your webhook uses your own (custom) response format, you can use the following code snippet:
[block:code]
{
  "codes": [
    {
      "code": "- (void)runWebHook:(NSString *)webhookName {\n  [SCWebhook runCustomWebhookWithName:webhookName completion:^(id responseObject, NSError *error) {\n    //handle error\n    //responseObject is most likely an instance of NSData*\n    NSArray *jsonArray = [NSJSONSerialization JSONObjectWithData:responseObject options:kNilOptions error:NULL];\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func runWebHook(name: String) {\n    SCWebhook.runCustomWebhookWithName(name) { response, error in\n        //handle error\n        do {\n            let array = try NSJSONSerialization.JSONObjectWithData(response as! NSData!, options: []);\n        } catch let jsonError {\n            //handle parse error\n        }\n    }\n}",
      "language": "swift"
    }
  ]
}
[/block]
Synchronization and Channels
========================
You can read more about Real-Time Syncing and channels in the [Realtime communication](doc:realtime-communication) chapter, but here you can quickly learn how to listen for notifications sent to a given channel.

First, add the channel object to your view controller and add information. For this you will implement SCChannelDelegate protocol.
[block:code]
{
  "codes": [
    {
      "code": "@interface ViewController ()<SCChannelDelegate>\n@property (strong) Syncano *syncano;\n@property (strong) SCChannel *channel;\n@end",
      "language": "objectivec"
    },
    {
      "code": "class SwiftViewController: SCChannelDelegate {\n  let syncano = Syncano.sharedInstanceWithApiKey(\"API_KEY\", instanceName: \"INSTANCE_NAME\")\n  var channel: SCChannel? = nil\n  //rest of the class\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Initialize the channel object and start listening to incoming messages. In our example we will do it in the viewDidLoad method by adding the following lines there.
[block:code]
{
  "codes": [
    {
      "code": "self.channel = [[SCChannel alloc] initWithName:@\"channel\" andDelegate:self];\n[self.channel subscribeToChannel];",
      "language": "objectivec"
    },
    {
      "code": "self.channel = SCChannel(name: \"channel\", andDelegate: self)\nself.channel?.subscribeToChannel()",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
Implement the SCChannelDelegate protocol by pasting this function into your class implementation (e.g. right above `viewDidLoad`).
[block:code]
{
  "codes": [
    {
      "code": "- (void)chanellDidReceivedNotificationMessage:(SCChannelNotificationMessage *)notificationMessage {\n    //handle notification\n}",
      "language": "objectivec"
    },
    {
      "code": "func chanellDidReceivedNotificationMessage(notificationMessage: SCChannelNotificationMessage) {\n  //handle notification\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
That's it! From now on, whenever there's an incoming message, such as an object added/changed/deleted, the `chanellDidReceivedNotificationMessage` method will be called.

Make sure to read [Realtime communication](doc:realtime-communication) chapter to learn more about how it works!

Handling users
============

To find out more about the Users module on Syncano - read [User management](doc:user-management) chapter. Here, we'll quickly teach you how to create a new user, log the user in and get their profile data.

### Register new user
[block:code]
{
  "codes": [
    {
      "code": "- (void)registerUser:(NSString *)username password:(NSString *)password {\n    [SCUser registerWithUsername:username password:password completion:^(NSError *error) {\n      //handle user registration\n    }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func registerUser(username: String, password: String) {\n  SCUser.registerWithUsername(username, password: password) { error in\n    //handle user registration\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
### Log in user with password
[block:code]
{
  "codes": [
    {
      "code": "- (void)loginUser:(NSString *)username password:(NSString *)password {\n  [SCUser loginWithUsername:username password:password completion:^(NSError *error) {\n    //handle logged in user\n  }];\n}",
      "language": "objectivec"
    },
    {
      "code": "func loginUser(username: String, password: String) {\n  SCUser.loginWithUsername(username, password: password) { error in\n    //handle logged in user\n  }\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
### Log in user with social backend
[block:code]
{
  "codes": [
    {
      "code": "// Available backends:\n// SCSocialAuthenticationBackendFacebook\n// SCSocialAuthenticationBackendGoogle\n// SCSocialAuthenticationBackendLinkedIn\n// SCSocialAuthenticationBackendTwitter\n[SCUser loginWithSocialBackend:SCSocialAuthenticationBackendFacebook authToken:@\"FACEBOOK_AUTH_TOKEN\" completion:^(NSError *error) {\n\t//handle logged in user\n}];",
      "language": "objectivec"
    },
    {
      "code": "// Available backends:\n// .Facebook\n// .Google\n// .LinkedIn\n// .Twitter\nSCUser.loginWithSocialBackend(.Facebook, authToken: \"FACEBOOK_AUTH_TOKEN\") { error in\n\t//handle logged in user\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
### Get user profile

First we need to grab a currently logged in user, next we can easily get their profile.
[block:code]
{
  "codes": [
    {
      "code": "- (void)getCurrentUserProfile {\n  SCUser *currentUser = [SCUser currentUser];\n  SCUserProfile *profile = currentUser.profile;\n  //display profile in your app\n}",
      "language": "objectivec"
    },
    {
      "code": "func getCurrentUserProfile() {\n  let currentUser = SCUser.currentUser()\n  let profile = currentUser.profile\n  //display profile in your app\n}",
      "language": "javascript",
      "name": "Swift"
    }
  ]
}
[/block]
### Subclassing user and user profile

You can also subclass SCUser and SCUserProfile class - really handy when you want to expand user's profile and some extra properties. For more details on how that works, go to [User management](doc:user-management#user-profiles) and see below how to prepare your classes on iOS.
[block:code]
{
  "codes": [
    {
      "code": "//declare new User Profile class, by subclassing SCUserProfile\n@interface MyUserProfile : SCUserProfile\n@property (strong) SCFile *avatar;\n@property (strong) NSString *first_name;\n@property (strong) NSString *last_name;\n@property (strong) NSString *email;\n@end\n  \n@implementation MyUserProfile\n//implementation can be empty - no need to add anything\n@end\n\n//subclass SCUser to type of 'profile' property to MyUserProfile\n@interface MyUser : SCUser\n@property (strong, nonatomic) MyUserProfile *profile;\n@end\n  \n@implementation MyUser\n//we won't be implementing this property here - it's already implemented in superclass, so we will tell that to compiler by using '@dynamic' \n@dynamic profile;\n@end\n  \n//now, in your code, before you use for the first time SCUser class (or your subclass):\n[MyUser registerClassWithProfileClass:[MyUserProfile class]];\n\n//from now on you can keep using your own classes\n[MyUser loginWithUsername:@\"a\" password:@\"a\" completion:^(NSError *error) {\n  MyUser *user = [MyUser currentUser];\n  MyUserProfile *profile = user.profile;\n  //work with user profile and user objects\n}];",
      "language": "objectivec"
    },
    {
      "code": "//declare new User Profile class, by subclassing SCUserProfile\nclass MyUserProfile : SCUserProfile {\n    var avatar : SCFile? = nil;\n    var name = \"\"\n    var first_name = \"\"\n    var email = \"\"\n}\n\n//subclass SCUser to add new profile variable for the profile\n//Swift doesn't allow overring superclass' property type\n//(so we cannot 'profile' to be of MyUserProfile type)\n//but we can add a convenient setter and getter\nclass MyUser : SCUser {\n    var myProfile : MyUserProfile? {\n        get {\n            return self.profile as? MyUserProfile\n        }\n        set {\n            self.profile = newValue\n        }\n    }\n}\n\n//now, in your code, before you use for the first time SCUser class (or your subclass):\nMyUser.registerClassWithProfileClass(MyUserProfile.self)\n  \n//from now on you can keep using your own classes\nMyUser.loginWithUsername(\"login\", password: \"password\") { error in\n  let user = MyUser.currentUser()\n  let profile = user.myProfile\n  //work with user profile and user objects\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
Support
======

Now you’re ready to use Syncano in your iOS project. We really hope you will enjoy working with our platform. If you have any issues or suggestions, just let us know at support@syncano.com.

License
======

Syncano’s iOS Library (syncano-ios) is available under the MIT license. See the LICENSE file inside library sources for more info.

------------------------------------------------------------------------------------------------------------------

Swift Bridging Header
========================

If for any reason, you need to use syncano-ios in Swift, using with a bridging header, follow the steps below.

**If you don't have the bridging header yet:**

You can add a bridging header manually, but the easiest way is to use XCode:
1. Add a new **.m** file to your project, by choosing **File -> New -> File -> “Objective-C File”** (the one with **.m** icon)
2. Give it some random name e.g. “SyncanoSwift” (it’s not important, as we will not be using this file for anything later)
3. As for file type, you can leave the selection on **Empty File** and click **Next**
4. Save the file anywhere in the project, by choosing a directory and clicking **Create**
5. When XCode asks you “Would you like to configure an Objective-C bridging header?”, choose **Yes**
6. Now XCode should add a bridging file to your project named YourProjectName-Bridging-Header.h, so in our case it would be **SyncanoProject-Bridging-Header.h**
7. You can remove the previously created **.m** file. Choose it in your project, right click on it, choose **Delete** and **Move to Trash**
8. Add syncano-ios header to your bridging header as in `When you have the bridging header` section below

**When you have the bridging header:**

Choose the bridging header in XCode and paste this **#import** directive:
[block:code]
{
  "codes": [
    {
      "code": "#import <Syncano.h>",
      "language": "objectivec"
    }
  ]
}
[/block]
Because you already have library files included in the bridging header, there’s no need to import anything else in your `.swift` files.