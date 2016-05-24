---
title: "Data Endpoints"
excerpt: "In this chapter you will learn:\n- What are Data Endpoints\n- How to create Data Endpoints\n- How to expand the Data Endpoints"
---
[block:api-header]
{
  "type": "basic",
  "title": "Chapter Contents"
}
[/block]
1. [Overview](#overview)
2. [What are Data Endpoints](#what-are-data-endpoints)
3. [How to create Data Endpoints](#how-to-create-data-endpoints)
4. [Summary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
In the previous chapters, you have learned how to create, filter and order Data Objects. But what if you wanted to make a call that would filter and order the response, exclude some fields and return a specific numbers of Data Objects on page? 
Of course you can do that with Syncano but the call would have plenty of parameters and it would be at least cumbersome to use. And what if you wanted to have 10 of that kind of calls but with different parameters? Or 50? A nightmare to maintain!

Fortunately, we have a solution for that and it's called Data Endpoints. With this feature you can configure calls to Data Object endpoints and save them to be used later. 
This way, you won't have to pass all the parameters every time you need to make a specific call. All you need to do, is make a call to specific Data Endpoint, that has all the parameters preconfigured. 

Ok, let's see the Data Endpoints in action!
[block:callout]
{
  "type": "info",
  "body": "This chapter assumes that you already know how to create [Classes](doc:classes) and [Data Objects](doc:data-objects) and you are familiar with [Data Object filtering and ordering](doc:data-objects-filtering)"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "What are Data Endpoints"
}
[/block]
As mentioned above, Data Endpoints are custom calls to Data Objects endpoint that you can configure to suit your needs and save for later use. 
These are the parameters that you can set up when creating a View:
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "0-0": "`order_by`",
    "0-1": "You specify the ordering with this paramter. You will need to pass the field you want to order by as value. Default ordering is `ascending`.",
    "1-0": "`page_size`",
    "2-0": "`expand`",
    "1-1": "Page size for the returned Data Objects. (You can read more about paging [here](http://docs.syncano.com/v0.1.1/docs/general-information#paging))",
    "2-1": "Now this one is a bit tricky. Remember that you can create a reference field inside a Data Object that will point it to a [different Data Object from a target Class](doc:classes#section-types-of-class-schema-fields)? By passing a referenced Class name in the `expand` field, you'll be able to see whole referenced Data Objects in the configured View. More on this functionality in the examples below.",
    "3-0": "`query`*",
    "3-1": "In this field you can specify a filtering method just as you would when using Data Objects filtering.",
    "4-0": "`excluded_fields`*",
    "4-1": "Data Object fields that you would like to exclude from the response."
  },
  "cols": 2,
  "rows": 5
}
[/block]
\* currently the `query` and `excluded_fields` params are only available through the REST API.
[block:api-header]
{
  "type": "basic",
  "title": "How to create Data Endpoints"
}
[/block]
1. [Prerequisites](#section-prerequisites)
2. [Creating a Data Endpoint](#section-creating-a-data-endpoint)
3. [Calling a Data Endpoint](#section-calling-a-data-endpoint)

### Prerequisites

Before we create any Data Endpoints, we should have a Class ready. It would be good if it was also populated with some Data Objects. Lets say we have two Classes  "inventory" and "warriors" with the following schemas:
- "inventory" Class:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/mahlrUWETPOu7XbnpXjr",
        "inventory_class.png",
        "700",
        "130",
        "#8b9db8",
        ""
      ]
    }
  ]
}
[/block]
Inventory Class has two string fields, which will hold values of weapon and armor as strings.

- "warriors Class:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/jKQ45Y9KRyqK7riG2uPr",
        "warriors_class.png",
        "708",
        "186",
        "#3b5077",
        ""
      ]
    }
  ]
}
[/block]
Warriors Class has a name field. which will describe the warrior type, number field stating the number of warriors of this type and an inventory field, which references the "Inventory" Class.

### Creating a Data Endpoint

Ok, let's create a Data Endpoint! 
Our goal here is:
- The response should be sorted ascending by the `number` field
- Inventory reference field should be expanded, so that it shows the referenced Data Objects
- Response shouldn't consist of more than 4 Data Objects

Our view will be called `troops` and will be based on the `warriors` Class. 
To create such a view:
1. Go to the Sockets page
2. Click 'Create Data Endpoint' button
3. Fill and check all the fields as on the screenshots below
4. Click 'Confirm' button

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/pYbnyvZpTWKnTSWLmPqJ",
        "Add_data_endpoint_00.png",
        "1279",
        "655",
        "#22426f",
        ""
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/JlNnbm4iREeKDVJ1fW2d",
        "Add_data_endpoint_01.png",
        "1279",
        "655",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
###  Calling a Data Endpoint

Now when we have a Data Endpoint created, we can make a call to it. This is how such a call will look like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: API_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/endpoints/data/troops/get/\"",
      "language": "curl"
    }
  ]
}
[/block]
The response will return the Data Objects that will meet the criteria we defined when creating the view (removed default properties for clarity):
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"next\": null,\n    \"objects\": [\n        {\n            \"name\": \"sorcerers\",\n            \"number\": 10,\n            \"id\": 14,\n            \"inventory\": {\n                \"armor\": \"cloak\",\n                \"id\": 11,\n                \"weapon\": \"dagger\"\n            }\n        },\n        {\n            \"name\": \"palladins\",\n            \"number\": 20,\n            \"id\": 13,\n            \"inventory\": {\n                \"armor\": \"full plate armor\",\n                \"id\": 15,\n                \"weapon\": \"halberd\"\n            }\n        },\n        {\n            \"name\": \"goblins\",\n            \"number\": 500,\n            \"id\": 12,\n            \"inventory\": {\n                \"armor\": \"leather jacket\",\n                \"id\": 10,\n                \"weapon\": \"sword\"\n            }\n        },\n        {\n            \"name\": \"orcs\",\n            \"number\": 1000,\n            \"id\": 16,\n            \"inventory\": {\n                \"armor\": \"leather armor\",\n                \"id\": 9,\n                \"weapon\": \"axe\"\n            }\n        }\n    ],\n    \"prev\": null\n}",
      "language": "json"
    }
  ]
}
[/block]
We've got exactly what we wanted. 
Our Data Endpoint returned warriors, that are sorted in an ascending order. 
The inventory field that references the "inventory" Class, has it's fields expanded, so that you have access to all its fields. What is more, the view is limited to four results per page - exactly like we set it up.
[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
You have just learned how to make Data Endpoints. 
It will allow you to create really flexible and custom calls to Syncano and reduce the number of operations on the client side. 
That's everything there is to know about working with Data Objects. 

Next on our reading list is learning about a feature allowing you to create and run scripts in the cloud, called [Script Endpoints](doc:endpoints-scripts).