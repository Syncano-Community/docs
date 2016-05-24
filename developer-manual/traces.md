---
title: "Traces"
excerpt: "Aka how do I know that Snippet Script was executed? In this Chapter you'll learn what Traces are and how to get Traces for Scripts, Schedule, Triggers and Script Endpoints runs."
---
## Chapter Contents:
1. [Overview](#overview)
2. [Scripts, Schedule and Trigger Sockets Traces](#script-schedule-and-trigger-sockets-traces)
3. [Script Endpoint Traces](#script-endpoint-traces)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
What Traces are essentially, is a history of Snippet Script execution. They log information about the Script execution status, its execution duration, time, etc. Since Scripts can be run directly, through Schedule, Trigger Sockets and Script Endpoints, the Traces are grouped based on a method of execution. Script Endpoint Traces are described in a separate section of this chapter because their properties differ from Script, Trigger and Schedule Sockets Traces.

[block:callout]
{
  "type": "warning",
  "body": "Traces aren't meant to be persistent. Syncano will store a **100** of latest Script runs . If you want to store Script results, the best way is to create Data Objects and store traces in them. It can be done easily because python and nodejs runtimes have the `syncano` library inside and you can just import and start using it straight away.",
  "title": "Important!"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Script, Schedule and Trigger Sockets Traces"
}
[/block]
An example of how Syncano represents Snippet Script (as well as Schedules and Triggers) Traces in the API:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 184, \n    \"status\": \"success\", \n    \"executed_at\": \"2015-03-18T08:52:39.667359Z\", \n    \"duration\": 233, \n    \"result\": \"some_result\", \n    \"links\": {\n        \"self\": \"/v1.1/instances/<instance>/snippets/scripts/666/traces/777/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
You can also find same information in the [Dashboard](https://dashboard.syncano.io). Go to the Scripts view, select the Script you want to check Traces of, and switch to the Traces view.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/cZhKzDLSB61E39RCrYcl",
        "Traces_01.png",
        "1439",
        "728",
        "#254372",
        ""
      ]
    }
  ]
}
[/block]
By default you see only execution status, Trace ID, duration time and execution date. If you want to see Snippet Script result as well, click on selected Trace - it will expand, showing more details.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/hzQGcXpdTUeaQg6BqufV",
        "Traces_02.png",
        "1439",
        "731",
        "#4a4942",
        ""
      ]
    }
  ]
}
[/block]
This is the information that Traces hold:
[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Description",
    "0-0": "`status`",
    "1-0": "`executed_at`",
    "2-0": "`duration`",
    "3-0": "`result`",
    "0-1": "Status of the connected Snippet Script. This field can have `success`, `failure` or `timeout` values.",
    "1-1": "Time when connected Script was executed (ISO format)",
    "2-1": "Duration of Snippet Script run (in ms)",
    "3-1": "Snippet Script result represented as a raw string"
  },
  "cols": 2,
  "rows": 4
}
[/block]
You can easily see traces in the Dashboard as well
[block:api-header]
{
  "type": "basic",
  "title": "Script Endpoint Traces"
}
[/block]
As mentioned earlier, Script Endpoint Traces are a bit different from Traces found in other parts of Syncano Platform. They have two additional `meta` and `args` properties. Let's see how a Script Endpoint Trace looks like:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 2039, \n    \"status\": \"success\", \n    \"executed_at\": \"2015-04-28T13:13:23.970574Z\", \n    \"duration\": 257, \n    \"result\": \"ello\", \n    \"meta\": {\n        \"HTTP_ACCEPT\": \"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8\", \n        \"HTTP_USER_AGENT\": \"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36\", \n        \"HTTP_DNT\": \"1\", \n        \"REMOTE_ADDR\": \"91.202.125.2\", \n        \"HTTP_ACCEPT_LANGUAGE\": \"en-US,en;q=0.8\", \n        \"REQUEST_METHOD\": \"GET\", \n        \"HTTP_HOST\": \"api.syncano.rocks\", \n        \"PATH_INFO\": \"/v1.1/instances/<instance>/snippets/scripts/run/\", \n        \"HTTP_CACHE_CONTROL\": \"max-age=0\", \n        \"HTTP_ACCEPT_ENCODING\": \"gzip, deflate, sdch\"\n    }, \n    \"args\": {}, \n    \"links\": {\n        \"self\": \"/v1.1/instances/<instance>/snippets/scripts/<script_id>/traces/<trace_id>/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Here are the Script Endpoint Trace properties:
[block:parameters]
{
  "data": {
    "0-0": "`meta`",
    "2-1": "Status of the connected Script. This field can have success, failure or timeout values.",
    "2-0": "`status`",
    "3-0": "`executed_at`",
    "3-1": "Time when connected Script was executed (ISO format)",
    "4-0": "`duration`",
    "4-1": "Duration of Script run (in ms)",
    "5-0": "`result`",
    "5-1": "Script result represented as a raw string",
    "h-0": "Property",
    "h-1": "Description",
    "1-0": "`args`",
    "0-1": "This field contains HTTP Request Header information from a client that made a HTTP request using this specific Script Endpoint.",
    "1-1": "Payload that was passed to the Script."
  },
  "cols": 2,
  "rows": 6
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For example API calls with traces - visit the following pages:\n\n**Snippet Scripts** - [Trace List](http://docs.syncano.com/v0.1.1/docs/script-list-traces), [Trace Detail](http://docs.syncano.com/v0.1.1/docs/script-traces-details)\n**Schedule Sockets** - [Trace List](http://docs.syncano.com/v0.1.1/docs/schedules-traces-list), [Trace Detail](http://docs.syncano.com/v0.1.1/docs/schedules-traces-details)\n**Trigger Sockets** - [Trace List](http://docs.syncano.com/v0.1.1/docs/triggers-traces-list), [Trace Detail](http://docs.syncano.com/v0.1.1/docs/triggers-traces-details)\n**Script Endpoints** - [Trace List](http://docs.syncano.com/v0.1.1/docs/endpoints-scripts-traces-list), [Trace Detail](http://docs.syncano.com/v0.1.1/docs/endpoints-scripts-traces-details)"
}
[/block]
Next chapter is [Request Batching](doc:request-batching) - learn how to send more than 1 request at a time to Syncano.