---
title: "[Meta]"
excerpt: ""
---
[block:api-header]
{
  "type": "basic",
  "title": "Examples"
}
[/block]
Proper ordering and formatting of code examples:
[block:code]
{
  "codes": [
    {
      "code": "(curl call examples)\n- api_key user_key vars <THIS_WAY>\n- dummy data in json payloads\n- vars in urls <this_way>\n- urls in brackets \"www.brackets.com\"\n\norder as below, so:\n1. curl + call type (POST, GET)\n2. auth\n3. Content type if needed\n4. Content if needed\n5. url\n\ncurl -X POST \\\n-H \"X-API-KEY: <ACCOUNT_KEY>\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\":\"instance_name\", \n \"description\":\"this is a description\",\n \"meatadata\": [{\"color\":\"#123456\"}]}' \\\n\"https://api.syncano.io/v1/instances/\"\n\ncurl -X POST \\\n-H \"X-API-KEY: <API_KEY>\" \\\n-H \"X-USER-KEY: <USER_KEY>\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"name\": \"I am an object name}' \\\n\"https://api.syncano.io/v1/instances/<instance_name>/classes/<class>/objects/",
      "language": "curl"
    },
    {
      "code": "",
      "language": "python"
    },
    {
      "code": "",
      "language": "javascript"
    },
    {
      "code": "",
      "language": "java",
      "name": "android"
    },
    {
      "code": "",
      "language": "objectivec"
    },
    {
      "code": "",
      "language": "objectivec",
      "name": "Swift"
    },
    {
      "code": "",
      "language": "ruby"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# Coming soon!",
      "language": "curl"
    },
    {
      "code": "# Coming soon!",
      "language": "python"
    },
    {
      "code": "// Coming soon!",
      "language": "javascript"
    },
    {
      "code": "// Coming soon!",
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

[block:callout]
{
  "type": "info",
  "body": "if you don't want to add all the languages in code sample manually, you can switch to raw mode on this page (\"cog\" icon in top right corner) and copy the above \"code block\" section."
}
[/block]
Examples should be quite verbose and possibly explain all the steps to achieve the result.