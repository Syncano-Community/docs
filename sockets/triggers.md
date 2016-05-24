---
title: "Trigger Sockets"
excerpt: "In this chapter you will read about:\n* What is a Trigger and what are its properties\n* How to use a Trigger in a real-life example\n* Possible usage examples"
---
## Chapter content:
1. [Trigger Sockets Overview](#trigger-sockets-overview)
2. [Using Trigger Sockets](#using-trigger-sockets)
3. [Sumary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Trigger Sockets Overview"
}
[/block]
Apart from directly running Snippet Scripts, or using CodBoxes and Schedules, Trigger Sockets are another way of executing Scripts. Trigger Sockets execute a Script when a Data Object inside selected Class is created, updated or deleted (depends on "signal" field value). 
This is how a Trigger Socket object can look:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"label\": \"get reviews\", \n    \"signal\": \"post_create\",\n    \"description\": \"this trigger will start a snippet script with an id = 1\",\n    \"class\": \"books\", \n    \"created_at\": \"2015-04-25T10:55:37.957096Z\", \n    \"updated_at\": \"2015-04-25T10:55:37.957138Z\", \n    \"script\": 1, \n    \"links\": {\n        \"script\": \"/v1.1/instances/bookstore/snippets/scripts/1/\", \n        \"self\": \"/v1.1/instances/bookstore/triggers/1/\", \n        \"traces\": \"/v1.1/instances/bookstore/triggers/1/traces/\", \n        \"class\": \"/v1.1/instances/bookstore/classes/books/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
And these are the unique properties of a Trigger Socket:

[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "signal",
    "0-1": "This property determines on what condition the Snippet Script should run. There are three possible values here:\n* `post_create` - run Script when a Data Object is created\n* `post_update` - run Script when a Data Object is updated\n* `post_delete` - run Script when a Data Object is removed",
    "1-0": "class",
    "1-1": "Name of a class. A Trigger will execute a Script when changes happen to a Data Object is this class.",
    "2-0": "script",
    "2-1": "`id` of a Snippet Script that'll be executed by this trigger."
  },
  "cols": 2,
  "rows": 3
}
[/block]
Since we now know what Triggers are, we can now move to a real-life example.
[block:api-header]
{
  "type": "basic",
  "title": "Using Trigger Sockets"
}
[/block]
So how can we use triggers to our advantage?
[block:callout]
{
  "type": "info",
  "body": "This tutorial assumes that you are familiar with [Classes](classes) and [Snippet Scripts](snippets-scripts) concepts."
}
[/block]
Let's use our beloved books example. We've got a library application that people add books to. We'd like to get reviews for a book whenever someone adds a book to our library.

What we need to do:
1. Create a `book` Class
2. Create a Snippet Script that'll get us books reviews
3. Add a trigger that'll run when a new book Data Object is added to books Class
4. Add a book Data Object and enjoy the results!

## Create a books Class

Our book Data Objects will have two custom fields which are `title` and `isbn`.
[block:callout]
{
  "type": "info",
  "body": "`isbn` stands for [International Standard Book Number](https://en.wikipedia.org/wiki/International_Standard_Book_Number). We'll need it for our Script to work."
}
[/block]
In order to be able to add our Data Objects, first create a Class that uses this schema:
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Type",
    "0-0": "title",
    "0-1": "string",
    "1-0": "isbn",
    "1-1": "string"
  },
  "cols": 2,
  "rows": 2
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "For details on adding a new Class, follow the guide in [Classes](doc:classes#creating-a-class) chapter of this Developer Manual."
}
[/block]
## Create a Snippet Script that'll get us books reviews

