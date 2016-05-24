---
title: "Ember.js"
excerpt: "Ember add-on Quick Start"
---
1. [Overview](#overview)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Data Management with Ember Data](#data-management-with-ember-data)
5. [Injecting the Service](#injecting-the-service)
6. [Examples](#examples)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
The Syncano Ember add-on is designed to integrate Syncano directly into your Ember-CLI application seamlessly. It works with Ember Data to persist data changes from your Ember application to your Syncano Instance.

View the [source code](https://github.com/Syncano/ember-syncano).
[block:api-header]
{
  "type": "basic",
  "title": "Installation"
}
[/block]
To install the Syncano Ember add-on, you will need to run the following command from your Ember-CLI application.

`ember install ember-syncano`
[block:api-header]
{
  "type": "basic",
  "title": "Configuration"
}
[/block]
Once installed, you will need to add the code block below to your app's config file at `app/config/environment.js` and insert your credentials.

```javascript
syncano: {
  accountKey: 'your-account-key', // optional unless calling methods from the account scope
  apiKey: 'your-api-key',
  instance: 'your-instance-name'
}
```


[block:api-header]
{
  "type": "basic",
  "title": "Data Management with Ember Data"
}
[/block]
The `ember-syncano` add-on works seamlessly with Ember Data. All you need to do is set up your Syncano Class and Ember Model with the same name, properties, and attribute types. For example, if you wanted to create a Todo model, you could set it up in Ember like this under `app/models/todo.js`.

```javascript
import DS from 'ember-data';

export default DS.Model.extend({
  title: DS.attr('string'),
  is_completed: DS.attr('boolean')
});
```

Then, you would create a Syncano Class named `todo` with `title` as a `string` and `is_completed` as a `boolean`.


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/7SelHdAqTqan53PiurfH",
        "Screen Shot 2016-03-30 at 2.08.58 PM.png",
        "1085",
        "700",
        "#275b94",
        ""
      ]
    }
  ]
}
[/block]
With that set up correctly, you can manage your data like your normally would with Ember Data's store. For example, you could pull in a list of all of your todo items like this.
[block:code]
{
  "codes": [
    {
      "code": "this.store.findAll('todo');",
      "language": "javascript"
    }
  ]
}
[/block]
Add a todo item
[block:code]
{
  "codes": [
    {
      "code": "var item = this.store.createRecord('todo', {'title': 'First Item', 'is_completed': false});\nitem.save();",
      "language": "javascript"
    }
  ]
}
[/block]
Update a todo item
[block:code]
{
  "codes": [
    {
      "code": "item.set('is_completed', true);\nitem.save();",
      "language": "javascript"
    }
  ]
}
[/block]
Delete a todo item
[block:code]
{
  "codes": [
    {
      "code": "item.destroyRecord();",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Injecting the Service"
}
[/block]
In addition to data management, you can also inject the Syncano library directly into a route, controller, or component in Ember. This will allow you to access other Syncano functions such as running Scripts and Script Endpoints, User Authentication, and others. To do this, you need to add the code below to the route, controller, or component you want to be able to use Syncano.
[block:code]
{
  "codes": [
    {
      "code": "syncano: Ember.inject.service('syncano')",
      "language": "javascript"
    }
  ]
}
[/block]
Once Syncano is injected correctly, you will be able to call its functions by accessing the object.
[block:code]
{
  "codes": [
    {
      "code": "this.get('syncano')",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Examples"
}
[/block]
For more information about using `ember-syncano` check out the following examples:
* [Build a 5 Minute Todo App with Syncano's New Ember Add-on](https://www.syncano.io/blog/ember-syncano-addon/)
* [Add Filtering and Sorting to an Ember-Syncano Todo App](https://www.syncano.io/blog/queries-with-ember-syncano/)