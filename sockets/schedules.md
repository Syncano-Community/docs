---
title: "Schedule Sockets"
excerpt: "What's a Schedule Socket and how does it work?"
---
## Chapter Contents:

1. [Overview](#overview)
2. [Creating a Schedule Socket from the Dashboard](#section-creating-schedule-socket-from-the-dashboard)
3. [Creating Schedule Socket with `interval_sec` parameter](#section-creating-schedule-socket-with-interval_sec-parameter)
4. [Creating Schedule Socket with a `crontab` parameter](#section-creating-schedule-socket-with-a-crontab-parameter)
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Schedule Sockets are one of the available ways of running your Snippet Scripts. Thanks to Schedules you can execute Scripts at a desired date (e.g. every Thursday at 9PM) and/or time interval (e.g. every 5 minutes). 

There are two ways of setting the desired time interval:
+ [interval_sec](#section-creating-schedule-socket-with-interval_sec-parameter)
+ [crontab](#section-creating-schedule-socket-with-a-crontab-parameter)

We'll get into the details of setting a Schedule 
in code with these two options below, but before that - we will learn how to add a schedule using our [Dashboard](https://dashboard.syncano.io).

## Creating Schedule Socket from the Dashboard

Schedules can be found under the `Tasks` tab on the Navigation Bar in Syncano's [Dashboard](https://dashboard.syncano.io).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/szUOobjQamnflkf5ztNT",
        "Add_schedule_01.png",
        "1279",
        "653",
        "#223e68",
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
        "https://www.filepicker.io/api/file/mX6AlrwkTjasErRGdUSu",
        "Add_schedule_02.png",
        "1279",
        "657",
        "#6c84a4",
        ""
      ]
    }
  ]
}
[/block]
As you can see, you have quite a few options to choose from. 
If you need even more flexibility, read further, and see what can you accomplish using  `interval_sec` and `crontab` parameters directly!

## Creating Schedule Socket with `interval_sec` parameter

`interval_sec` is simply an interval (defined in seconds) at which the Script will run. 
[block:callout]
{
  "type": "info",
  "body": "`interval_sec` has a valid range from `30` to `86400`s (from half a minute to 24 hours), so a daily Script execution is a maximum in this case. If you need more complex solution, use cron tables."
}
[/block]
So let's say we'd like our Snippet Script to run every minute. This is how a Schedule creation call would look like:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\"label\":\"every 60 sec\", \n     \"script\": script_id, \n     \"interval_sec\": 60}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/schedules/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Schedule\n\nsyncano.connect(api_key='API_KEY')\n\nSchedule.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  label=\"every 60 sec\",\n  script=\"CODEBOX_ID\",\n  interval_sec=60\n)\n",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Schedule = connection.Schedule;\n\nvar options = {\n\tlabel: \"every 60 sec\", // Schedule label\n\tscript: 4, // Script Id to run\n\tinterval_sec: \"60\", // how often (in seconds) schedule should run\n  instanceName: \"INSTANCE_NAME\"\n};\n\nSchedule.please().create(options).then(function(schedule) {\n\tconsole.log(\"schedule\", schedule);\n});",
      "language": "javascript"
    },
    {
      "code": "// N/A",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// N/A",
      "language": "objectivec"
    },
    {
      "code": "// N/A",
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
## Creating Schedule Socket with a `crontab` parameter

Crontab is a file that contains instructions that say "run this command at this time on this date". These instructions are passed in form of a cron expression which is a string comprising five fields separated by whitespace:
[block:parameters]
{
  "data": {
    "h-0": "",
    "h-1": "Field 1",
    "h-2": "Field 2",
    "h-3": "Field 3",
    "h-4": "Field 4",
    "h-5": "Field 5",
    "0-0": "Field type:",
    "0-1": "Minutes",
    "0-2": "Hours",
    "0-3": "Day of the month",
    "0-4": "Month",
    "0-5": "Day of the week",
    "1-0": "Allowed values:",
    "1-1": "0-59",
    "1-2": "0-23",
    "1-3": "1-31",
    "1-4": "1-12",
    "1-5": "0-6",
    "2-0": "Allowed special characters",
    "2-1": "`* , -`",
    "2-2": "`* , -`",
    "2-3": "`* , -`",
    "2-4": "`* , -`",
    "2-5": "`* , -`"
  },
  "cols": 6,
  "rows": 2
}
[/block]
There are couple of special characters that you can use when constructing cron expressions:
[block:parameters]
{
  "data": {
    "h-0": "Character",
    "h-1": "Usage",
    "0-0": "Comma \n`,`",
    "1-0": "Hypen  \n`-`",
    "2-0": "Asterisk \n`*`",
    "0-1": "Commas are used to separate items of a list. For example, using \"1,3,5\" in the 5th field (day of week) means Mondays, Wednesdays and Fridays.",
    "1-1": "Hyphens define ranges. For example, 12-16 in second field indicates every hour between 12:00 and 16:00, inclusive.",
    "2-1": "Stands for \"first-last\". Cron expression with `*` in first field will run every minute, with `*` in second field, every hour etc."
  },
  "cols": 2,
  "rows": 3
}
[/block]
Couple of crontab examples:
[block:parameters]
{
  "data": {
    "0-0": "`5 0 * * *`",
    "1-0": "`0 9-18 * * *`",
    "1-1": "Schedule Socket will run every day at full hour from 9:00AM to 6:00PM",
    "2-0": "`0 9-18 * * 1-5`",
    "2-1": "Schedule Socket will run during weekdays at full hour from 9:00AM to 6:00PM",
    "0-1": "Schedule Socket will run every day, five minutes after midnight"
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If you'd like to know more about crontabs, [UNIX man pages](http://man7.org/linux/man-pages/man5/crontab.5.html) are a great place to start."
}
[/block]
Putting it all together, a Schedule Socket with crontab parameter would look like this:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\"label\":\"every 60 sec\", \n     \"script\": script_id, \n     \"crontab\": \"0 9 * * *\"}' \\\n\"https://api.syncano.io/v1.1/instances/instance_name/schedules/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models import Schedule\n\nsyncano.connect(api_key='API_KEY')\n\nSchedule.please.create(\n  instance_name=\"INSTANCE_NAME\",\n  label=\"every 60 sec\",\n  script=\"SCRIPT_ID\",\n  crontab=\"0 9-18 * * *\")",
      "language": "python"
    },
    {
      "code": "var Syncano = require(\"syncano\");  // CommonJS\nvar connection = Syncano({accountKey: \"ACCOUNT_KEY\"});\nvar Schedule = connection.Schedule;\n\n\nvar options = {\n\tlabel: \"every day\", // Schedule label\n\tscript: 4, // Script Id to run\n\tcrontab: \"0 0 * * 0\", // when schedule should run (cron syntax),\n  instanceName: \"INSTANCE_NAME\"\n};\n\nSchedule.please().create(options).then(function(schedule) {\n\tconsole.log(\"schedule\", schedule);\n});",
      "language": "javascript"
    },
    {
      "code": "var syncano = new Syncano();\nsyncano.connect(\"ACCOUNT_KEY\");\nsyncano.setInstance(\"instanceName\");\n\nvar params = {\n\tlabel: \"every day\", // Schedule label\n\tcodebox: 4, // CodeBox Id to run\n\tcrontab: \"0 0 * * 0\" // when schedule should run (cron syntax)\n}\n\nsyncano.Schedules.create(params).then(function (output) {\n\tconsole.log(output);\n}, function (err) {\n\tconsole.log(\"ERR :\" + err)\n});",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "// N/A",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// N/A",
      "language": "objectivec"
    },
    {
      "code": "// N/A",
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
So in the example above a Snippet Script with an id = 4  would run every day at midnight.
[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For more details about the available methods for schedules, visit the [Schedule API reference](http://docs.syncano.com/v0.1.1/docs/schedules-list)."
}
[/block]