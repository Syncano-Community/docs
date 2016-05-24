---
title: "Backup and restore"
excerpt: ""
---
1. Overview
2. Making backups
3. Restoring data from backups.
[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Backups allow you to backup your instance state, or export some part of it to a file.
We have distinguished 2 types of backups: Full and Partial. Full backups work
like snapshots, and allow you to save your instance current state and files into
backup object. Partial backups allow you to export some of your instance data
and files into an archive, that you can download for later use.
[block:api-header]
{
  "type": "basic",
  "title": "Making backups"
}
[/block]
In syncano we distinguish between two types of backups. Full and Partial. Full
backups allow you to take a snapshot in time of your instance configuration and
data. Partial backups allow you to choose what objects you want to backup (eg.
Classes, Codeboxes, DataObjects, Users).

### Full backups

To create a Full backup you have to create a POST request to full backup endpoint
like this:

[block:code]
{
  "codes": [
    {
      "code": "   curl -X POST \\\n    -H \"X-API-KEY: <ACCOUNT_KEY>\" \\\n    -H \"Content-type: application/json\" \\\n    -d '{\"label\": \"backup label\",\n         \"description\":\"backup description\"}' \\\n         \"https://api.syncano.io/v1/instances/<INSTANCE>/backups/full/\"",
      "language": "curl"
    }
  ]
}
[/block]
where <ACCOUNT_KEY> is your account key, and <INSTANCE> is name of an instance
that you want to backup. Optionally you can send label and description values to
describe backup contents. In response you will get something like this:
[block:code]
{
  "codes": [
    {
      "code": "    {\n        \"id\": 8,\n        \"instance\": \"syncano\",\n        \"created_at\": \"2016-04-25T09:23:58.154759Z\",\n        \"updated_at\": \"2016-04-25T09:23:58.154800Z\",\n        \"size\": null,\n        \"status\": \"scheduled\",\n        \"status_info\": \"\",\n        \"description\": \"backup description\",\n        \"label\": \"backup label\",\n        \"links\": {\n            \"self\": \"/v1/backups/full/8/\"\n        }\n    }",
      "language": "json"
    }
  ]
}
[/block]
As you can see. The backup status is "scheduled". When the backup is done the
status property will change to "success". Other statuses are
    "error" - there was an error during backup, check "status_info" for details
    "running" - backup is running, creating and archive
    "uploading" - backup archive is uploaded to storage space.
You can periodically check for backup status making get request to endpoint pointed
by "links.self" attribute.

You can get a list of full instance backups at `/v1/instances/<instance_name>/backups/full/`
There is also an endpoint that will allow you to get a list of backups for all
your instances at `/v1/backups/full`.

### Partial backups

To create a Partial backup you have to create a POST request to full backup endpoint
like this:

[block:code]
{
  "codes": [
    {
      "code": "   curl -X POST \\\n    -H \"X-API-KEY: <ACCOUNT_KEY>\" \\\n    -H \"Content-type: application/json\" \\\n    -d '{\"label\": \"backup label\",\n         \"room\":\"backup descriptio\",\n         \"query_args\": {\"class\": [\"user_profile\"]}}' \\\n    \"https://api.syncano.io/v1/instances/<INSTANCE>/backups/partial/\"",
      "language": "curl"
    }
  ]
}
[/block]
where <ACCOUNT_KEY> is your account key, and <INSTANCE> is name of an instance
that you want to backup. You have to provide "query_args" parameter, that
describes what you want to backup.It's a JSON objecta where keys are what type of objects you want to
include in backup, and values can be either be null (all objects) or a list
of id-s or name-s of objects that you want to include in backup file.
If there is no key for given type or value of key is null, it means that
no filtering should be applied and all objects of this type should be included
in backup file. You can get current schema for query_args argument by issuing
OPTIONS request to partial backups endpoint.
[block:code]
{
  "codes": [
    {
      "code": "curl -X OPTIONS \\\n-H \"X-API-KEY: <ACCOUNT_KEY>\"\\ \"https://api.syncano.io/v1/instances/<instance>/backups/partial/\"",
      "language": "curl"
    }
  ]
}
[/block]
And you should get response like that below.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"name\": \"Partial Backup List\", \n    \"description\": \"\", \n    \"renders\": [\n        \"application/json\", \n        null, \n        \"text/html\"\n    ], \n    \"parses\": [\n        \"application/json\", \n        \"application/x-www-form-urlencoded\", \n        \"multipart/form-data\"\n    ], \n    \"actions\": {\n        \"POST\": {\n            \"id\": {\n                \"type\": \"integer\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"ID\"\n            }, \n            \"instance\": {\n                \"type\": \"field\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Instance\"\n            }, \n            \"created_at\": {\n                \"type\": \"datetime\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Created at\"\n            }, \n            \"updated_at\": {\n                \"type\": \"datetime\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Updated at\"\n            }, \n            \"archive\": {\n                \"type\": \"file upload\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Archive\"\n            }, \n            \"size\": {\n                \"type\": \"integer\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Size\"\n            }, \n            \"status\": {\n                \"type\": \"choice\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Status\"\n            }, \n            \"status_info\": {\n                \"type\": \"string\", \n                \"required\": false, \n                \"read_only\": true, \n                \"label\": \"Status info\"\n            }, \n            \"description\": {\n                \"type\": \"string\", \n                \"required\": false, \n                \"read_only\": false, \n                \"label\": \"Description\", \n                \"max_length\": 256\n            }, \n            \"label\": {\n                \"type\": \"string\", \n                \"required\": false, \n                \"read_only\": false, \n                \"label\": \"Label\", \n                \"max_length\": 64\n            }, \n            \"query_args\": {\n                \"type\": \"field\", \n                \"required\": true, \n                \"read_only\": false, \n                \"label\": \"Query args\", \n                \"schema\": {\n                    \"additionalProperties\": false, \n                    \"type\": \"object\", \n                    \"properties\": {\n                        \"gcm_device\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"group\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"data_object\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"script\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"script_endpoint\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"data_endpoint\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"schedule\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"apns_config\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"response_template\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"membership\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"trigger\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"user\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"apns_device\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"gcm_config\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"user_social_profile\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"class\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }, \n                        \"channel\": {\n                            \"anyOf\": [\n                                {\n                                    \"type\": \"null\"\n                                }, \n                                {\n                                    \"items\": {\n                                        \"anyOf\": [\n                                            {\n                                                \"type\": \"integer\"\n                                            }, \n                                            {\n                                                \"type\": \"string\"\n                                            }\n                                        ]\n                                    }, \n                                    \"type\": \"array\"\n                                }\n                            ]\n                        }\n                    }\n                }\n            }, \n            \"links\": {\n                \"links\": [\n                    {\n                        \"type\": \"detail\", \n                        \"name\": \"self\"\n                    }\n                ]\n            }\n        }\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
Optionally you can send label and description values to
describe backup contents. In response you should get document like one below:

