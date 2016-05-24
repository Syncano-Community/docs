---
title: "Limits"
excerpt: "Limits on Syncano"
---
The limits we apply are based on our research on data operations and are set to provide best performance and user experience. If they block you for any reason then please [contact us](mailto:support@syncano.com) so that we can work on a custom solution.

1. [Data Objects](#data-objects)
2. [Files](#files)
3. [Requests](#requests)
4. [Snippet - Scripts](#snippet-scripts)
5. [Instances](#instances)
6. [Classes](#classes) 
[block:api-header]
{
  "type": "basic",
  "title": "Data Objects"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Description",
    "0-0": "Data Object size",
    "h-1": "Limit",
    "h-2": "Notes",
    "0-1": "32kB (serialized)",
    "1-0": "Data Object fields number",
    "1-1": "32 fields",
    "1-2": "16 of the Data Object fields can be indexed (you can use any type as index except TextField and FileField)",
    "2-0": "Object field name",
    "2-1": "64 max character length",
    "3-0": "TextField",
    "3-1": "32000 max character length",
    "4-0": "StringField",
    "4-1": "128 max character length",
    "5-0": "IntegerField",
    "5-1": "32-bit (signed)",
    "6-0": "FloatField",
    "6-1": "64-bit",
    "6-2": "Double precision field with up to 15 decimal digit precision"
  },
  "cols": 3,
  "rows": 7
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Files"
}
[/block]
A limit for a file size is 128MB.
[block:api-header]
{
  "type": "basic",
  "title": "Requests"
}
[/block]
Limits for the number of requests that you can make (per second).
[block:parameters]
{
  "data": {
    "h-0": "Requests number / second",
    "h-1": "Request target/source",
    "0-0": "60",
    "0-1": "Administrator with Production Plan account (paid)",
    "1-0": "15",
    "1-1": "Administrator with Builder Plan account (free)",
    "2-0": "60",
    "2-1": "Instance (either by admin or API Key usage on a Production plan)",
    "3-0": "10",
    "3-1": "Anonymus usage"
  },
  "cols": 2,
  "rows": 4
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Snippet - Scripts"
}
[/block]
There are certain timeouts that apply to Snippets run via different methods:

[block:parameters]
{
  "data": {
    "h-0": "Method",
    "0-0": "Direct Snippet Script run",
    "1-0": "Trigger Socket",
    "2-0": "Schedule Socket",
    "3-0": "Script Endpoint",
    "0-1": "300s",
    "1-1": "20s",
    "h-1": "Timeout",
    "2-1": "300s",
    "3-1": "15s"
  },
  "cols": 2,
  "rows": 4
}
[/block]
### Snippet Scripts Traces retention time

Syncano will store the last 100 of Snippet Scripts Traces. Older traces will be automatically removed 
[block:api-header]
{
  "type": "basic",
  "title": "Instances"
}
[/block]
Maximum number of Instances that an administrator can be an owner of is `16`
[block:api-header]
{
  "type": "basic",
  "title": "Classes"
}
[/block]
Maximum number of Classes that can be created within an Instance is `32`
[block:api-header]
{
  "type": "basic",
  "title": "Groups"
}
[/block]
Maximum number of Groups User can belong to is `32`