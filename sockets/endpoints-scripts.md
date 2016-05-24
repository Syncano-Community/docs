---
title: "Script Endpoints"
excerpt: "After reading this chapter, you'll know how to create a Script Endpoint and how to run a Script via Script Endpoint"
---
## Chapter Contents:
1. [Overview](#overview)
2. [Script Endpoint properties](#script-endpoint-properties)
3. [Example](#example)
4. [Public Script Endpoints](#public-script-endpoints)
5. [Custom response format for Script Endpoints](#custom-response-format-for-script-endpoints)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
As mentioned in the [Snippets - Scripts](doc:snippets-scripts#running-script) chapter, there are several methods of running a Snippet. Apart from running it directly, or by Schedule or Trigger Sockets, you can create a Script Endpoint. 

Idea behind Script Endpoints is pretty simple:
1. You create a Script Snippet.
2. You want the Script to be run based on some external event.
3. You create a Script Endpoint which essentially is an URL.
4. When Script Endpoint URL is hit, it triggers a running the Script that it was connected to. 
5. Event from step 2 causes the website to make a HTTP request to the URL defined earlier 

So a Script Socket is simply a URL that a website or application can make a HTTP request to. In the case of Syncano, this HTTP request to the URL will cause a Script execution. 
[block:callout]
{
  "type": "info",
  "title": "Further reading",
  "body": "If you would like to learn more about webhooks idea in general, we recommend this [Wiki](https://webhooks.pbworks.com/w/page/13385124/FrontPage)"
}
[/block]
You can find list of your Script Endpoints, under `Sockets` tab in our [Dashboard](https://dashboard.syncano.io).

If a Script Endpoint is [public](#public-script-endpoints), you can see a clip icon in the `Public` column. You can click it to copy the link. 
[block:api-header]
{
  "type": "basic",
  "title": "Script Endpoint properties"
}
[/block]
This is how an example Script Endpoint object stored on Syncano looks:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"random-potato\", \n  \"script\": 818, \n  \"public_link\": \"7265e3724eaa5f3f1aa2faa0414f2bc95bf48a21\", \n  \"public\": true, \n  \"links\": {\n    \"reset-link\": \"/v1.1/instances/syncano/endpoints/scripts/random-potato/reset_link/\", \n    \"script\": \"/v1.1/instances/syncano/endpoints/scripts/818/\", \n    \"self\": \"/v1.1/instances/syncano/endpoints/scripts/random-potato/\", \n    \"run\": \"/v1.1/instances/syncano/endpoints/scripts/random-potato/run/\", \n    \"public-link\": \"/v1.1/instances/syncano/sockets/endpoints/scripts/p/7265e3724eaa5f3f1aa2faa0414f2bc95bf48a21/\"\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
And here is a rundown of Script Endpoint properties:
[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`name`",
    "1-0": "`script`",
    "2-0": "`public_link`",
    "3-0": "`public`",
    "0-1": "Name of the Script Endpoint and part of the url that makes this endpoint",
    "1-1": "Snippet `id` that Script Endpoint will run.",
    "2-1": "Hash value of the public link. It's used to create a Script Endpoint that can be accessed without an authentication. This public Script Endpoint is later available at `https://api.syncano.io/v1/<instance_name>/ednpoints/scripts/p/<public_link>/` endpoint.",
    "3-1": "Flag that determines whether the Script Endpoint public link is active. You can read more about public Script Endpoints in [this section](#public-script-endpoints) below."
  },
  "cols": 2,
  "rows": 4
}
[/block]
After running a Script Endpoint you'll receive a result object, that will contain the details of Script execution. Result object will have following properties:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"duration\": 274,\n    \"result\": \"{\\\"stderror\\\": \\\"\\\", \\\"stdout\\\": \\\"Hello!\\\"}\",\n    \"executed_at\": \"2015-06-02T12:55:06.625847Z\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`status`",
    "0-1": "Status of Script execution. Possible results are: `success` or `failure`",
    "1-0": "`duration`",
    "1-1": "Script execution time in miliseconds",
    "2-0": "`result`",
    "2-1": "This value will contain the results of functions executed within the Snippet. It has two parameters:\n`stdout`: Standard output. It will hold a result of a successful Snippet execution\n`srderr`: Standard error. It will hold error tracebacks, if such errors occur during Snippet execution.",
    "3-0": "`executed_at`",
    "3-1": "Execution timestamp in ISO 8601 date time format"
  },
  "cols": 2,
  "rows": 4
}
[/block]
Ok, but you'd probably like to know how to actually use Script Endpoint, so here is the...
[block:api-header]
{
  "type": "basic",
  "title": "Example"
}
[/block]
Let's say we would like to create a pseudo-random potato image generator (because of reasons). We we'll do it in 3 easy steps:
1. [Create Snippet to get random image](#section-step-1-create-script-to-get-random-image)
2. [Create a Script Endpoint that will run that Script](#section-step-2-create-a-script-endpoint-that-will-run-that-script)
3. [Run a Script Endpoint with payload (ARGS)](#section-step-3-run-a-script-endpoint-with-payload-args-)

## Step 1 - Create Script to get random image
Create a new Snippet and paste this code into it. The script will make a google search for potato images and return a url of randomly chosen potato image:
[block:code]
{
  "codes": [
    {
      "code": "import random\nimport re\nimport requests\n\nurl = 'https://www.google.com/search?q=potato&tbm=isch'\n\ndef random_potato():\n    request = requests.get(url)\n    potatoes = re.findall('src=\"(http[^\"]+)\"', request.text)\n    return random.choice(potatoes)\n\nprint random_potato()",
      "language": "python"
    },
    {
      "code": "var request = require('request');\n\nvar url = 'https://www.google.com/search?q=potato&tbm=isch'\n\nrequest(url, function (error, response) {\n  if (!error && response.statusCode == 200) {\n    var regex = /src=\"(https?:\\/\\/[^\\s]+)\"/g;\n    var re;\n    var result = [];\n    while ((re = regex.exec(response.body)) !== null) {\n      result.push(re[1]);\n    }\n    \n    console.log(result[Math.floor(Math.random()*result.length)]);\n  }\n});",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "# Coming soon!",
      "language": "ruby"
    },
    {
      "code": "// Courtesy of Bryan Smith. \n\nimport( \n\"fmt\" \n\"io/ioutil\" \n\"net/http\" \n\"math/rand\" \n\"regexp\" \n\"time\" \n)\n\nfunc main(){ \nurl := \"https://www.google.com/search?q=potato&tbm=isch\" \n\nresp, _ := http.Get(url) \ndefer resp.Body.Close() \n\nbody, _ := ioutil.ReadAll(resp.Body) \n\nre := regexp.MustCompile(\"src=\\\"(http[^\\\"]+)\\\"\") \nmatches := re.FindAllStringSubmatch(string(body), -1) \n\npotatoes := make([]string, len(matches)) \n\n//retrieve only the captured url from the matches not the full src='url' \nfor index, match := range matches { \npotatoes[index] = match[1] \n} \n\n//seed with nanoseconds to get make sure unique random number returned \nrand.Seed(time.Now().UnixNano()) \n\n//get random image url and print to stdout \nfmt.Println(potatoes[rand.Intn(len(potatoes))]) \n}",
      "language": "go",
      "name": "Go"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "For details on adding a Script using the Dashboard, please follow the guide in [Snippet](doc:snippets-scripts#adding-a-new-script) chapter of this Developer Manual."
}
[/block]
## Step 2 - Create a Script Endpoint that will run that Script
Next step would be to add a Script Endpoint that will run the above Script.

To a add a new Script, you need to: 
* name it - naming rules are the same as for the [URL slug](https://en.wikipedia.org/wiki/Semantic_URL#Slug) - use only letters and number, no whitespaces - replace them with hyphens or underscored (e.g. use `getpotato`, `get_potato`, or `get-potato` instead of `get potato`)
* choose a `Script` - it will be run every time Script Endpoint is called
* decide if Script Endpoint should be public - read more on public Script Endpoints [here](#public-script-endpoints) 
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/lCjZJs1SauF3o5fyyxIN",
        "Add_script_enpoint_00.png",
        "1493",
        "765",
        "#224270",
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
        "https://www.filepicker.io/api/file/IGVrzFciSZeJoq3F3TCb",
        "Add_script_enpoint_01.png",
        "1279",
        "655",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
Great! We now have a Script Endpoint that will run our random potato image generator. Let's run it to test if everything works:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/endpoints/scripts/SCRIPT_ENDPOINT/run/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import ScriptEndpoint\n\nscript_endpoint = ScriptEndpoint.please.get(name='SCRIPT_ENDPOINT_NAME')\nrun_result = script_endpoint.run()\n\nif run_result.status == 'success':\n    result = run_result.result['stdout']\n    # result is 'https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcTQ2NETFbvCEfE6EjwC9E0N3Ti6chOvLWQLKGjVh78drtrIy965RPsHDlw'\nelse:\n    error = run_result.result['stderr']\n    \n# if you used set_response in Script code just use run_result (that's the custom # defined response) - without 'stdout' or 'stderr'    \n",
      "language": "python"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar account = new Syncano({accountKey: \"ACCOUNT_KEY\"});\n\naccount.instance('INSTANCE_NAME').webhook('WEBHOOK_NAME').run(callback());",
      "language": "javascript"
    },
    {
      "code": "ScriptEndpoint endpoint = new ScriptEndpoint(\"endpoint_name\");\nendpoint.run();\nString output = endpoint.getOutput();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[SCWebhook runWebhookWithName:webhookName completion:^(SCWebhookResponseObject *responseObject, NSError *error) {\n  //handle error\n}];",
      "language": "objectivec"
    },
    {
      "code": "SCWebhook.runWebhookWithName(name) { trace, error in\n  //handle error\n}",
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
And there you go. A nice fresh random potato image!
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/umKq3qeQgeePygwaGdL9",
        "potato-heart.jpg",
        "560",
        "335",
        "#c8905d",
        ""
      ]
    }
  ]
}
[/block]
## Step 3 - Run a Script Endpoint with payload (ARGS)

Ok, so we have our random potato image generator but what if we wanted to grant our users the ability to **choose** what kind of a potato they would like to search for (round, black, sexy). That's exactly (well, maybe not exactly) what custom payloads were designed for!

Let's try to add an option to pass these extra arguments to the random potato image generator Script:
[block:code]
{
  "codes": [
    {
      "code": "import random, re, requests\n\n# handling different cases of passing payload to Snippet\npayload = ''\nif 'GET' in ARGS or 'POST' in ARGS: \n    if 'user_search' in ARGS['GET']:\n        payload = ARGS['GET']['user_search']\n    else:\n        payload = ARGS['POST']['user_search']\n\n# constructing the query url\nurl = 'https://www.google.com/search?q=' + payload + '+potato&tbm=isch'\n\n# function that extracts the potato image urls\ndef random_potato():\n    request = requests.get(url)\n    potatoes = re.findall('src=\"(http[^\"]+)\"', request.text)\n    return random.choice(potatoes)\n\nprint random_potato()",
      "language": "python"
    },
    {
      "code": "var request = require('request');\n\n// handling different cases of passing payload to CodeBox\nvar payload = '';\nif ('GET' in ARGS || 'POST' in ARGS) {\n  if ('user_search' in ARGS.GET) {\n      var payload = ARGS.GET.user_search;\n  } else {\n      var payload = ARGS.POST.user_search;\n  }\n}\n\nvar url = 'https://www.google.com/search?q=' + payload + '+potato&tbm=isch'\n\nrequest(url, function (error, response) {\n  if (!error && response.statusCode == 200) {\n    var regex = /src=\"(https?:\\/\\/[^\\s]+)\"/g;\n    var re;\n    var result = [];\n    while ((re = regex.exec(response.body)) !== null) {\n      result.push(re[1]);\n    }\n    \n    console.log(result[Math.floor(Math.random()*result.length)]);\n  }\n});",
      "language": "javascript",
      "name": "Node.js"
    }
  ]
}
[/block]
We have added an extra variable called `payload`. The ARGS are the arguments that were passed along with the Script Endpoint call. Since Script Endpoints can be run with both `GET` and `POST` HTTP requests, we made an `if` statement to handle both use cases.
[block:callout]
{
  "type": "info",
  "body": "More info about ARGS can be found in the [Scripts chapter](doc:snippets-scripts#section-args-dictionary)."
}
[/block]
Now, let's run the Script Endpoint with custom user payload: 
[block:code]
{
  "codes": [
    {
      "code": "# running a Script Endpoint with POST HTTP request\ncurl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\"user_search\":\"dancing\"}' \\\n\"https://api.syncano.io/v1/instances/INSTANCE_NAME/endpoints/scripts/run_the_potato/run/\"\n\n# running a Script Endpoint with GET HTTP request\ncurl -X GET \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n\"https://api.syncano.io/v1/instances/INSTANCE_NAME/endpoints/scripts/run_the_potato/run/?user_search=dancing\"",
      "language": "curl"
    },
    {
      "code": "var Syncano = require('syncano'); //CommonJS\nvar account = new Syncano({accountKey: \"ACCOUNT_KEY\"});\n\naccount.instance('INSTANCE_NAME').webhook('WEBHOOK_NAME').run({\"user_search\":\"dancing\"})\n    .then(function(res){\n  \t\t\t// res.result.stdout contains the image link\n        console.log(res);\n    })\n    .catch(function(err){\n        console.log(err);\n    });",
      "language": "javascript"
    },
    {
      "code": "import syncano\nfrom syncano.models import ScriptEndpoint\n\nscript_endpoint = ScriptEndpoint.please.get(name='SCRIPT_ENDPOINT_NAME')\nrun_result = script_endpoint.run(user_search='PAYLOAD')\n\nif run_result.status == 'success':\n    result = run_result.result['stdout']\nelse:\n    error = run_result.result['stderr']\n\n# if you used set_response in Script code just use run_result (that's the custom # defined response) - without 'stdout' or 'stderr'",
      "language": "python",
      "name": "Python"
    }
  ]
}
[/block]
Result should be similar to this one and should hold a url to a new potato image:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"duration\": 830,\n    \"executed_at\": \"2015-09-21T14:05:32.800795Z\",\n    \"id\": 18,\n    \"result\": {\n        \"stderr\": \"\",\n        \"stdout\": \"https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcTuivYbh5Wpk0voSvXjRN22wphFUyzWZo9eZEkbfbq9xxBUw5GfSdRzUGU\"\n    },\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:embed]
{
  "html": false,
  "url": "http://i.imgur.com/JKTNN8G.gif",
  "title": null,
  "favicon": "http://i.imgur.com/favicon.ico",
  "image": "http://imgur.com/JKTNN8G.gif",
  "iframe": true,
  "width": "50%",
  "height": "444px"
}
[/block]
Great work! Now let's see how to create and run public Script Endpoints.
[block:api-header]
{
  "type": "basic",
  "title": "Public Script Endpoints"
}
[/block]
If while creating a Script Endpoint you set the `public` property to `true`, it can now be run by anyone with access to its public url. The simplest way would be to just paste the link in the internet browser window of your choice and see the response there!
[block:callout]
{
  "type": "info",
  "body": "You can copy the public url from [Syncano Dashboard](https://dashboard.syncano.io/) - Instance - Script Endpoints section."
}
[/block]
Assuming that your public link is `44cfc5552eacc599400eae80f2ecaf4ea9814091` you could also run it as following (you can pass extra arguments to a Snippet in the URL):
[block:code]
{
  "codes": [
    {
      "code": "curl -X GET \"https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/?message=Hello\"",
      "language": "curl"
    },
    {
      "code": "import requests\n\nrequest = requests.get('https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/?message=Hello')\n\nresult = json.loads(json.loads(request.text)['result'])['stdout']\n\n# you can obtain publick link this way:\nimport syncano\nfrom syncano.models import ScriptEndpoint\n\nscript_endpoint = ScriptEndpoint.please.get(name='SCRIPT_ENDPOINT_NAME')\nresult = requests.get('https://api.syncano.io/v1.1/' + script_endpoint.links.public_link)\n\nresult = result.json()['result']['stdout']",
      "language": "python"
    },
    {
      "code": "//Client Side - jQuery\nvar url = \"https://api.syncano.io/v1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/?message=Hello\";\n\n$.get(url, function(data) {\n  console.log(data)\n});\n\n//Server Side - request\nvar request = require('request');\n\nvar url = \"https://api.syncano.io/v1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/?message=Hello\";\n\nrequest(url, function (err, res) {\n  if (err) {console.log(err); return;}\n  console.log(res.body)\n});",
      "language": "javascript"
    },
    {
      "code": "// url is a full url\nResponse<Trace> response = syncano.runScriptEndpointUrl(url).send();\nTrace trace = response.getData();",
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

If you wanted to use POST, you can pass extra arguments for the Script as a JSON body:
[block:code]
{
  "codes": [
    {
      "code": "// this time a POST instead of GET\ncurl -X POST \\\n-H \"Content-Type: application/json\" \\\n-d '{\"message\":\"Hello World!\"}' \\\n\"https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/\" ",
      "language": "curl"
    },
    {
      "code": "import requests\n\npostPayload = {\"message\":\"Hello World!”}\nrequest = requests.post('https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/', data=postPayload)",
      "language": "python"
    },
    {
      "code": "//Client Side - jQuery\nvar url = \"https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/;\n\n$.post(url,{\"message\":\"Hello World!\"}, function(data) {\n  console.log(data)\n});\n\n//Server Side - request\nvar request = require('request');\n\nvar url = \"https://api.syncano.io/v1.1/instances/<instance_name>/endpoints/scripts/p/44cfc5552eacc599400eae80f2ecaf4ea9814091/;\n\nrequest.post(url,{json:{\"message\":\"Hello World!\"}}, function (err, res) {\n  if (err) {console.log(err); return;}\n  console.log(res.body)\n});",
      "language": "javascript"
    },
    {
      "code": "JsonObject params = new JsonObject();\nparams.addProperty(\"message\", \"Hello World!\");\n// url is a full url\nResponse<Trace> response = syncano.runScriptEndpointUrl(url, params).send();\nTrace trace = response.getData();",
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
Depending on how you passed the arguments, you would access them differently inside the ARGS dictionary. You can find more info about ARGS in the [Scripts chapter](doc:snippets-scripts#section-args-dictionary).
[block:api-header]
{
  "type": "basic",
  "title": "Custom response format for Script Endpoints"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Currently this feature is supported in Python and NodeJS Script environments. \nIf you'd like to see it in other Script types, let us know at support@syncano.com and we will let you know what we can do about it."
}
[/block]
As you've seen above, the standard response format for a Script Endpoint is:
* 200 OK HTTP status code 
* and a json body containing the `duration`, `id`, `status`, `result` and `executed_at` keys:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"duration\": 504,\n    \"executed_at\": \"2016-01-29T11:33:05.790846Z\",\n    \"id\": 24,\n    \"result\": {\n        \"stderr\": \"\",\n        \"stdout\": \"foo\"\n    },\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
This response should handle most of the use cases, but some external service might require a custom response format (e.g. an array or an empty body). 

That's why we are giving you a once-in-a-life-time opportunity, to create your very own Custom Script Endpoint Response!

To set up a custom response format, go to the Script that the Script Endpoint is supposed to run and add one line of code:
[block:code]
{
  "codes": [
    {
      "code": "set_response(HttpResponse(status_code=201, content='super uber custom content', content_type='text/plain'))",
      "language": "python"
    },
    {
      "code": "setResponse(new HttpResponse(201, 'super uber custom content', 'text/plain'));",
      "language": "javascript",
      "name": "NodeJS"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "The \"set response\" method can be called multiple times within a single Script. Only the last call will affect the response."
}
[/block]
As you can see there are three parameters that you can override:
[block:parameters]
{
  "data": {
    "0-0": "`status_code`",
    "1-0": "`content`",
    "2-0": "`content_type`",
    "h-0": "Parameter",
    "h-1": "Required",
    "h-2": "Default Value",
    "0-2": "200",
    "1-2": "`(no content)`",
    "2-2": "`application/json`",
    "0-1": "No",
    "1-1": "No",
    "2-1": "No",
    "h-3": "Description",
    "0-3": "The status code of the custom HTTP response",
    "1-3": "The custom response content",
    "2-3": "The custom response content type"
  },
  "cols": 4,
  "rows": 3
}
[/block]
We will now run this Script Endpoint from a terminal using cURL (you can also test it in your terminal or internet browser, because the Script Endpoint I used is public):
[block:code]
{
  "codes": [
    {
      "code": "curl -v https://api.syncano.io/v1.1/instances/icy-haze-4653/endpoints/scripts/p/bea34e12ac6485fa9472abc3a5d50cd96fe84b35/custom/",
      "language": "curl"
    }
  ]
}
[/block]
And the result:
[block:code]
{
  "codes": [
    {
      "code": " HTTP/1.1 201 Created\n Cache-Control: no-cache\n Content-Type: text/plain; charset=utf-8\n Date: Fri, 29 Jan 2016 13:24:01 GMT\n Server: nginx/1.8.0\n Content-Length: 50\n Connection: keep-alive\n\n* Connection #0 to host api.syncano.io left intact\nsuper uber custom content%",
      "language": "http",
      "name": "Custom response"
    }
  ]
}
[/block]
As you can see, both the HTTP headers and the response body were modified.
[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
That is all you need to know to be able to run Script via Script Endpoint. The next recommended read is [Schedule Sockets](schedules) chapter.