Now it's time for the magic. Since we are lazy and don't want to write the reviews ourselves, we'll use [Goodreads API](https://www.goodreads.com/api/) to get them. What the source code below does is:
1. Gets the isbn of the most recently added Data Object 
2. Goes to Goodreads and gets the reviews
3. Returns the reviews widget for our book
[block:code]
{
  "codes": [
    {
      "code": "import json\nimport requests\n\nisbn = ARGS['isbn']\n\ngoodreads_api = \"https://www.goodreads.com/book/isbn?format=json&isbn=\"+isbn+\"&user_id=42491463\"\nr = requests.get(goodreads_api)\nprint json.loads(r.content)[\"reviews_widget\"]",
      "language": "python"
    },
    {
      "code": "var request = require('request');\n\nvar goodreadsApi = 'https://www.goodreads.com/book/isbn?format=json&isbn=%' + ARGS.isbn + '&user_id=42491463';\n\nrequest.get(goodreadsApi, function(err, res) {\n  if (err) {console.log(err); return;}\n  console.log(JSON.parse(body).reviews_widget);\n  return;\n});",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "For details on adding a Snippet Script using the Dashboard, please follow the guide in [Snippet Scripts](doc:snippets-scripts#adding-a-new-script) chapter of this Developer Manual."
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "ARGS",
  "body": "It is important to note that you can access the attributes of the Data Object that triggered the Script run. The attributes can be accessed by using the built in ARGS variable. For instance, in the example above we are accessing `isbn` attribute of a book Data Object which has just triggered the `get reviews` Script."
}
[/block]
## Add a Trigger Socket

This that'll run when a new book Data Object is added to books Class

Now what we'd like to do is to make our Snippet Script code run whenever someone adds a new book to our library (books class). Enter Trigger Sockets! We can create a Trigger Socket that'll run the above Script when someone adds a Data Object to books Class. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/xtwbaVfDTL64RFhcH1wx",
        "Add_trigger_01.png",
        "1279",
        "653",
        "#233f6a",
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
        "https://www.filepicker.io/api/file/59UrL1koStCggvEgNAL2",
        "Add_trigger_02.png",
        "1279",
        "657",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
You have added your trigger socket! Now lets check out how it works.

## Add a book Data Object to see the Trigger Sockets in action!
Since we now have all the elements in place, let's test it out by adding a book Data Object. We'll add "Fight Club" which has the isbn# `9780805062977`.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: application/json\" \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-d '{\"title\": \"Fight Club\", \"isbn\": \"9780805062977\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/books/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nsyncano.connect(api_key=\"Api_key\")\n\nObject.please.create(\n  title=\"Fight Club\",\n  isbn=\"9780805062977\",\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"books\"\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar book = {\n  title: \"Fight Club\", \n  isbn: \"9780805062977\",\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_NAME\"\n};\n\nDataObject.please().create(book).then(callback);",
      "language": "javascript"
    },
    {
      "code": "// Not supported",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "@interface Book : SCDataObject\n@property (nonatomic,copy) NSString *title;\n@property (nonatomic,copy) NSString *isbn;\n@end\n  \nBook *book = [Book new];\nbook.title = @\"Fight Club\";\nbook.isbn = @\"9780805062977\";\n[book saveWithCompletionBlock:^(NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "class Book : SCDataObject {\n  var title = \"\"\n  var isbn = \"\"\n}\n\nlet book = Book()\nbook.title = \"Fight Club\"\nbook.isbn = \"9780805062977\"\nbook.saveWithCompletionBlock { error in\n  //handle error\n}",
      "language": "javascript",
      "name": "Swift"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    }
  ]
}
[/block]
This Data Object creation triggered our Snippet Script execution. To see the results we could make a call to the [Trigger Sockets Traces endpoint](traces), get the latest ID and get the trace. This is how the response would look:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1, \n    \"status\": \"success\", \n    \"executed_at\": \"2015-04-25T11:01:18.595539Z\", \n    \"duration\": 824, \n    \"result\": \"<style>\\n  #goodreads-widget {\\n    font-family: georgia, serif;\\n    padding: 18px 0;\\n    width:565px;\\n  }\\n  #goodreads-widget h1 {\\n    font-weight:normal;\\n    font-size: 16px;\\n    border-bottom: 1px solid #BBB596;\\n    margin-bottom: 0;\\n  }\\n  #goodreads-widget a {\\n    text-decoration: none;\\n    color:#660;\\n  }\\n  iframe{\\n    background-color: #fff;\\n  }\\n  #goodreads-widget a:hover { text-decoration: underline; }\\n  #goodreads-widget a:active {\\n    color:#660;\\n  }\\n  #gr_footer {\\n    width: 100%;\\n    border-top: 1px solid #BBB596;\\n    text-align: right;\\n  }\\n  #goodreads-widget .gr_branding{\\n    color: #382110;\\n    font-size: 11px;\\n    text-decoration: none;\\n    font-family: \\\"Helvetica Neue\\\", Helvetica, Arial, sans-serif;\\n  }\\n</style>\\n<div id=\\\"goodreads-widget\\\">\\n  <div id=\\\"gr_header\\\"><h1><a href=\\\"https://www.goodreads.com/book/show/6397843-wapr\\\">wapr Reviews</a></h1></div>\\n  <iframe id=\\\"the_iframe\\\" src=\\\"https://www.goodreads.com/api/reviews_widget_iframe?did=DEVELOPER_ID&amp;format=html&amp;isbn=isbn&amp;links=660&amp;review_back=fff&amp;stars=000&amp;text=000\\\" width=\\\"565\\\" height=\\\"400\\\" frameborder=\\\"0\\\"></iframe>\\n  <div id=\\\"gr_footer\\\">\\n    <a class=\\\"gr_branding\\\" href=\\\"https://www.goodreads.com/book/show/6397843-wapr?utm_medium=api&amp;utm_source=reviews_widget\\\" target=\\\"_blank\\\">Reviews from Goodreads.com</a>\\n  </div>\\n</div>\", \n    \"links\": {\n        \"self\": \"/v1.1/instances/bookstore/triggers/1/traces/1/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
What do have in the `result` field? It's a nice Goodreads reviews iframe that we can embed on our website just like we did below:
[block:html]
{
  "html": "<style>\n  #goodreads-widget {\n    font-family: georgia, serif;\n    padding: 18px 0;\n    width:565px;\n  }\n  #goodreads-widget h1 {\n    font-weight:normal;\n    font-size: 16px;\n    border-bottom: 1px solid #BBB596;\n    margin-bottom: 0;\n  }\n  #goodreads-widget a {\n    text-decoration: none;\n    color:#660;\n  }\n  iframe{\n    background-color: #fff;\n  }\n  #goodreads-widget a:hover { text-decoration: underline; }\n  #goodreads-widget a:active {\n    color:#660;\n  }\n  #gr_footer {\n    width: 100%;\n    border-top: 1px solid #BBB596;\n    text-align: right;\n  }\n  #goodreads-widget .gr_branding{\n    color: #382110;\n    font-size: 11px;\n    text-decoration: none;\n    font-family: \"Helvetica Neue\", Helvetica, Arial, sans-serif;\n  }\n</style>\n<div id=\"goodreads-widget\">\n  <div id=\"gr_header\"><h1><a href=\"https://www.goodreads.com/book/show/375782.Fight_Club\">Fight Club Reviews</a></h1></div>\n  <iframe id=\"the_iframe\" src=\"https://www.goodreads.com/api/reviews_widget_iframe?did=DEVELOPER_ID&amp;format=html&amp;isbn=0805062971&amp;links=660&amp;review_back=fff&amp;stars=000&amp;text=000\" width=\"565\" height=\"400\" frameborder=\"0\"></iframe>\n  <div id=\"gr_footer\">\n    <a class=\"gr_branding\" href=\"https://www.goodreads.com/book/show/375782.Fight_Club?utm_medium=api&amp;utm_source=reviews_widget\" target=\"_blank\">Reviews from Goodreads.com</a>\n  </div>\n</div>"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]

We've learned what Trigger Sockets are and how to use them in a real life. You can learn about other Snippet Scripts execution methods like [Script Endpoints](doc:endpoints-scripts) or [Schedule Sockets](doc:schedules). If you already know how these work, [Channel Sockets](doc:realtime-communication) chapter is the next recommended read.
[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For more details about the available methods for Trigger Sockets, visit the [Trigger Sockets API reference](http://docs.syncano.com/v0.1.1/docs/triggers-list)."
}
[/block]