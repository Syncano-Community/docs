---
title: "JavaScript"
excerpt: ""
---
1. [Overview](#overview)
2. [Installing the Library](#installing-the-library)
3. [Using Syncano library](#using-syncano-library)
4. [Promises](#promises)
5. [Model instances](#model-instances)
5. [QuerySet](#queryset)
6. [Nested models](#nested-models)
7. [Contributing](#contributing)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This guide will walk you through the installation steps of the [Syncano Library](https://github.com/Syncano/syncano-js) for JavaScript as well as give you a couple of usage examples.

If you don't have a Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano).

For the full JavaScript documentation, view the [syncano-js reference](http://syncano.github.io/syncano-js/index.html).
[block:api-header]
{
  "type": "basic",
  "title": "Installing the Library"
}
[/block]
There are a couple of ways to get the Syncano Library - through `npm`, `bower` or directly from our GitHub repository.

## npm
Install the [npm module](https://www.npmjs.com/package/syncano) using:
[block:code]
{
  "codes": [
    {
      "code": "npm install syncano --save",
      "language": "shell"
    }
  ]
}
[/block]
## Bower
Install the [bower module](https://www.bower.io) using:
[block:code]
{
  "codes": [
    {
      "code": "bower install syncano",
      "language": "shell"
    }
  ]
}
[/block]
##CDN
We also provide the `syncano.min.js` file via jsDelivr.
[block:code]
{
  "codes": [
    {
      "code": "<script src=\"//cdn.jsdelivr.net/syncanojs/1.0.5/syncano.min.js\"></script>",
      "language": "text"
    }
  ]
}
[/block]
## GitHub

Download the latest version [here](https://github.com/Syncano/syncano-js/archive/master.zip) or browse our repo [here](https://github.com/syncano/syncano-js).
[block:api-header]
{
  "type": "basic",
  "title": "Using Syncano library"
}
[/block]
To use Syncano with the client, include the following script tag in your HTML document:
[block:code]
{
  "codes": [
    {
      "code": "<script src=\"path/to/bower_components/syncano/dist/syncano.min.js\"></script>",
      "language": "html"
    }
  ]
}
[/block]
Using Syncano with a CommonJS pattern in Node.js, or using Browserify or WebPack, simply `require` Syncano.
[block:code]
{
  "codes": [
    {
      "code": "var Syncano = require('syncano');",
      "language": "javascript"
    }
  ]
}
[/block]
Syncano utilizes a method chaining pattern to provide a flexible API. To access the API from the most basic level requires instantiating a new object.
[block:code]
{
  "codes": [
    {
      "code": "// Create empty connection\nvar connection = Syncano();\n\n// Create a connection with an account key\nvar connection = Syncano({ accountKey: 'MY_ACCOUNT_KEY'});\n\n// Create a connection with a user key\nvar connection = Syncano({ userKey: 'USER_KEY'});\n\n// Create a connection with a social token\nvar connection = Syncano({ userKey: 'SOCIAL_TOKEN'});",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Promises"
}
[/block]
As a convenience, the Syncano JavaScript library also uses [Bluebird](https://github.com/petkaantonov/bluebird) to provide a promise interface that you can utilize. On a successful call, `success` will return the `serialized response` from the API call, on a failed call it will return the `error` object. 
[block:code]
{
  "codes": [
    {
      "code": "var success = function(instances) {\n  console.log(instances);\n};\nvar error = function(error) {\n  console.log(error);\n};\n\nconnection.Instance.please().list().then(success).catch(error);",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Model instances"
}
[/block]
The `connection` you configured in the previous step has a set of factories that make interacting with objects on the platform easier. For example, if you would like to create a new instance object, you can do it like so:
[block:code]
{
  "codes": [
    {
      "code": "var instance = connection.Instance({\n  name: 'INSTANCE_NAME', \n  description: 'INSTANCE_DESCRIPTION'\n});",
      "language": "javascript"
    }
  ]
}
[/block]
You can later save the instance, by calling its `save` method:
[block:code]
{
  "codes": [
    {
      "code": "instance.save().then(function(instance) {\n\tconsole.log('instance', instance);\n});",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "QuerySet"
}
[/block]
Every model has a static `please` method that returns a `QuerySet` object allowing you to perform additional queries like listing objects:

[block:code]
{
  "codes": [
    {
      "code": "connection.Instance.please().list().then(function(instances) {\n\tconsole.log('instances', instances);\n});",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Nested models"
}
[/block]
The objects (models) are also nested, so if you would like to list the `Classes` of an `Instance`, there's an elegant function chain for that:
[block:code]
{
  "codes": [
    {
      "code": "connection.Instance({name: 'silent-dawn-3609'}).classes().list().then(function(classes) {\n\tconsole.log('classes', classes);\n});",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Contributing"
}
[/block]
This library is built using [Stampit](https://github.com/stampit-org/stampit). If you find a bug, feel free to submit an [issue](https://github.com/Syncano/syncano-js/issues). If you would like to directly contribute to the library, we are open for [pull requests](https://github.com/Syncano/syncano-js).