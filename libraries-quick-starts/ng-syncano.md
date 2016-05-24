---
title: "AngularJS"
excerpt: "ng-Syncano AngularJS Library Quick Start"
---
1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Injecting ngSyncano](#injecting-ngsyncano)
4. [Setting Up the Config](#setting-up-the-config)
5. [Config with a User Key](#config-with-a-user-key)
6. [Using the syncanoService In Your Controller](#using-the-syncanoservice-in-your-controller)
7. [Example View](#example-view)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
`ng-syncano` is set up to allow you to use Syncano API calls from within your AngularJS application. This guide will walk you through the installation steps of the `ng-syncano` library.

If you don't have a Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano).
[block:api-header]
{
  "type": "basic",
  "title": "Getting Started"
}
[/block]
Using our Angular Service is simple! After you set it up, you'll be able to use Syncano API calls within Angular without having to import `syncano.js` anywhere else in your code.

If this is your first time using Angular, please take a look at our blog post <a href="https://www.syncano.io/blog/intro-angular-js/" target="_blank">Intro to Angular.js</a> or the <a href="https://angularjs.org/#the-basics">AngularJS</a> homepage.

This library is intended to be used with a [Syncano](http://www.syncano.io/) account. If you don't already have one - you can sign up [here](https://dashboard.syncano.io/).

**Install from Bower**
[block:code]
{
  "codes": [
    {
      "code": "bower install ng-syncano --save",
      "language": "shell"
    }
  ]
}
[/block]
**Install from NPM**
[block:code]
{
  "codes": [
    {
      "code": "npm install ng-syncano --save",
      "language": "shell"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Injecting ngSyncano"
}
[/block]
Once you have put the `ng-syncano.js` or `ng-syncano.min.js` file into your AngularJS app directory, you'll need to import ngSyncano so you can use it's API calls and services.

In your `app.js` file or the file that contains code that looks something like this:
[block:code]
{
  "codes": [
    {
      "code": "var myApp = angular.module('myApp', []);",
      "language": "javascript"
    }
  ]
}
[/block]
You'll need to insert the `ngSyncano` keyword in between the brackets like this:
[block:code]
{
  "codes": [
    {
      "code": "var myApp = angular.module('myApp', ['ngSyncano']);",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Setting Up the Config"
}
[/block]
Next you'll need to set up the config part of your app, so that Syncano knows what your API details are.

In that same `app.js` file, put the following code, but be sure to replace the `apiKey` and `instance` with your Syncano **public api key** and instance:

_If you need help finding your API key, read our docs <a href="http://docs.syncano.io/#getting-an-api-key">here</a>._
[block:code]
{
  "codes": [
    {
      "code": "myApp.config(function (syncanoConfigProvider) {\n  \tsyncanoConfigProvider.configure({\n    \t\tapiKey: 'APIKEY/ACCOUNTKEY',\n    \t\tinstance: 'INSTANCE'\n  \t});\n});",
      "language": "javascript"
    }
  ]
}
[/block]
If you already have a `.config` section in your app, make sure you add the `ngSyncano` service to that configuration instead of creating a new configuration.

*Be aware that if you use more than one config for ngSyncano, only the first one will be used!*

[block:api-header]
{
  "type": "basic",
  "title": "Config with a User Key"
}
[/block]
Most API calls will require more authentication or a higher level API key. The one we suggest using for your public app is a public API key. This key will need a user key to provide you with access to more permissions and API calls.

We have set up the ngSyncano library so you can start your app with a `username` and `password`. The code below shows you how!
[block:code]
{
  "codes": [
    {
      "code": "myApp.config(function (syncanoConfigProvider) {\n\t  syncanoConfigProvider.configure({ // enter Syncano details\n\t\t    apiKey: 'MY_PUBLIC_API_KEY',\n\t\t    instance: 'MY_INSTANCE'\n\t\t    username: 'USERNAME',\n\t\t    password: 'PASSWORD'\n\t  });\n});",
      "language": "javascript"
    }
  ]
}
[/block]
**This would replace your original config function!**
[block:api-header]
{
  "type": "basic",
  "title": "Using the `syncanoService` In Your Controller"
}
[/block]
After you have completely set up the config for ngSyncano in your app, you will need to inject the Syncano Service into your controller.

To use the Syncano API calls in your Angular controller, just include `syncanoService` in the `function` of your controller. The `syncanoService` will allow you to do a few things:

1. Get the current Syncano object (global)
2. Add a User Key
3. Remove a User Key

Then you just use the regular JS Library API calls to perform the rest of your Syncano API calls.
[block:code]
{
  "codes": [
    {
      "code": "myApp.controller('SyncanoController', function ($scope, syncanoService) {\n\tvar syncano = null; // will be used for API calls\n\t$scope.dataRetrievedFromSyncano = null;\n\t$scope.error = null;\n\n\tsyncanoService.getSyncano() // gets the current Syncano object\n\t\t.then(function(res){ // uses promises\n\t\t\tsyncano = res; // set to current Syncano Object\n\t\t\t\n\t\t\t/* TO REMOVE A USER */\n\t\t\t//syncanoService.removeSyncanoUser(); // returns string\n\n\t\t\t/* TO LOG IN */\n\t\t\t//var user = {\n\t\t\t\t//\"username\": \"USERNAME\",\n\t\t\t\t//\"password\": \"PASSWORD\"\n\t\t\t//};\n\t\t\t//syncanoService.setSyncanoUser(user)\n\t\t\t\t//.then(function(res){\n\t\t\t\t\t//syncano = res;\n\t\t\t\t//})\n\t\t\t\t//.catch(function(err){\n\t\t\t\t\t//console.log(err);\n\t\t\t\t//});\n\t\t})\n    .catch(function(err){\n\t\t\tconsole.log(err);\n\t\t});\n});",
      "language": "javascript"
    }
  ]
}
[/block]
The `syncano` object will contain all of your API details, so you won't need to type them in again. You'll notice that the `getSyncano()` call expects a promise. This is done so that you can choose to either include user info at the beginning, or leave them out, but the library will always send a promise back.
[block:callout]
{
  "type": "warning",
  "title": "Note",
  "body": "All functions needing Syncano data will need to be called in the `.then()` of the `getSyncano()` call! This is because that call is asynchronous."
}
[/block]

### **For more details see `example.html` in this repo.**

Once you have imported `syncanoService` and have used it for an API call, the last step is to display it in the DOM or `html` page! That's where you get creative ;)

Look <a href="http://docs.syncano.io/" target="_blank">here</a> for more examples on our JS Library API calls.
[block:api-header]
{
  "type": "basic",
  "title": "Example View"
}
[/block]
Here is a sample controller:
[block:code]
{
  "codes": [
    {
      "code": "var myApp = angular.module('myApp', ['ngSyncano']);\n\nmyApp.config(function (syncanoConfigProvider) { // NOTE: please be aware that only first configuration would be applied\n\tsyncanoConfigProvider.configure({ // enter Syncano details\n\t\tapiKey: 'MY_PUBLIC_API_KEY',\n\t\tinstance: 'MY_INSTANCE'\n\t\t// username: 'USERNAME',\n\t\t// password: 'PASSWORD'\n\t});\n  \n\t// You can also use your account key, BUT then your API calls in the controllers\n\t// will need to include the .instance('MY_INSTANCE') call AFTER syncano\n\t// syncanoConfigProvider.configure({\n\t//\taccountKey: 'MY_ACCOUNT_KEY'\n\t// });\n});\n\nmyApp.controller('SyncanoController', function ($scope, syncanoService) {\n\tvar syncano = null; // will be used for API calls\n\t$scope.dataRetrievedFromSyncano = null;\n\t$scope.error = null;\n\tsyncanoService.getSyncano() // gets the current Syncano object\n\t\t.then(function(res){ // uses promises in case a userKey is needed\n\t\t\tsyncano = res; // set to current Syncano Object\n    \tgetList(); // ONLY WORKS if you have permissions set correctly\n  \t})\n    .catch(function(err){\n    \tconsole.log(err);\n  \t});\n\tfunction getList(){ // can only be used with apiKey/userKey or accountKey\n\t\tsyncano.class('CLASS').dataobject().list() // Change CLASS to your class\n\t\t\t.then(function(res){\n\t\t\t\t$scope.dataRetrievedFromSyncano = res.objects;\n\t\t\t})\n\t\t\t.catch(function(err){\n\t\t\t\t$scope.error = err;\n\t\t\t});\n\t}\n});",
      "language": "javascript"
    }
  ]
}
[/block]
And here is the example `view.html`
[block:code]
{
  "codes": [
    {
      "code": "<body ng-app=\"myApp\">\n\t<div ng-controller=\"SyncanoController\">\n\t\t<div ng-if=\"error !== null\" class=\"error\">{{error.message}}</div>\n\t\t<div ng-if=\"dataRetrievedFromSyncano !== null\" class=\"data\">\n      {{dataRetrievedFromSyncano | json}}\n    </div>\n\t</div>\n</body>",
      "language": "html"
    }
  ]
}
[/block]
_This will simply print the ID numbers of each object in your class in a list._

You would include this code in either your `view` or inbetween your `index.html` `<body>` tags, depending on how your Angular app is set up!

The github repo for `ng-syncano` can be found <a href="https://github.com/Syncano/ng-syncano">here</a>.