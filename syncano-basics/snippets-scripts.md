---
title: "Snippets - Scripts"
excerpt: "How do you run Snippet Scripts on Syncano? What can they do?"
---
## Chapter Contents:
1. [Overview](#overview)
2. [Adding a new Script](#adding-a-new-script)
3. [Runtimes and libraries](#runtimes-and-libraries)
4. [Running a Script](#running-script)
5. [Script Example](#script-code-example)
6. [Scripts data dictionaries: ARGS, CONFIG and META](#scripts-data-dictionaries-args-config-and-meta)
7. [Custom Script Timeouts](#section-script-timeouts-and-custom-timeout)
8. [Handling files in Scripts](#handling-files-in-scripts)
9. [Summary](#summary)

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
A [Script](glossary#section-snippet-script) is an object that contains code that can be run on Syncano's servers. 

### What can be accomplished with a Script

A Script is a very powerful tool. Just like with code, you can do a lot with it. Additionally, Syncano gives you many ways to run Scripts. This is where you can get creative. 

With a Script you can:

- compute tasks that are too expensive to compute on mobile/frontend side.
- create your own logic in the cloud that can be accessed by an API built on Script Endpoints (complex filtering, composing multiple objects)
- handle events on Syncano with Trigger Sockets
- trigger jobs in the cloud with the API
- run scheduled jobs - cloud cron
- build your own integrations between services with Scripts powered Script Endpoints
- create public Script Endpoints available for people that know the link

[block:callout]
{
  "type": "info",
  "title": "Note on pricing",
  "body": "Scripts pricing is based on total execution time. So, in terms of billing, 30 Script runs that took 1 second each are equal to one Script run that took 30 seconds. Builder plan allows for 20000 Script seconds per month."
}
[/block]
### Script Properties

So what exactly is this Script? This is how a Script object can look like:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"config\": {}, \n  \"id\": 1, \n  \"runtime_name\": \"python\", \n  \"label\": \"here\", \n  \"description\": \"there\", \n  \"source\": \"print('ello')\", \n  \"created_at\": \"2015-01-28T14:02:38.408864Z\", \n  \"updated_at\": \"2015-01-28T14:02:38.408920Z\", \n  \"links\": {\n      \"traces\": \"/v1.1/instances/syncano/snippets/scripts/1/traces/\", \n      \"self\": \"/v1.1/instances/syncano/snippets/scripts/1/\", \n      \"run\": \"/v1.1/instances/syncano/snippets/scripts/1/run/\", \n      \"runtimes\": \"/v1.1/instances/syncano/snippets/scripts/runtimes/\"\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
And here's an overview of the unique properties that Script has:
[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`runtime_name`",
    "0-1": "The `runtime environment` that the Script will run in (i.e. Ruby, Python, NodeJS, Golang, Swift and PHP). Every language is supported by a different runtime environment. The runtime environment also supports several libraries for each language. For example: The Python runtime has a `requests` library.",
    "1-0": "`source`",
    "1-1": "A piece of code that will run when the Script is executed.",
    "2-0": "`label`",
    "2-1": "Label of the Script",
    "3-0": "`description`",
    "3-1": "Script description",
    "4-1": "A very useful field that can be used to store your confidential data such as APIKEYS or any other configurable variables.",
    "4-0": "`config`"
  },
  "cols": 2,
  "rows": 5
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "For advanced users:",
  "body": "Runtimes are described by Dockerfiles, you can find all of them on our Github account:\n- https://github.com/Syncano/python-codebox\n- https://github.com/Syncano/golang-codebox\n- https://github.com/Syncano/nodejs-codebox\n- https://github.com/Syncano/ruby-codebox\n- https://github.com/syncano/swift-codebox\n- https://github.com/syncano/php-codebox"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Adding a new Script"
}
[/block]
The easiest way to add a new Script, is with help of the [Dashboard](https://dashboard.syncano.io). 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/0uS8T5ymRbeES06ShK2v",
        "Add_codebox_01.png",
        "1279",
        "654",
        "#234473",
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
        "https://www.filepicker.io/api/file/z5SkghuSjum5t89VXFzA",
        "Add_codebox_02.png",
        "1279",
        "655",
        "#6c84a4",
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
        "https://www.filepicker.io/api/file/LcpgJVUrSmGA6oqWZjJh",
        "Add_codebox_03.png",
        "1279",
        "653",
        "#25426f",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Runtimes and libraries"
}
[/block]
It is possible to write your code in one of few selected runtimes.

You can always find most up-to-date list with supported languages using our API. 
To do so, make a GET request on an `/instances/:instance_name/scripts/snippets/runtimes/` endpoint, e.g.:
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \\\n-H \"X-API-KEY: <ACCOUNT_KEY>\" \\\n\"https://api.syncano.io/v1.1/instances/<instance_name>/scripts/snippets/runtimes/\"",
      "language": "curl"
    }
  ]
}
[/block]
As of February 2016, we support following programming languages/runtimes:

- NodeJS
- Python
- Swift
- Golang
- PHP
- Ruby

For some of the languages, we support adding libraries/packages, from popular repositories/package managers. 
Currently it is not possible to define packages available in a Script using our Dashboard. Instead, to request a new library: 

- Write to us at [support@syncano.com](mailto:support@syncano.com). Let us know which library you'd like to have added for which language;
- or make a pull request to our repository (and change a file with list of available libraries)

You can find listing of libraries available in a Script on our GitHub:

- Python - https://github.com/Syncano/python-codebox/blob/master/requirements.txt
- NodeJS - https://github.com/Syncano/nodejs-codebox/blob/master/package.json.j2

Would you like to add a library to a different language? Let us know by mailing us at [support@syncano.com](mailto:support@syncano.com).
[block:api-header]
{
  "type": "basic",
  "title": "Running Script"
}
[/block]
There are couple of possibilities here:
[block:parameters]
{
  "data": {
    "h-0": "Execution method",
    "h-1": "Description",
    "0-0": "Script.run()",
    "0-1": "Scripts can be executed directly with a `run()` method (It might look a bit differently depending on the library of your choice)",
    "1-0": "[Script Endpoints](endpoints-scripts)",
    "1-1": "With attached Script - with POST or GET to api - first you have to create and configure a Script",
    "2-0": "[Schedule Sockets](schedules)",
    "2-1": "Syncano will run a Script attached to a Schedule object repeatedly at a schedule that you specify. You have to create a [Schedule](schedules) first",
    "3-0": "[Trigger Sockets](triggers)",
    "3-1": "Syncano will run a Script attached to a Trigger Socket, when an event described by trigger occurs (i.e. a Data Object is created)"
  },
  "cols": 2,
  "rows": 4
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Scripts can only be run with an Account Key! API Key will not work on `/snippets/scripts/` endpoint!"
}
[/block]
Easiest way to run a Script Snippet is using the [Dashboard](https://dashboard.syncano.io).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/WQNHqgJHQiy1DbvruxVF",
        "Run_codebox_01.png",
        "1279",
        "654",
        "#27436f",
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
        "https://www.filepicker.io/api/file/Sto9w97uRgetip8NCpcy",
        "Run_codebox_02.png",
        "1279",
        "653",
        "#26416b",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Script Code Example"
}
[/block]
Ok, so once we have our Script created, we can write some code!

### Complex logic in a cloud

We will run some complex logic in a Script that would be too expensive to compute on mobile or some front-end.

Task: Predict if person likes cats or dogs more? (Not very complex nor expensive but it's an example, ok?!)

![https://www.flickr.com/photos/10233916@N03/3392281751](https://farm4.staticflickr.com/3459/3392281751_e90898ef14_m_d.jpg)

Example input that will be passed in a payload:
[block:code]
{
  "codes": [
    {
      "code": "{\"first_name\": \"Ryan\", \"last_name\": \"Gosling\"}",
      "language": "json",
      "name": "Input"
    }
  ]
}
[/block]
Example Output:
[block:code]
{
  "codes": [
    {
      "code": "{\"preferred\": \"cats\"}\n// OR\n{\"preferred\": \"dogs\"}",
      "language": "json",
      "name": "Output"
    }
  ]
}
[/block]
 Our algorithm will be pretty simple:
[block:code]
{
  "codes": [
    {
      "code": "import json\n\n# Get arguments passed in a payload. You can read about\n# ARGS in the Script data dictionaries section below\nfirst_name = ARGS.get(\"first_name\", None)\nlast_name = ARGS.get(\"last_name\", None)\n\npreferred = None\n\nif first_name and last_name:\n    if len(first_name) + len(last_name) % 2 == 0:\n        preferred = 'cats'\n    else:\n        preferred = 'dogs'\n        \n# now we know, what's the preference!\n\n\n# we only have to dump it as a json and print to std out\nprint(json.dumps({'preferred': preferred}))\n",
      "language": "python"
    },
    {
      "code": "// Get arguments passed in a payload. You can read about\n// ARGS in the Script data dictionaries section below\nvar first_name = ARGS.first_name;\nvar last_name = ARGS.last_name;\nvar preferred;\n\nif (first_name && last_name) {\n  if ((first_name.length + last_name.length) % 2 === 0) {\n    preferred = \"cats\";\n  }\n  else {\n    preferred = \"dogs\";\n  }\n}\n\n// use console.log to return data\nconsole.log(\"Preferrences: \", preferred);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "require 'json'\n\n# Get arguments passed in a payload. You can read about\n# ARGS in the Script data dictionaries section below\nfirst_name = ARGS[\"first_name\"]\nlast_name = ARGS[\"last_name\"]\n\npreferred = nil\n\nif first_name && last_name\n  if (first_name.length + last_name.length) % 2 == 0\n    preferred = 'cats'\n  else\n    preferred = 'dogs'\n        \n# now we know, what's the preference!\n\n\n# we only have to dump it as a json and print to std out\nputs { preferred: preferred }.to_json",
      "language": "ruby"
    },
    {
      "code": "// Courtesy of Bryan Smith. \n// https://github.com/bryanwsmith/syncano-examples-golang/\n//package main\n\nimport (\n    \"fmt\"\n    \"log\"\n)\n\nfunc main() {\n\n//uncomment to test without using Payload option\n    // ARGS[\"first_name\"] = \"Bryan\"\n    // ARGS[\"last_name\"] = \"Smith\"\n\n    value, ok := ARGS[\"first_name\"]\n    \n\n    if !ok { \n        log.Println(\"first_name is required\")\n        return \n    }\n\t\n\tfirstName, ok := value.(string)\n\n    if !ok { \n        log.Println(\"first_name must be a string\")\n        return \n    }\n\n    value, ok = ARGS[\"last_name\"]\n\t\n    if !ok { \n        log.Println(\"last_name is required\")\n        return \n    }\n    \n\tlastName , ok:= value.(string)\n    \n    if !ok { \n        log.Println(\"last_name must be a string\")\n        return \n    }\n\n\tvar preferred = \"\"\n\n\tif len(firstName) > 0 && len(lastName) > 0 {\n\n\t\tif (len(firstName)+len(lastName))%2 == 0 {\n\t\t\tpreferred = \"cats\"\n\t\t} else {\n\t\t\tpreferred = \"dogs\"\n\t\t}\n\t}\n    \n    result := map[string]string{ \"preferred\": preferred }\n    resultBytes, err := json.Marshal(result)\n    \n    if err != nil {\n        log.Println(\"failed to serialize result to json\")\n        return\n    }\n    \n    fmt.Println(string(resultBytes))\n}",
      "language": "go"
    },
    {
      "code": "// Create Enum for more readable code\n// Each Enum value will have a String raw value assigned\nenum PreferredAnimal : String {\n    case None = \"None\"\n    case Cat = \"Cat\"\n    case Dog = \"Dog\"\n}\n\n// Before we assign an animal to a name, we set preferred animal to .None - meaning it wasn't chosen yet\nvar preferredAnimal = PreferredAnimal.None\n\n// We check if firstName and lastName were passed in the payload. \n// If they are not, firstName and lastName are be equal to nil and check fails. \n// We also make sure passed values aren't just empty strings.\nif let firstName = ARGS[\"first_name\"] as! String? where !firstName.isEmpty, \n   let lastName = ARGS[\"last_name\"] as! String? where !lastName.isEmpty\n{\n    // We 'calculate' based on number of characters in name, \n    // if preferred animal is a cat or a dog\n    if (firstName.characters.count + lastName.characters.count) % 2 == 0 {\n        preferredAnimal = .Cat\n    } else {\n        preferredAnimal = .Dog\n    }\n}\n        \n// now we know, what's the preference!\n// lets print it as JSON\nprint(\"{\\\"preferred\\\":\\\"\\(preferredAnimal.rawValue)\\\"}\")",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
If you are running a Script directly, you can use the same syntax as shown in the section above. 

### Running Scripts
You can pass data to Script.run() - it will make an asynchronous call 
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\"payload\": {\"first_name\": \"Ryan\", \"last_name\": \"Gosling\"}}'\\\n\"https://api.syncano.io/v1.1/instances/instance_name/scripts/snippets/snippet_id/run/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Script \n\nsyncano.connect(api_key='ACCOUNT_KEY')\n\ntrace = Script.please.run(\n  instance_name=\"INSTANCE_NAME\",\n  id=\"SCRIPT_ID\",\n  payload={\n      'first_name': 'Ryan', \n      'last_name': 'Gosling'\n  })\n\n\n# Here, we store the id of the trace in a variable called trace_id.\n# It will be used if we want to check the results of the trace later.\ntrace_id = trace.id\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\"); // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Script = connection.Script;\n\nvar query = {\n  id: 7,\n\tinstanceName: \"INSTANCE_NAME\"\n}\n\nvar payload = {\n  payload: {\n    first_name: \"John\", \n    last_name: \"Snow\"\n  }\n}\n\nScript.please().run(query, payload).then(function(result) {\n\tconsole.log(\"result\", result);\n});",
      "language": "javascript"
    },
    {
      "code": "JsonObject params = new JsonObject();\nparams.addProperty(\"first_name\", \"Ryan\");\nparams.addProperty(\"last_name\", \"Gosling\");\n\nResponse<Trace> response = syncano.runScript(scriptId, params).send();\nTrace trace = response.getData();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[SCSnippet runSnippetWithId:@(SnippetID) params:@{\n  @\"first_name\":@\"John\",\n  @\"last_name\":@\"Snow\"}\n completion:^(SCTrace *trace, NSError *error) {\n   //handle error\n }];",
      "language": "objectivec"
    },
    {
      "code": "SCSnippet.runSnippetWithId(SnippetID, params: [\n            \"first_name\":\"John\",\n            \"last_name\":\"Snow\"]) \n{ trace, error in\n  //handle error\n}",
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
and give you an empty trace, that you can later check for data. 
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\":\"pending\",\n  \"links\":\t\t{\n    \"self\": \"/v1.1/instances/syncano/snippets/scripts/446/traces/183/\"\n  },\n  \"executed_at\":null,\n  \"result\":\"\",\n  \"duration\":null,\n  \"id\":183\n}",
      "language": "json"
    }
  ]
}
[/block]

Remember to store the "id" because you can use it later to check the results of the trace. These results should arrive after a very short time, usually 200ms. You can see below how we access the trace results with the trace id:
[block:code]
{
  "codes": [
    {
      "code": "curl \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/syncano/snippets/scripts/script_id/traces/trace_id/",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import ScriptTrace\n \nsyncano.connect(api_key='ACCOUNT_KEY')\n \n  trace = ScriptTrace.please.get(\n    instance_name='INSTANCE_NAME',\n    script_id='SCRIPT_ID',\n    id=trace_id\n  )\n\n  print trace.status\n  print trace.result\n# Remember that the \"id\" variable here is where you should put\n# the stored variable 'trace_id' from the previous example.",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\"); // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar ScriptTrace = connection.ScriptTrace;\n\nvar query = {\n  scriptId: 7,\n\tinstanceName: \"INSTANCE_NAME\"\n}\n\nScriptTrace.please().list(query).then(function(traces) {\n\tconsole.log(\"traces\", traces);\n});",
      "language": "javascript"
    },
    {
      "code": "trace.fetch();\n\nif (trace.getStatus() == TraceStatus.SUCCESS) {\n    trace.getOutput();\n}",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCTrace *trace = ...//obtain trace from Snippet execution\n[trace fetchWithCompletion:^(SCTrace *trace, NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "let trace = ...//obtain trace from Snippet execution\ntrace.fetchWithCompletion { trace, error in\n  //handle error\n}",
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
Example response:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"result\": {\n        \"stderr\": \"\",\n        \"stdout\": \"{\\\"preferred\\\": \\\"dogs\\\"}\"\n    },\n    \"executed_at\": \"2015-09-21T14:05:32.800795Z\",\n    \"duration\": 330,\n    \"id\": 18\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Scripts data dictionaries: ARGS, CONFIG and META"
}
[/block]
You can wonder: "Hm, these Script thing is pretty cool, but how do I pass data to it?"

In every Script Snippet there are three dictionaries with data:
1. [ARGS](#section-args-dictionary)
2. [CONFIG](#section-config-dictionary)
3. [META](#section-meta-dictionary)

### ARGS dictionary
ARGS is the data passed to the Script. You can think of this data as the arguments to a function. The way we access ARGS is different depending on how you use the Script. You will notice this in our examples below:

If you'll decide to pass data to a Script when running it directly through the Script `run` method (like in [Running Scripts](#section-running-scripts) section), ARGS dictionary can be accessed this way:

[block:code]
{
  "codes": [
    {
      "code": "first_name = ARGS['first_name']\nlast_name = ARGS['last_name']",
      "language": "python"
    },
    {
      "code": "first_name = ARGS.first_name\nlast_name = ARGS.last_name",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "first_name = ARGS[\"first_name\"]\nlast_name = ARGS[\"last_name\"]",
      "language": "ruby"
    },
    {
      "code": "first_name = ARGS[\"first_name\"]\nlast_name = ARGS[\"last_name\"]",
      "language": "go"
    },
    {
      "code": "let firstName = ARGS[\"first_name\"]\nlet lastName = ARGS[\"last_name\"]",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
If you are using the Script from a **Script Endpoint** (you can read more on Script Endpoints [here](doc:endpoints-scripts)), the data in ARGS must be accessed differently in the Script. This is how you can access the data from ARGS if you use a Script Endpoint and you pass the arguments inside of a POST body. 
[block:code]
{
  "codes": [
    {
      "code": "first_name = ARGS['POST']['first_name']\nlast_name = ARGS['POST']['last_name']",
      "language": "python"
    },
    {
      "code": "first_name = ARGS.POST.first_name\nlast_name = ARGS.POST.last_name",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "first_name = ARGS[\"POST\"][\"first_name\"]\nlast_name = ARGS[\"POST\"][\"last_name\"]",
      "language": "ruby"
    },
    {
      "code": "first_name = ARGS[\"POST\"][\"first_name\"]\nlast_name = ARGS[\"POST\"][\"last_name\"]",
      "language": "go"
    },
    {
      "code": "let POST = ARGS[\"POST\"] as? [String: AnyObject]\nlet firstName = POST?[\"first_name\"]\nlet lastName = POST?[\"last_name\"]",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
If you want to pass arguments in the URL (make a HTTP GET Request), you can access the ARGS data like this
[block:code]
{
  "codes": [
    {
      "code": "first_name = ARGS['GET']['first_name']\nlast_name = ARGS['GET']['last_name']",
      "language": "python"
    },
    {
      "code": "first_name = ARGS.GET.first_name\nlast_name = ARGS.GET.last_name",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "first_name = ARGS[\"GET\"][\"first_name\"]\nlast_name = ARGS[\"GET\"][\"last_name\"]",
      "language": "ruby"
    },
    {
      "code": "first_name = ARGS[\"GET\"][\"first_name\"]\nlast_name = ARGS[\"GET\"][\"last_name\"]",
      "language": "go"
    },
    {
      "code": "let GET = ARGS[\"GET\"] as? [String: AnyObject]\nlet firstName = GET?[\"first_name\"]\nlet lastName = GET?[\"last_name\"]",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
### CONFIG dictionary

The configuration specified in the Script config property. Here, you can store passwords, api keys, and other important data. It's not necessary to use it, but it can help you manage your code and make it easier to read. Here is an example CONFIG dictionary:

[block:code]
{
  "codes": [
    {
      "code": " {\n \t\"password\": \"my_password\",\n  \"email\": \"duke.nukem@gmail.com\n }",
      "language": "json"
    }
  ]
}
[/block]
And here is an example of a Script that uses CONFIG dictionary to connect to Syncano:
[block:code]
{
  "codes": [
    {
      "code": "import syncano\n\nconnection = syncano.connect(\n\temail=CONFIG['email'],\n\tpassword=CONFIG[\"password\"]\n)\n\t\nprint(list(connection.instances.list()))",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano');\nvar account = new Syncano({accountKey : \"ACCOUNT_KEY\"});  \n\naccount.instance().list(function(err, res) {\n  if (err) {\n      console.log(err); return;\n  }\n  console.log(res);\n});",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "require 'syncano'\n\nconnection = Syncano.connect(\n  email: CONFIG['email'],\n\tpassword: CONFIG['password'])\n\t\np connection.instances.all",
      "language": "ruby"
    },
    {
      "code": "//Coming Soon!",
      "language": "text",
      "name": "Go"
    },
    {
      "code": "// Syncano librry is not yet available in Swift Snippet, but you can still access CONFIG dictionary\nlet email = CONFIG[\"email\"]\nlet password = CONFIG[\"password\"]",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
It will list instances that are available in this account:
[block:code]
{
  "codes": [
    {
      "code": "[<Instance: syncano>, <Instance: brunhilda>, <Instance: tabaluga>]",
      "language": "python"
    },
    {
      "code": "{ prev: null,\n  objects: \n   [ { name: 'icy-forest-9448',\n       links: [Object],\n       created_at: '2015-09-18T11:46:04.773437Z',\n       updated_at: '2015-09-18T11:46:04.778263Z',\n       role: 'full',\n       owner: [Object],\n       metadata: [Object],\n       description: '' },\n     { name: 'hidden-tree-7627',\n       links: [Object],\n       created_at: '2015-09-27T19:27:02.160103Z',\n       updated_at: '2015-09-27T19:27:02.164393Z',\n       role: 'full',\n       owner: [Object],\n       metadata: [Object],\n       description: '' },\n     { name: 'blue-tree-2284',\n       links: [Object],\n       created_at: '2015-10-05T07:23:15.885570Z',\n       updated_at: '2015-10-05T07:23:15.889781Z',\n       role: 'full',\n       owner: [Object],\n       metadata: [Object],\n       description: '' },\n     { name: 'sparkling-sunset-9842',\n       links: [Object],\n       created_at: '2015-10-05T07:45:30.684964Z',\n       updated_at: '2015-10-05T07:45:30.691388Z',\n       role: 'full',\n       owner: [Object],\n       metadata: [Object],\n       description: '' } ],\n  next: null }",
      "language": "json",
      "name": "Node.js"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    },
    {
      "code": "// Coming Soon!",
      "language": "go"
    },
    {
      "code": "// Syncano librry is not yet available in Swift Snippet",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
You can set/update CONFIG using our API, but the best way to access it, is through our Dashboard:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/c31tlmXiQ6i36nfKtnbe",
        "Config_01.png",
        "1279",
        "654",
        "#26426d",
        ""
      ]
    }
  ]
}
[/block]
### META Dictionary
META dictionary is the metadata specific to the execution of the Script: information if the Script was run by [Script Endpoint](doc:endpoints-scripts), Schedule or Trigger Socket and other information such as request info for Script Endpoint. This is an example META dictionary:

[block:code]
{
  "codes": [
    {
      "code": "{\n  'instance': 'red-paper-5274', \n  'executed_by': 'webhook',\n  // 'request' key is only present if Script was executed by a Script Endpoint\n  'request': {'Request details here. Removed from example for brevity'},\n  'executor': 1\n}",
      "language": "json"
    }
  ]
}
[/block]
### Script Timeouts and Custom Timeout

Scripts have a timeout setting built-in. It's default value is currently set to 300 seconds for direct runs. Timeouts for running Scripts via Script Endpoint, Schedule, or Trigger Sockets are different and can be found [here](limits#snippet-scripts). When the Script reaches the timeout limit, its code execution stops and it's status is marked as `timeout` on the Scripts traces list:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Lh5o5SfeSTOlTLpBLA7C",
        "Snippet_timeout.png",
        "752",
        "60",
        "#e85958",
        ""
      ]
    }
  ]
}
[/block]
On top of that, you can add your own custom timeout setting, to override the default one. 
The condition here is that the custom timeout cannot exceed the default one -- so it has to be smaller than 300s.
[block:callout]
{
  "type": "warning",
  "body": "Custom timeout affects only direct Script runs. \nScript run via Script Endpoint, Schedule or Trigger Sockets will keep their original timeout.",
  "title": "Timeout setting for Script Endpoints, Schedules or Triggers Sockets"
}
[/block]
To set a custom Script timeout:
1. Go to Script Config tab
2. Add a "timeout" Key
3. Add a Value in 1 - 299 second range 
4. Select "Integer" as Value Type
5. Click "+" button to add this field to Script Config
6. Click "Save" button to save the Script Config
 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/RWg70Lh8RYaEeJdAhO9x",
        "Timeout_01.png",
        "1283",
        "604",
        "#c5185f",
        ""
      ]
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Handling files in Scripts"
}
[/block]
Script Snippets allow for operations on files too. Each script is a separate environment and during script run you'll have access to the filesystem operations. The data on filesystem is temporary and is accessible only during the script execution. If you'd like to use the data in the future the best approach would be to save it as Data Object once you are done processing it. We will go through a common use case of processing a pdf file to show how it works. In the example below a script will:
1. Get a file from a url
2. Write the file to a temporary directory
3. Zip the file
4. Save the file as a Data Object
[block:callout]
{
  "type": "info",
  "body": "Currently there's no file size limit for storage during Script execution."
}
[/block]
Prerequisites:
For this example to work, you'll need:
1. An [Account Key](doc:authentication) 
2. A [Class](doc:classes) called `zip` with the following schema:

[block:code]
{
  "codes": [
    {
      "code": "[\n  {\n    \"name\": \"file\",\n    \"type\": \"file\"\n  },\n  {\n    \"name\": \"file_url\",\n    \"type\": \"string\"\n  }\n]",
      "language": "json"
    }
  ]
}
[/block]
3. You'll also need to pass the following payload when executing the Script:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"file_url\": \"a url to pdf file\"\n}",
      "language": "json"
    }
  ]
}
[/block]
### The file handling example
[block:code]
{
  "codes": [
    {
      "code": "from syncano.models import Object\nimport syncano\nimport requests\nimport zipfile\nimport tempfile\nimport os\n\nsyncano.connect(api_key=\"ACCOUNT_KEY\")\n\nfile_url = ARGS.get('file_url', None)\nif file_url is None:\n    raise ValueError(\"You didn't pass file_url to your CodeBox Execution\")\n\n# get pdf file from url\nfile = requests.get(file_url)\n\n# set up temporary directory and save the pdf file in it\ntemp_directory = tempfile.mkdtemp()\npdf_file_name = temp_directory + \"/pdf_file.pdf\"\nf = open(pdf_file_name, \"wb\")\nf.write(file.content)\nf.close()\n\n# zip the file we placed in the temp directory\nzipped_path = temp_directory + \"/pdf_zipped.zip\"\nwith zipfile.ZipFile(zipped_path, \"w\") as zf:\n    zf.write(pdf_file_name, os.path.basename(pdf_file_name))\n\n# Save zipped file to Syncano\nfile = open(zipped_path, \"r\")\nObject.please.create(instance_name=META[\"instance\"],\n                     class_name=\"zip\",\n                     file=file,\n                     file_url=file_url)\nfile.close()",
      "language": "python"
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
Now you know how to create and run Scripts. Next step is learning more about running them by using [Script Endpoints](doc:endpoints-scripts), [Schedule](doc:schedules) and [Trigger](doc:triggers) Sockets.

Alternatively you can read about another type of code that can be stored on Syncano which is called Template Snippets. They allow for creation of, for instance, html views out of any of the Syncano endpoints. You can read more about templates [here](http://docs.syncano.io/v1.1/docs/snippets-templates).