---
title: "API keys"
excerpt: ""
---
## Creating new API key
[block:code]
{
  "codes": [
    {
      "code": "var Syncano = require(\"syncano4.js\");\n\n// var syncano = new Syncano();\n// syncano.connect(\"our_API_key\");\n// syncano.setInstance(\"instanceName\");\n\nsyncano.ApiKeys.create().then(function (output) {\n\t\tconsole.log(output)\n\t}, function (err) {\n\t\tconsole.log(err)\n\t})",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\napi_key_instance = ApiKey.please.create()\napi_key = api_key_instance.api_key",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## Get API key
[block:code]
{
  "codes": [
    {
      "code": "var Syncano = require(\"syncano4.js\");\n\n// var syncano = new Syncano();\n// syncano.connect(\"our_API_key\");\n// syncano.setInstance(\"instanceName\");\n\nvar apiKeyId = 199;\n\n\tsyncano.ApiKeys.get(apiKeyId).then(function (output) {\n\t\tconsole.log(output)\n\t}, function (err) {\n\t\tconsole.log(err)\n\t})",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\napi_key_instance = ApiKey.please.get(id=31531)\napi_key = api_key_instance.api_key",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## Removing API key
[block:code]
{
  "codes": [
    {
      "code": "var Syncano = require(\"syncano4.js\");\n\n// var syncano = new Syncano();\n// syncano.connect(\"our_API_key\");\n// syncano.setInstance(\"instanceName\");\n\nvar apiKeyId = 199;\n\nsyncano.ApiKeys.remove(apiKeyId).then(function (output) {\n\t\tconsole.log(output)\n\t}, function (err) {\n\t\tconsole.log(err)\n\t})",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\napi_key_instance = ApiKey.please.get(id=31531)\napi_key_instance.delete()",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
## List API keys
[block:code]
{
  "codes": [
    {
      "code": "var Syncano = require(\"syncano4.js\");\n\n// var syncano = new Syncano();\n// syncano.connect(\"our_API_key\");\n// syncano.setInstance(\"instanceName\");\n\nsyncano.ApiKeys.list().then(function (output) {\n\t\tconsole.log(output)\n\t}, function (err) {\n\t\tconsole.log(err)\n\t})",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import ApiKey\n\napi_key_instances = ApiKey.please.list()\nfor api_key_instance in api_key_instances:\n    print(api_key_instance.api_key)",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]