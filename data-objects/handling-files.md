---
title: "Handling files"
excerpt: "In this chapter you will learn:\n- What are Data Objects\n- What's inside a Data Object\n- How to perform different operations on Data Objects"
---
## Chapter contents

1. [Creating a Data Object with `file` property](#creating-a-data-object-with-file-property)
2. [Summary](#summary)
[block:api-header]
{
  "type": "basic",
  "title": "Creating a Data Object with `file` property"
}
[/block]
Adding Data Objects with the `file` property is a special case. First, make sure you've created a Class with a `file` field type in the Class Schema.

Once you have done that, this is how creating a Data Object with a file looks like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"Content-type: multipart/form-data\" \\\n-H \"X-API-KEY: API_KEY\" \\\n-F \"file=@PATH_TO_FILE\" \\\n\"https://api.syncano.io/v1.1/instances/instance_name/classes/class_name/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nimage_path = \"/sample_path/image.jpg\"\nimage_file = open(image_path, \"r\")\nObject.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  class_name=\"CLASS_NAME\",\n  image=image_file\n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\n// To upload your file, you have to use the `files` property of the input and attach it to the object via `Syncano.file` method. Do this during the submission of the form. \nvar object = {\n  file_field: Syncano.file(input.files[0]),\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\n// To upload files from the server environment, use the `fs`and `Syncano.file` method for attaching the file to your object.\n\nvar fs = require(\"fs\");\nvar object = {\n  file_field_name: Syncano.file(fs.readFileSync(\"path_to_my_file.txt\")),\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nDataObject.please().create(object).then(function(object) {\n\tconsole.log(\"object\", object);\n});",
      "language": "javascript"
    },
    {
      "code": "Book bookWithCover = new Book();\nbookWithCover.author = \"Ernest Hemingway\";\nbookWithCover.title = \"The Old Man and the Sea\";\nbookWithCover.cover = new SyncanoFile(new File(getAssetsDir(), \"blue.png\"));\n\nResponse<Book> response = bookWithCover.save();",
      "language": "java",
      "name": "Android"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Current limit for file size that can be stored on Syncano is 128MB."
}
[/block]
In the response, the `file` property will have to fields:
- type
- value

Value field is a url. The resource that you just uploaded is being stored under this url. 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"channel\": null,\n    \"channel_room\": null,\n    \"created_at\": \"2015-07-22T15:21:23.388062Z\",\n    \"file\": {\n        \"type\": \"file\",\n        \"value\": \"https://d3rij3t703q5l6.cloudfront.net/305/10/523956605dbc303fa74e06061f40ccf1ce1ea978.txt\"\n    },\n    \"group\": null,\n    \"group_permissions\": \"none\",\n    \"id\": 29,\n    \"links\": {\n        \"self\": \"/v1.1/instances/instance_name/classes/class/objects/29/\"\n    },\n    \"other_permissions\": \"none\",\n    \"owner\": null,\n    \"owner_permissions\": \"none\",\n    \"revision\": 1,\n    \"string\": null,\n    \"updated_at\": \"2015-07-22T15:21:23.388100Z\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
Great! Now you know the how to upload files to Syncano. Next chapter will be about adding spacial coordinates to your Data Objects, which are called [GeoPoints](doc:geopoints).