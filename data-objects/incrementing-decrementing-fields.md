---
title: "Incrementing / decrementing fields"
excerpt: ""
---
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
As you already know, Data Objects can have various custom field types (read about it [here](classes#section-types-of-class-schema-fields)). One of the possible types is `integer`. If you choose to add this field you can not only set the field value but also increment or decrement it by a certain amount (for example `1` or `5`).
[block:api-header]
{
  "type": "basic",
  "title": "How does it work?"
}
[/block]
Let's say we have a game app and we created a Class named `Player`. It holds information about game users, and has one attribute called `number_of_games`. Players can buy games inside your app (we will be increasing `number_of_games` value) and every time they play a game - we want to decrease value of `number_of_games` by 1. We could do it by updating it's value - e.g. when the old value was 20, we could update it with 19. But to avoid conflicts, for situations where game is played at the same time on two devices, it's much safer to user our increase/decrease api.

Instead of sending a new value of the field when a player buys 10 games, you could send the following: 
[block:code]
{
  "codes": [
    {
      "code": "{“number_of_games”: {“_increment”: 10}}",
      "language": "json"
    }
  ]
}
[/block]
When player played 1 game and we want to decrease it by one, we could send:
[block:code]
{
  "codes": [
    {
      "code": "{“number_of_games”: {“_increment”: -1}} ",
      "language": "json"
    }
  ]
}
[/block]
This way, you don't even have to know the old value of the field!

The whole request could look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X PATCH \\\n-H \"X-API-KEY: API_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"number_of_games\": {\"_increment\":10}}' \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/player/objects/data_object_id/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Object\n\nconnection = syncano.connect(api_key='API_KEY')\n\nObject.please.increment(\n  'counter', #field to increment\n  1, #by what value to increment\n  instance_name='instance_name, \n  class_name='class_name', \n  id=object_id #e.g. id = 45 \n  different_string_field = 'new content' #you can also update different fields \n)",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar DataObject = connection.DataObject;\n\nvar query = {\n\tid: 7,\n  instanceName: \"INSTANCE_NAME\",\n  className: \"CLASS_name\"\n};\n\nvar book = {\n  number_of_games: {_increment: -1}\n};\n\nDataObject.please().update(query, book).then(function(book) {\n\tconsole.log(\"book\", book)\n});",
      "language": "javascript"
    },
    {
      "code": "Book book = new Book();\n// object to update has to have id set\nbook.setId(serverId);\nbook.increment(\"number_of_games\", 10);\nbook.save();",
      "language": "java",
      "name": "Android"
    }
  ]
}
[/block]