[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 9,\n    \"instance\": \"INSTANCE_NAME\",\n    \"created_at\": \"2016-04-25T10:45:01.336604Z\",\n    \"updated_at\": \"2016-04-25T10:45:01.336638Z\",\n    \"archive\": null,\n    \"size\": null,\n    \"status\": \"scheduled\",\n    \"status_info\": \"\",\n    \"description\": \"\",\n    \"label\": \"\",\n    \"links\": {\n        \"self\": \"/v1/backups/partial/9/\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
When status becomes 'success', fields size and archive should be filled too.
Size field tells us how big is your backup file, and archive is an url to
file with instance data. This file can be downloaded by any user, that knows url.

You are allowed to create maximum number of 25 backups per account. Old or
uneeded backups can be deleted by issuing DELETE request on backup's endpoint.
[block:api-header]
{
  "type": "basic",
  "title": "Restore"
}
[/block]
When you have existing backup you can restore it into an instance. Restoring
Full Backup will erase all existing data from instance, and put data from
backup object. Think of it as a way to get back in time, to exact instance state
during backup creation.

You can restore from stored backup by issuing POST request to instance's restore
endpoint.
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n-H \"X-API-KEY: ACCOUNT_KEY\" \\\n-H \"Content-type: application/json\" \\\n-d '{\"backup\": 1}' \\\n\"https://api.syncano.io/v1/instances/INSTANCE_NAME/restores/\"",
      "language": "curl"
    }
  ]
}
[/block]
You can have only one restore running per instance.