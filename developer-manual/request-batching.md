---
title: "Request Batching"
excerpt: "In this chapter you'll learn:\n- What are batch requests\n- How to create a batch requests\n- How to execute a batch requests"
---
[block:api-header]
{
  "type": "basic",
  "title": "Chapter Contents"
}
[/block]
1. [Introduction](#introduction)
2. [Overview](#overview)
3. [Creating a Batch Request](#creating-a-batch-request)
4. [Summary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Introduction"
}
[/block]
There might be cases where instead of sending single requests to Syncano, you'd like to group them into a one single, mighty request to rule them all. 

We thought that you might like that and came up with **The Batch Requests**! 

The concept behind batch requests is pretty simple. You make a request to the `batch` endpoint with a payload, where you specify a list of requests that you'd like to make. 
In the response, you'll receive an array of objects containing the responses codes and content for the particular responses. 
We'll get to the details below.
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Creating a batch requests might differ when, depending on which library you use. 
There are several rules though, that are common for all the libraries.
Inside a batch request, you can put all standard CRUD (create, read, update and delete) operations. 

When creating a batch request you can mix and match them freely. What is more -- the calls are not limited only to Data Objects. You can create a batch requests for any resource within a given instance. 
For example, you can create a Data Object in one class, delete a Channel and reset an Api Key. There are couple of limits that you should be aware of though:
[block:callout]
{
  "type": "warning",
  "title": "Limits",
  "body": "- Number of requests can't exceed 50 per batch call\n- Currently, it's not possible upload files using batch requests (when using Data Objects endpoint)\n- Batch calls should be made within a scope of a single instance"
}
[/block]
Since the way of making a batch request differs for each library, the explanation is divided into categories:
[block:code]
{
  "codes": [
    {
      "code": "Endpoint for making batch requests:\nhttps://api.syncano.io/v1/instances/INSTANCE_NAME/batch/\n\nAn array of requests should be passed as a request payload\nEach request in the array should consist of:\n\n- \"method\" type: GET / POST / PATCH / PUT / DELETE\n- \"path\": A location of the resource i.e.: \"/v1.1/instances/INSTANCE_NAME/classes/\"\n- \"body\": JSON dictionary of additional parameters\n\nIn case of GET and DELETE requests the body property is ignored.\nCredentials (API Key, Account Key, User Key) from the batch request are passed along to every sub request",
      "name": "cURL",
      "language": "curl"
    },
    {
      "code": "# The general syntax for making batch requests with python library is as follows:\n\nklass = instance.classes.get(name='some_class')\n\nObject.please.batch(\n    klass.objects.as_batch().delete(id=652),\n    klass.objects.as_batch().delete(id=653)\n)\n\n\"\"\"\nYou can mix up the create, delete and update requests but what is important to note is that the get() create() and get_or_create() requests won't work.\n\nYou can read more about the batch requests in the Syncano Python API Reference Library: http://syncano.github.io/syncano-python/refs/syncano.models.manager.html#module-syncano.models.manager\n\"\"\"\n",
      "name": "python",
      "language": "python"
    },
    {
      "code": "# Coming soon!",
      "name": "JavaScript",
      "language": "javascript"
    },
    {
      "code": "# Coming soon!",
      "name": "",
      "language": "objectivec"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "name": "Ruby",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Creating a Batch Request"
}
[/block]
Let's try to use that information and design a batch request. 

We'll create a batch request that will:
- Add an API Key with an `allow_user_create` flag
- Update a Data Object with an `id = 38`, from a class named `class`
- Delete a CodeBox with an `id = 2`

This is how this batch request should look like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\n\"requests\":[\n  {\n     \"method\":\"POST\",\n     \"path\":\"/v1.1/instances/INSTANCE_NAME/api_keys/\"\n     \"body\": {\"allow_user_create\": true}\n  },\n  {\n     \"method\":\"PATCH\",\n     \"path\":\"/v1.1/instances/INSTANCE_NAME/classes/class/objects/38/\",\n     \"body\": {\"string\": \"new\"}\n  },\n  {\n     \"method\":\"DELETE\",\n     \"path\":\"/v1.1/instances/INSTANCE_NAME/snippets/scripts/2/\"\n  }\n]\n}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/batch/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models.base import Instance\n\nconnection = syncano.connect(api_key='ACCOUNT_KEY')\n\ninstance = Instance(name='red-smoke-9323')\nklass = instance.classes.get(name='class')\n\nObject.please.batch(\n    instance.api_keys.as_batch().create(allow_user_create=True),\n    klass.objects.as_batch().update(id=14, string='new'),\n    instance.codeboxes.as_batch().delete(id=3)\n)",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
    },
    {
      "code": "// delete one object, update other\nBatchBuilder batch = new BatchBuilder(syncano);\nbatch.add(syncano.updateObject(book1));\nbatch.add(syncano.deleteObject(book2));\nResponse<List<BatchAnswer>> response = batch.send();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
And this is the response we received:
[block:code]
{
  "codes": [
    {
      "code": "[\n    {\n        \"code\": 201,\n        \"content\": {\n            \"allow_anonymous_read\": false,\n            \"allow_user_create\": true,\n            \"api_key\": \"e91c2410b9ecad7f00b48edbec11c6076cb2adbd\",\n            \"created_at\": \"2016-01-28T14:20:01.451813Z\",\n            \"description\": \"\",\n            \"id\": 25831,\n            \"ignore_acl\": false,\n            \"links\": {\n                \"reset_key\": \"/v1/instances/INSTANCE_NAME/api_keys/25831/reset_key/\",\n                \"self\": \"/v1.1/instances/INSTANCE_NAME/api_keys/25831/\"\n            }\n        }\n    },\n    {\n        \"code\": 200,\n        \"content\": {\n            \"channel\": null,\n            \"channel_room\": null,\n            \"created_at\": \"2016-01-27T13:09:25.621678Z\",\n            \"group\": null,\n            \"group_permissions\": \"none\",\n            \"id\": 38,\n            \"links\": {\n                \"self\": \"/v1.1/instances/INSTANCE_NAME/classes/class/objects/38/\"\n            },\n            \"other_permissions\": \"none\",\n            \"owner\": null,\n            \"owner_permissions\": \"full\",\n            \"revision\": 2,\n            \"string\": \"new\",\n            \"updated_at\": \"2016-01-28T14:20:01.481877Z\"\n        }\n    },\n    {\n        \"code\": 404,\n        \"content\": {\n            \"detail\": \"Not found.\"\n        }\n    }\n]",
      "language": "curl"
    },
    {
      "code": "[<ApiKey: 3378>, <RedSmoke9323ClassObject: 14>, {u'content': {u'detail': u'Not found.'}, u'code': 404}]",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
    },
    {
      "code": "List<BatchAnswer> answers = response.getData();\nBook bookUdpated = answers.get(0).getDataAs(Book.class);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec"
    },
    {
      "code": "// Coming soon!",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
As you can see, the response has a form of an array, that contains three objects. 
These objects correspond to the requests made in the batch call. 
The first one, is the response to the first call (create API Key), the second to the second call (update Data Object) etc. 

Each of the objects, contains two properties:
- `code`: it's the HTTP Status code of the response
- `content`: response body

In the above example:
- First call returned `201 CREATED` status code and content with info on created Api Key (with the `allow_user_create` set to true)
- Second call returned `200 OK` status code and content with an updated a Data Object from the `class` Class
- Third call returned `404 NOT FOUND` error code and content with the error message. This was expected, since we tried to delete a CodeBox that doesn't exist.


[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
That's it! We hope making batch requests on Syncano will make your life easier!
[block:callout]
{
  "type": "info",
  "title": "Pricing for Batch calls",
  "body": "Pricing for the batch calls is the same as if the requests where sent individually. \nE.g. if you send 50 API Calls to Download Objects - it will be counted as 50 API Calls against your limit. If you launch 50 CodeBoxes - it will also be counted as 50 CodeBox runs."
}
[/block]